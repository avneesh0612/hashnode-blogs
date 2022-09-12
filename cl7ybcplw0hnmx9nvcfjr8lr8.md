## Allow GitHub contributors to mint an NFT

In this guide, we'll show you how to create an app where users can sign in with their GitHub and web3 wallet. Once they are signed in, we will validate if they have contributed to one of our repos; and if they have, we will allow the users to mint an NFT using signature-based minting!

Before we get started, below are some helpful resources where you can learn more about the tools we're going to be using in this guide.

* [View project source code](https://github.com/avneesh0612/github-contributor-nft-rewards)
* [Edition contract](https://portal.thirdweb.com/pre-built-contracts/edition)
* [Auth](https://portal.thirdweb.com/auth)

Let's get started!

## Setup
I am going to use the [Next.js Typescript starter template](https://github.com/thirdweb-example/next-typescript-starter) for this guide.

If you are following along with the guide, you can create a project with the template using the [thirdweb CLI](https://github.com/thirdweb-dev/js/tree/main/packages/cli):

```
npx thirdweb create --next --ts
```

If you already have a Next.js app you can simply follow these steps to get started:

* Install `@thirdweb-dev/react` and `@thirdweb-dev/sdk` and `ethers`

* Add MetaMask authentication to the site. You can follow this [guide](https://portal.thirdweb.com/guides/add-connectwallet-to-your-website) to add metamask auth.
By default the network is Mainnet, we need to change it to Goerli.

```
import type { AppProps } from "next/app";
import { ChainId, ThirdwebProvider } from "@thirdweb-dev/react";

// This is the chainId your dApp will work on.
const activeChainId = ChainId.Goerli;

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ThirdwebProvider desiredChainId={activeChainId}>
      <Component {...pageProps} />
    </ThirdwebProvider>
  );
}

export default MyApp;
```

### Creating an Edition Contract and Lazy-Minting An NFT
Let's go ahead and deploy an Edition contract and add an NFT!

To do that, head to the [thirdweb dashboard](https://thirdweb.com/dashboard) and create an Edition contract!

Fill out the details and deploy the contract by clicking on `Deploy Now`.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662951517478/RdrxdAG3g.png align="left")


Once the contract is deployed, go to the NFTs tab and click on Mint. Enter the details of your NFT and set the initial supply to 0. Once you have filled out all the details, click on Mint NFT!

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662951526341/LF_4gDE4i.png align="left")


We only want users to own `1` NFT per wallet from this collection. To achieve this, we'll set our NFTs to be **soulbound**, meaning they cannot transfer it!

Head to the permissions tab and set the NFTs of this collection to be Non-Transferrable.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662951549923/URUTn2Rl5.png align="left")

## Setting up Auth
We need the user to sign in both with their wallet and with their GitHub account, to do this, we'll first set up web3 sign-in using thirdweb then use NextAuth to add GitHub authentication!

### thirdweb Auth
Firstly, we need to install the thirdweb auth package:

```
npm i @thirdweb-dev/auth # npm

yarn add @thirdweb-dev/auth # yarn
```

Now, create a file called auth.config.ts and the following:

```
import { ThirdwebAuth } from "@thirdweb-dev/auth/next";

export const { ThirdwebAuthHandler, getUser } = ThirdwebAuth({
    // Using environment variables to secure your private key is a security vulnerability.
    // Learn how to store your private key securely:
    // https://portal.thirdweb.com/sdk/set-up-the-sdk/securing-your-private-key
    process.env.ADMIN_PRIVATE_KEY!,
  // Set this to your domain to prevent signature malleability attacks.
  const domain = "http://localhost:3000";
});
```

* The domain is important since you don't want your users signing malicious signatures

* Create a new `.env.local` file and add a new variable named `PRIVATE_KEY`. Learn how to [export your private key](https://portal.thirdweb.com/guides/create-a-metamask-wallet?utm_source=hashnode&utm_medium=crosspost&utm_campaign=on-demand-pass#export-your-private-key) from your wallet. Using private keys as an env variable is vulnerable to attacks and is not the best practice. We are doing it in this guide for the sake of brevity, but we strongly recommend [using a secret manager to store your private key](https://portal.thirdweb.com/sdk/set-up-the-sdk/securing-your-private-key).


To configure the auth API, create a new folder inside `pages/api` called `thirdwebauth` and a `[...thirdweb].ts` file inside it!

Within this file, we need to export the `ThirdwebHandler` that we made in the config file previously.

```
import { ThirdwebAuthHandler } from "../../../auth.config";

export default ThirdwebAuthHandler();
```

This will catch all the API requests that go to `/api/auth/...`, using Next.js's catch-all API route syntax.

Behind the scenes, this handles the logic for three endpoints that we're going to utilize: `/login`, `/logout`, and `/user`.

Finally, inside the `_app.tsx` file, add the `authConfig` prop to `ThirdwebProvider`:
```
    <ThirdwebProvider
      desiredChainId={activeChainId}
      authConfig={{
        authUrl: "/api/thirdwebauth",
        domain: "example.org",
        loginRedirect: "/",
      }}
    >
      <Component {...pageProps} />
    </ThirdwebProvider>
```

### Next Auth with GitHub

Create a new folder called `auth` inside the `pages/api` folder and then create a file inside `auth` called `[...nextauth].ts`. Once you have done this add the following to the file:

```
import NextAuth, { NextAuthOptions } from "next-auth";
import GithubProvider from "next-auth/providers/github";

export const authOptions: NextAuthOptions = {
  providers: [
    GithubProvider({
      clientId: process.env.GITHUB_ID as string,
      clientSecret: process.env.GITHUB_SECRET as string,
    }),
  ],

  callbacks: {
    async session({ session, token }) {
      session.id = token.sub;
      return session;
    },
  },
};

export default NextAuth(authOptions);
```

Now, go to `_app.tsx` and wrap the component in a session provider like this:

```
<SessionProvider session={pageProps.session} refetchInterval={0}>
        <Component {...pageProps} />
</SessionProvider>
```

The `SessionProvider` will be imported from next-auth like this:

```
import { SessionProvider } from "next-auth/react";
```

We need to add some env variables to make the auth work! In the `.env.local` file add these variables:

```
GITHUB_ID=
GITHUB_SECRET=
NEXTAUTH_URL=
NEXTAUTH_SECRET=
```

The `NEXTAUTH_URL` will be a link to the homepage of your website, since we are working on localhost it will be `http://localhost:3000`.

To generate a secret run the following command:

```
openssl rand -base64 32               
```
      
Now, we need to create a GitHub OAuth app. You can do that from the [Github Developer Settings](https://github.com/settings/applications/new) and fill out the information like so:

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662951831291/TWEwr9dDp.png align="left")

Now, copy the client id and generate a new secret and copy that. Go to your `.env.local` file and update these variables. Once you have filled all the env vars restart your dev server!

Next, follow Github's [Creating a personal access token](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token) guide and add this as an environment variable in your project. We'll use this to make requests to GitHub's API without getting rate-limited.

```
GITHUB_ACCESS_TOKEN=
```

## Creating the login page
Create a new file `login.tsx` inside the `pages` folder and add the following:

```
import React from "react";
import { useSession, signIn } from "next-auth/react";
import styles from "../styles/Home.module.css";

const Login = () => {
  const { data: session } = useSession();

  if (!session) {
    return (
      <div className={styles.container}>
        <button className={styles.mainButton} onClick={() => signIn()}>
          Sign in with GitHub
        </button>
      </div>
    );
  }
};

export default Login;
```

This will show a Sign in with GitHub button if there is no session detected from NextAuth.

Now, we will check for the address and thirdweb user. If the user's wallet is not connected/the user isn't signed in we will show the corresponding buttons.

Firstly, we are going to need 3 hooks:
```
  const login = useLogin();
  const address = useAddress();
  const connect = useMetamask();
```

Below the session check add the following:

```
  if (!address) {
    return (
      <div className={styles.container}>
        <button className={styles.mainButton} onClick={() => connect()}>
          Connect Wallet
        </button>
      </div>
    );
  }

  return (
    <div className={styles.container}>
      <button className={styles.mainButton} onClick={() => login()}>
        Sign in with Ethereum
      </button>
    </div>
  );
```

This code block will first check if there is an address present. If there is no address present it will show the `connect wallet` button; otherwise, the `sign in with Ethereum` button.

We are using css modules to add some basic stylings to the app, so if you want to add the stylings as well. Create a new folder `styles` and `globals.css` inside it

```
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen, Ubuntu,
    Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}

a {
  color: inherit;
  text-decoration: none;
}

* {
  box-sizing: border-box;
}
```

Now, import it in _app.tsx like this:

```
import "../styles/globals.css";
```

Now, create another file called Home.module.css and the following:

```
.container {
  width: 100%;
  height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.mainButton {
  background: linear-gradient(to right, #ff1b6b, #45caff);
  border: none;
  color: #fff;
  font-weight: 700;
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 5px;
}
```

Now, we have the auth flow completed! You can go to [http://localhost:3000/login](http://localhost:3000/login) and check it out yourself.

## Creating the API for claim
Create a new file `claim-nft.ts` inside the `pages/api` folder. We are going to need to create an api so add the following:

```
import type { NextApiRequest, NextApiResponse } from "next";

const claimNft = async (req: NextApiRequest, res: NextApiResponse) => {
  if (req.method !== "POST") {
    return res.status(405).json({ error: "Method not allowed" });
  }

  return res.status(200).json({ message: "gm" });
};

export default claimNft;
```

This api first checks if the request is of the post method or not and if it is using the post method we will send a message "gm".

Now, let's get into the actual code! We will first get the users from the next auth and thirdweb auth like this:

```
  const session = await unstable_getServerSession(req, res, authOptions);
  const thirdwebUser = await getUser(req);

  if (!session || !thirdwebUser) {
    return res.status(401).json({ message: "Unauthorized" });
  }
```

We need to import them like this:

```
import { unstable_getServerSession } from "next-auth/next";
import { getUser } from "../../auth.config";
```
These are the two functions we use to view the authenticated user's information on the server-side:

* `unstable_getServerSession` is our way of getting the connected Github account
* `getUser` is our way of viewing the authenticated wallet.
Now, we will create a new array that will have a list of repos to check for:

```
const reposToCheck = [
    "js",
    "react",
    "typescript-sdk",
    "thirdweb-cli",
    "contracts",
    "auth",
    "storage",
    "go-sdk",
    "python-sdk",
    "docs",
    "portal",
    "examples",
  ];
```
Here, we have added some of our thirdweb repositories, but you need to update this with your list of repositories to check.

Now, we will loop through this array and for each item we will check if the user has contributed to them:

```
let hasContributed = false;

  for (const repo of reposToCheck) {
    const contributors: GithubContributor[] = await fetch(
      `https://api.github.com/repos/thirdweb-dev/${repo}/contributors`,
      {
        headers: {
          Authorization: `token ${process.env.GITHUB_ACCESS_TOKEN}`,
        },
      }
    ).then((res) => res.json());

    const hasContributedToThisRepo = contributors?.some((contributor) => {
      return contributor.id.toString() === session.id;
    });

    if (hasContributedToThisRepo) {
      hasContributed = true;
    }
  }
```
Now that we know if the user has contributed or not, if the user has not contributed we will simply say that you don't qualify:

```
  if (!hasContributed) {
    return res.status(401).json({ message: "Sorry, you don't qualify" });
  }
```

Now, let's initialize our SDK using our private key:

```
 const sdk = ThirdwebSDK.fromPrivateKey(
     // Using environment variables to secure your private key is a security vulnerability.
    // Learn how to store your private key securely:
    // https://portal.thirdweb.com/sdk/set-up-the-sdk/securing-your-private-key
    process.env.PRIVATE_KEY as string,
    "goerli"
  );
```

We will first check if the user already owns one of our NFTs first:

```
  const edition = sdk.getEdition("0xD71c27e6325f018b15E16C3992654F1b089C5fCe");

  const balance = await edition.balanceOf(thirdwebUser.address, 0);

  if (balance.gt(0)) {
    return res.status(401).json({ message: "You already have an NFT" });
  }
```

Finally, we will generate the signature to mint the NFT!

```
  const signedPayload = await edition.signature.generateFromTokenId({
    quantity: 1,
    tokenId: 0,
    to: thirdwebUser.address,
  });

  return res.status(200).json({ signedPayload });
```

Our final API code will looks like this:

```
import type { NextApiRequest, NextApiResponse } from "next";
import { unstable_getServerSession } from "next-auth/next";
import { getUser } from "../../auth.config";
import GithubContributor from "../../types/GithubContributor";
import { ThirdwebSDK } from "@thirdweb-dev/sdk";
import { authOptions } from "./auth/[...nextauth]";

const claimNft = async (req: NextApiRequest, res: NextApiResponse) => {
  if (req.method !== "POST") {
    return res.status(405).json({ error: "Method not allowed" });
  }

  const session = await unstable_getServerSession(req, res, authOptions);
  const thirdwebUser = await getUser(req);

  if (!session || !thirdwebUser) {
    return res.status(401).json({ message: "Unauthorized" });
  }

  const reposToCheck = [
    "js",
    "react",
    "typescript-sdk",
    "thirdweb-cli",
    "contracts",
    "auth",
    "storage",
    "go-sdk",
    "python-sdk",
    "docs",
    "portal",
    "examples",
  ];

  let hasContributed = false;

  for (const repo of reposToCheck) {
    const contributors: GithubContributor[] = await fetch(
      `https://api.github.com/repos/thirdweb-dev/${repo}/contributors`,
      {
        headers: {
          Authorization: `token ${process.env.GITHUB_ACCESS_TOKEN}`,
        },
      }
    ).then((res) => res.json());

    const hasContributedToThisRepo = contributors?.some((contributor) => {
      return contributor.id.toString() === session.id;
    });

    if (hasContributedToThisRepo) {
      hasContributed = true;
    }
  }

  if (!hasContributed) {
    return res.status(401).json({ message: "Sorry, you don't qualify" });
  }

  const sdk = ThirdwebSDK.fromPrivateKey(
    process.env.PRIVATE_KEY as string,
    "goerli"
  );

  const edition = sdk.getEdition("0xD71c27e6325f018b15E16C3992654F1b089C5fCe");

  const balance = await edition.balanceOf(thirdwebUser.address, 0);

  if (balance.gt(0)) {
    return res.status(401).json({ message: "You already have an NFT" });
  }

  const signedPayload = await edition.signature.generateFromTokenId({
    quantity: 1,
    tokenId: 0,
    to: thirdwebUser.address,
  });

  return res.status(200).json({ signedPayload });
};

export default claimNft;
```

## Creating the frontend
### Calling the API on the frontend

Go to `pages/index.tsx` and update the return block:

```
  return (
    <div className={styles.container}>
      <h1>GitHub Contributor NFTs</h1>
      <p>
        Claim an NFT if you have contributed to any of thirdweb&apos;s repos.
      </p>

      {!address ? (
        <button
          className={styles.mainButton}
          disabled={loading}
          onClick={() => connect()}
        >
          Connect to Metamask
        </button>
      ) : (
        <button
          className={styles.mainButton}
          disabled={loading}
          onClick={mintNft}
        >
          {loading ? "Loading..." : "Claim NFT"}
        </button>
      )}
    </div>
  );
```

We are going to check if the user's wallet is connected and if it is then we will show a button to claim the NFT!

As you can see we are going to need a `mintNFT`function so let's create that:

```
const mintNft = async () => {
    setLoading(true);

    if (networkMismatch) {
      return switchNetwork?.(ChainId.Goerli);
    }

    try {
      const req = await fetch("/api/claim-nft", {
        method: "POST",
      });

      const res = await req.json();

      if (!req.ok) {
        return alert(`Error: ${res.message}`);
      }

      await edition?.signature.mint(res.signedPayload);

      alert("Successfully minted NFT 🚀");
    } catch (err) {
      console.error(err);
      alert("Failed to mint NFT");
    } finally {
      setLoading(false);
    }
  };
```
We are also using some hooks to access the edition contract, get the user's address, connect the wallet to MetaMask, store loading, etc.:

```
const edition = useEdition("0xD71c27e6325f018b15E16C3992654F1b089C5fCe");
  const connect = useMetamask();
  const address = useAddress();
  const [, switchNetwork] = useNetwork();
  const networkMismatch = useNetworkMismatch();
  const [loading, setLoading] = useState<boolean>(false);
```

Now, if you have contributed to any of the repositories you will successfully be able to mint the NFT! 🎉

### Redirecting users if they are not logged in
If the user is not logged in and they come to the home page we should redirect them to `/login` so, we are going to use SSR to redirect the user if they are not logged in:

```
export const getServerSideProps: GetServerSideProps = async (context) => {
  const thirdwebUser = await getUser(context.req);

  const session = await unstable_getServerSession(
    context.req,
    context.res,
    authOptions
  );

  if (!thirdwebUser || !session) {
    return {
      redirect: {
        destination: "/login",
        permanent: false,
      },
    };
  }

  return {
    props: {},
  };
};
```

## Conclusion
In this guide, we learnt how to use thirdweb auth to allow only users who have contributed to our repos to mint an NFT!

If you did as well pat yourself on the back and share it with us on the [thirdweb discord](https://discord.gg/thirdweb)! If you want to take a look at the code, check out the [GitHub Repository](https://github.com/thirdweb-example/github-contributor-nft-rewards).



