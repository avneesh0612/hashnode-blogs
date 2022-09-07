## How to add web3 sign-in with thirdweb ✨

## Introduction

### Why use web3 sign-in?

Sign-in with Ethereum allows you to securely log in using a wallet and verify the wallet on the backend! We are going to use Thirdweb Auth which uses the very popular JWT standard! JSON Web Token (JWT) is an open standard that defines a compact and self-contained way for securely transmitting information between parties as a JSON object.

## Setup
### Creating Next.js App

I am going to use the [Next typescript starter template](https://github.com/thirdweb-example/next-typescript-starter) for this guide.

If you are following along with the guide, you can create a project with the
[Next TypeScript template](https://github.com/thirdweb-example/next-typescript-starter) using the [thirdweb CLI](https://portal.thirdweb.com/cli?utm_source=hashnode&utm_medium=crosspost&utm_campaign=on-demand-pass):

```
npx thirdweb create --next --ts
```

If you already have a Next.js app you can simply follow these steps to get started:

- Install `@thirdweb-dev/react` and `@thirdweb-dev/sdk` and `ethers`
- Add MetaMask authentication to the site. You can follow this [guide](https://portal.thirdweb.com/guides/add-connectwallet-to-your-website?utm_source=hashnode&utm_medium=crosspost&utm_campaign=tw-auth-next) to do this.


### Setting up thirdweb auth
Firstly, we need to install the thirdweb auth package:

```
npm i @thirdweb-dev/auth # npm

yarn add @thirdweb-dev/auth # yarn
```


Now, create a file called `auth.config.ts` and the following:

```
import { ThirdwebAuth } from "@thirdweb-dev/auth/next";

export const { ThirdwebAuthHandler, getUser } = ThirdwebAuth({
  privateKey: process.env.PRIVATE_KEY as string,
  domain: "example.org",
});
```

Update the domain with your website url and for the private key, create a new `.env.local` file and add a new variable named `PRIVATE_KEY`. Learn how to [export your private key](https://portal.thirdweb.com/guides/create-a-metamask-wallet?utm_source=hashnode&utm_medium=crosspost&utm_campaign=on-demand-pass#export-your-private-key) from your wallet.


To configure the auth api, create a new folder inside `pages/api` called auth and `[...thirdweb].ts` file inside it! Here we need to export the thirdwebHandler that we created! 

```
import { ThirdwebAuthHandler } from "../../../auth.config";

export default ThirdwebAuthHandler();
```

Finally, inside the `_app.tsx` file, add the authConfig prop to `ThirdwebProvider`:

```
  <ThirdwebProvider
      desiredChainId={activeChainId}
      authConfig={{
        authUrl: "/api/auth",
        domain: "example.org",
        loginRedirect: "/",
      }}
    >
      <Component {...pageProps} />
    </ThirdwebProvider>
```

## Building the frontend

Inside `pages/index.tsx` update the return statement with the following:

```
return (
    <div>
      {address ? (
        <button onClick={() => login()}>
          Sign in with Ethereum
        </button>
      ) : (
        <ConnectWallet />
      )}
    </div>
  );
```

We are going to use the `useAddress` and `useLogin` hooks to get the login function and user address:

```
  const address = useAddress();
  const login = useLogin();
```

This will add a sign-in with Ethereum to our site! Now we need to check if a user exists, so for that get the user from the `useUser` hook like this:

```
  const user = useUser();
```

And we will check if the user exists and if it exists we will return this:

```
if (user.user) {
  return (
    <div>
      <p>You are signed in as {user.user.address}</p>
      <button>Validate user</button>
    </div>
  );
}
```

## Creating api for validation
Let's now create an api to get the user details (address) on the backend! So, create a new file called `validate.ts` inside `pages/api` and add the following:


```
import type { NextApiRequest, NextApiResponse } from "next";
import { getUser } from "../../auth.config";

const handler = async (req: NextApiRequest, res: NextApiResponse) => {
  if (req.method === "POST") {
    const thirdwebUser = await getUser(req);

    if (thirdwebUser?.address) {
      return res.status(200).json({
        message: `You are signed in as ${thirdwebUser.address}`,
      });
    }
    return res.status(401).json({ message: "Account not validated" });
  }
  return res.status(405).json({ message: "Method not allowed" });
};

export default handler;
```

Here, we are using the getUser method from thirdweb to get the user's address and if it exists we send a message that "you are signed in as address".

## Calling the API on frontend

Create a new function called `handleClick` in `pages/index.tsx` like this:

```
  const handleClick = async () => {
    try {
      const response = await fetch("/api/validate", {
        method: "POST",
      });

      const data = await response.json();
      alert(data.message);
    } catch (error) {
      console.log(error);
    }
  };
```

And attach this function to onClick of the validate button:

```
<button onClick={handleClick}>Validate user</button>
```

%[https://www.loom.com/share/7f637c53db6b4cf6a380ff8e113bf765]

## Conclusion
Hope you learnt how to add web3 sign-in to your amazing Dapps via this guide!


### Useful links

[GitHub repo](https://github.com/avneesh0612/thirdweb-auth-demo)

[Thirdweb Auth](https://thirdweb.com/auth)

