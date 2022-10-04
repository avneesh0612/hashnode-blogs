## How to support multiple chains in your thirdweb Dapp

This guide will show you how to create a Dapp that works on multiple chains. In this Dapp we are going to allow users to select chains and claim an NFT from the respective chain. I am going to show goerli and Mumbai but you could do it on any network and go crazy with it.

Before we get started, below are some helpful resources where you can learn more about the tools we will use in this guide.

* [View project source code](https://github.com/thirdweb-dev/examples/tree/main/typescript/multi-chain-dapp)
* [React SDK](https://portal.thirdweb.com/react)

Let's get started.

## Setup

### Setting up our project

I am going to use the [Next.js Typescript starter template](https://github.com/thirdweb-example/next-typescript-starter) for this guide.

If you are following along with the guide, you can create a project with the template using the [thirdweb CLI](https://github.com/thirdweb-dev/js/tree/main/packages/cli):

```
npx thirdweb create --next --ts
```

If you already have a Next.js app you can simply follow these steps to get started:

* Install `@thirdweb-dev/react` and `@thirdweb-dev/sdk` and `ethers`.

* Add MetaMask authentication to the site. You can follow this [guide](https://portal.thirdweb.com/guides/add-connectwallet-to-your-website) to add metamask auth.


## Getting into the code

### Creating a context to store chain

Create a new file called `Context/Chain.ts` and add the following:

```
import { ChainId } from "@thirdweb-dev/sdk";
import { createContext } from "react";

const ChainContext = createContext({
  selectedChain: ChainId.Mainnet,
  setSelectedChain: (chain: ChainId) => {},
});

export default ChainContext;
```

This will allow us to create a global state for storing and changing our chainId.


Now, head over to `_app.tsx` and replait ce with the following:

```
import { ChainId } from "@thirdweb-dev/sdk";
import type { AppProps } from "next/app";
import { useState } from "react";
import ChainContext from "../context/Chain";
import { ThirdwebProvider } from "@thirdweb-dev/react";

function MyApp({ Component, pageProps }: AppProps) {
  const [selectedChain, setSelectedChain] = useState(ChainId.Mumbai);

  return (
    <ChainContext.Provider value={{ selectedChain, setSelectedChain }}>
      <ThirdwebProvider desiredChainId={selectedChain}>
        <Component {...pageProps} />
      </ThirdwebProvider>
    </ChainContext.Provider>
  );
}

export default MyApp;
```


We are adding the react context here, so we can access the state everywhere!

### Creating dropdown for networks

We will create a simple dropdown for users to switch between networks. So, in `pages/index.tsx` add this select element that will create a dropdown:

```
 <select
        value={String(selectedChain)}
        onChange={(e) => setSelectedChain(parseInt(e.target.value))}
      >
        <option value={String(ChainId.Mumbai)}>Mumbai</option>
        <option value={String(ChainId.Goerli)}>Goerli</option>
      </select>
``` 

We need to access the react context state that we just created, so we will use the `useContext` hook to do this:

```
  const { selectedChain, setSelectedChain } = useContext(ChainContext);
```


We also need to import these:

```
import { ChainId } from "@thirdweb-dev/react";
import ChainContext from "../context/Chain";
```

Now, we will create a JSON object that will have the addresses of the contract addresses:

```
 const addresses = {
    [String(ChainId.Mumbai)]: "0x25CB5C350bD3062bEaE7458805Fb069200e37fD5",
    [String(ChainId.Goerli)]: "0xA72234a2b9c1601593062f333D739C93291dF49F",
  };
```

This is creating a string key of the chainId with the contract address of the drops we created as values.

We will use the `Web3Component` for performing functions. Since I created an NFT Drop I am calling the call function but it might be different for you!

```ts
  <div style={{ maxWidth: "200px" }}>
        <Web3Button
          contractAddress={addresses[String(selectedChain)]}
          action={(contract) => contract.erc721.claim(1)}
        >
          Claim
        </Web3Button>
      </div>
```


If we go to [localhost](http://localhost:3000/) and check out the app, we can switch between networks and it all works completely fine! 🎉


## Conclusion
This guide taught us how to allow users to claim NFTs from different NFTs in the same Dapp using the react sdk. If you have any queries hop on the [thirdweb discord](https://discord.gg/thirdweb)! If you want to take a look at the code, check out the [GitHub Repository](https://github.com/thirdweb-dev/examples/tree/main/typescript/multi-chain-dapp).
