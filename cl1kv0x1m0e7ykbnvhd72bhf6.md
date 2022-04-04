## Create a gasless NFT drop with Next.js, OpenZeppelin, and thirdweb

## What is thirdweb?

[thirdweb](https://thirdweb.com/?utm_source=hashnode&utm_medium=crosspost&utm_campaign=create_gasless_nft_drop) is a platform that lets you deploy smart contracts without having to know Solidity, you can do it by using TypeScript, Python or Go or even without writing any code.

## Introduction
In some cases, you might wanna pay the gas for NFT drops like early access drops or when you are accepting payments in some other forms. So in this guide, we are going to see how to create a gasless NFT drop that everyone can claim. I am going to use an ERC-1155 so multiple people can hold the same NFT but you can do it any way you like! If you want to directly jump into the code, here is the finished [code](https://github.com/thirdweb-dev/examples/tree/main/javascript/gasless-nft-drop).

## Creating an NFT drop

Go to the [thirdweb dashboard](https://thirdweb.com/dashboard?utm_source=hashnode&utm_medium=crosspost&utm_campaign=create_gasless_nft_drop) dashboard and click on create a new contract. Select the chain you are going to build upon. I am going to use Mumbai for this guide. Select “Release a drop”

![Release a drop](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/release-a-drop.png)

And select Edition drop. If you want to drop unique NFTs you can select a normal drop

Fill out the details and hit deploy!

![New Edition drop contract](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/new-edition-drop-contract.png)

After the drop is created, create a new NFT inside of the drop like this-

![New Editon drop](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/new-edition-drop.png)

Finally, let’s add a claim phase! Click on the settings button for the token, update the details based on what you need, and hit update. You will be asked to confirm a small transaction so confirm that.

![Claim Phase](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/claim-phase.png)

## Building the website

We will now build a website where users will be able to come and claim the NFT!

### Setup

Create a new Next.js app-

```bash
npx create-next-app gasless-drop
```

Install the dependencies we need-

```bash
npm i @thirdweb-dev/react @thirdweb-dev/sdk ethers # npm

yarn add @thirdweb-dev/react @thirdweb-dev/sdk ethers # yarn
```

### Adding metamask auth

We are also going to need authentication in the app because users will need to sign a message as well!

Wrap the whole app in the `ThirdwebProvider` like the following:

```jsx
import '../styles/globals.css';
import { ThirdwebProvider, ChainId } from '@thirdweb-dev/react';

function MyApp({ Component, pageProps }) {
    return (
        <ThirdwebProvider desiredChainId={ChainId.Mumbai}>
            <Component {...pageProps} />
        </ThirdwebProvider>
    );
}

export default MyApp;
```

The desiredChainId is the chain id that you wanna build upon. You can use the `ChainId` object provided by the sdk to pass it in the provider, in this case, Mumbai.

To add authentication, we are going to use the `useMetamask` hook that thirdweb provides, so add the hook like the following-

```jsx
const connectWithMetamask = useMetamask();
```

Import it from the package we installed-

```jsx
import { useMetamask } from '@thirdweb-dev/react';
```

Finally, let’s create a button and use it-

```jsx
<button onClick={connectWithMetamask}>Sign in with metamask</button>
```

Now that we can authenticate users on our site, let’s check if the user is authenticated and show a mint button if a user is there-

thirdweb provides another hook to get the user’s wallet address, so get the address from the hook-

```jsx
const address = useAddress();
```

The `useAddress` hook will be imported from “@thirdweb-dev/react” as well-

```jsx
import { useMetamask, useAddress } from '@thirdweb-dev/react';
```

Now, if the user is authenticated then show their address otherwise the sign-in button like this-

```jsx
<div className={styles.container}>
    {address ? (
        <h2>You are signed in as {address}</h2>
    ) : (
        <button onClick={connectWithMetamask}>Sign in with metamask</button>
    )}
</div>
```

Once, you sign in it should now show you your address! 🔥

![Signed in as address](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/signed-in-address.png)

## Building the mint functionality

We will use the `useEditionDrop` for accessing the edition and calling functions with it-

```jsx
const editionDrop = useEditionDrop('EDITION_DROP_ADDRESS');
```

Copy the edition drop address from the dashboard and replace it with `EDITION_ADDRESS`

![Editon drop address](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/edition-drop-address.png)

Once again, import the hook-

```jsx
import { useMetamask, useAddress, useEditionDrop } from '@thirdweb-dev/react';
```

We will create a new function to claim the NFT-

```jsx
const tokenId = 0;
const quantity = 1;

const claimNFT = async () => {
    try {
        await editionDrop?.claimTo(address, tokenId, quantity);
        console.log('🎉 NFT claimed successfully!');
    } catch (err) {
        console.log('💩 Error claiming NFT: ', err);
    }
};
```

The edition drop has a method called `claimTo` that we can use to claim the NFT to a specific wallet address. I have created two variables for the tokenId and quantity for better readability of code. The second arg is tokenId and the third is quantity as it can be seen.

Let’s now try minting the NFT!

%[https://www.loom.com/share/7a1d9a37c90e4829abe7887b490ab006]

And the minting works! But we have to pay for the gas in the transaction, so let’s relay the gas so that we pay the gas and user can mint their NFTs for free!

## Making transactions gasless

thirdweb supports [Biconomy](https://www.biconomy.io/) and OpenZeppelin for relaying the gas. For this demo we are going to use [OpenZeppelin](https://defender.openzeppelin.com/) to relay the gas, so head over to OpenZeppelin and sign up for an account if you haven’t already.

Create a new relayer in OpenZeppelin:

![Create new Relayer](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/create-new-relayer.png)

Choose the network that you are building upon and give the relayer a name!

After the relayer has been created, copy the address and transfer some funds into it in the network you created the relayer.

You should be able to see the amount you transferred in your dashboard

![Relayer Dashboard](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/relayer-dashboard.png)

Now go ahead and create a new autotask, you will be asked to fill some data-

![Create Autotask](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/create-autotask.png)

You can get the code from [here](https://raw.githubusercontent.com/thirdweb-dev/ozdefender-autotask/main/src/forwarder_handler.js) and paste it into the code section of autotask. After you are done hit create. Once it is done creating, copy the webhook Uri

![Copy webhook uri](https://portal.thirdweb.com/assets/portal/guides/create-gasless-nft-drop/copy-webhook-uri.png)

Head back to the Next.js application, and create a new `.env.local` file in the root of the folder. We will use this to store our environment variables! Create a new variable `NEXT_PUBLIC_OPENZEPPELIN_URL` like this-

```jsx
NEXT_PUBLIC_OPENZEPPELIN_URL=https://api.defender.openzeppelin.com/autotasks/secret-stuff
```

Since we have changed env variables, you need to restart your development server.

Finally, add the OpenZeppelin Uri in the `ThirdwebProvider`-

```jsx
<ThirdwebProvider
  sdkOptions={{
    gasless: {
      openzeppelin: {
        relayerUrl: process.env.NEXT_PUBLIC_OPENZEPPELIN_URL,
      },
    },
  }}
  desiredChainId={desiredChainId}
>
```

Now the gas fees is being relayed and the users don’t have to pay any gas!

%[https://www.loom.com/share/0b1a961b538146678c29a1fa26f17799]

Want to have a look at the code? [We got you!](https://github.com/thirdweb-dev/examples/tree/main/javascript/gasless-nft-drop)
