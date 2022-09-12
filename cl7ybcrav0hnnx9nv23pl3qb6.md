## Create a todo app with thirdweb deploy and Next.js

This guide will show you how to build a full web3 application that allows users to create an on-chain to-do list, using Solidity for the smart contract and Next.js for the application.

Before we get started, below are some helpful resources where you can learn more about the tools we're going to be using in this guide.

*   [Full project source code](https://github.com/thirdweb-example/todo-list)
*   [Release](https://portal.thirdweb.com/release) & [Deploy](https://portal.thirdweb.com/deploy)

Let's get started!

## Creating the Smart Contract

To build the smart contract we will be using Hardhat.

Hardhat is an Ethereum development environment and framework designed for full stack development in Solidity. Simply put, you can write your smart contract, deploy it, run tests, and debug your code.

### Setting up a new hardhat project

Create a folder where the hardhat project and the Next.js app will go. To create a folder, open up your terminal and execute these commands

```bash
mkdir todo-dapp
cd todo-dapp
```

Now, we will use the thirdweb CLI to generate a new hardhat project! So, run this command:

```
npx thirdweb create --contract
```

When it asks for what type of project, you need to select an empty project!

Now you have a hardhat project ready to go!

### Writing the smart contract

Once the app is created, create a new file inside the contracts directory called `Todo.sol` and add the following code:

```
// SPDX-License-Identifier: MIT

// specify the solidity version here
pragma solidity ^0.8.0;

contract Todos {
    // We will declare an array of strings called todos
    string[] public todos;

    // We will take _todo as input and push it inside the array in this function
    function setTodo(string memory _todo) public {
        todos.push(_todo);
    }

    // In this function we are just returning the array
    function getTodo() public view returns (string[] memory) {
        return todos;
    }

    // Here we are returning the length of the todos array
    function getTodosLength() public view returns (uint) {
        uint todosLength = todos.length;
        return todosLength;
    }

    // We are using the pop method to remove a todo from the array as you can see we are just removing one index
    function deleteToDo(uint _index) public {
        require(_index < todos.length, "This todo index does not exist.");
        todos[_index] = todos[getTodosLength() - 1];
        todos.pop();
    }
}
```
This smart contract allows you to add todos to your contract, remove them, get their length and return the array which consists of all the tasks you have set up.

Now that we have written our basic `Todos` smart contract, we will go ahead and deploy our contract using [deploy](https://portal.thirdweb.com/deploy).

### Deploying the contract

```
npx thirdweb deploy
```

This command allows you to avoid the painful process of setting up your entire project like setting up RPC URLs, exporting private keys, and writing scripts.

Upon success, a new tab will automatically open and you should be able to see a link to the dashboard in your CLI.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950728342/gyi2Ncb6E.png align="left")

Now, choose the network you want to deploy your contract to! I am going to use Goerli but you can use whichever one you like. Once you have chosen your network click on `Deploy now!`

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950749737/MnEbNy4e9.png align="left")

After the transactions are mined you will be taken to the dashboard which consists of many options.

*   In the **overview** section, you can explore your contract and interact with the functions without having to integrate them within your frontend code yet so it gives you a better idea of how your functions are working and also acts as a good testing environment.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950763730/N7VfzapNu.png align="left")
![](\"__GHOST_URL__/content/images/2022/09/image-15.png\")

*   In the **code** section, you see the different languages and ways you can interact with your contract. Which we will look into later on in the tutorial.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950773074/p8tPFb8IL.png align="left")

*   In the **events** section, you can see all the transactions you make.
*   You can also customize the ****settings**** after enabling the required interfaces in the settings section.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950781456/P7KkWsUJW.png align="left")
[](\"__GHOST_URL__/content/images/2022/09/image-18.png\")

*   In the **source** section, you can see your smart contract and it also gives you a verification button to the relevant chain to which you have deployed your contract.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950801089/--qY0EUby.png align="left")

## Creating the Frontend

I am going to use the [Next.js Typescript starter template](https://github.com/thirdweb-example/next-typescript-starter) for this guide.

If you are following along with the guide, you can create a project with the template using the [thirdweb CLI](\"https://github.com/thirdweb-dev/js/tree/main/packages/cli\"):

    npx thirdweb create --next --ts

If you already have a Next.js app you can simply follow these steps to get started:

*   Install `@thirdweb-dev/react` and `@thirdweb-dev/sdk` and `ethers`
*   Add MetaMask authentication to the site. You can follow this [guide](\"https://portal.thirdweb.com/guides/add-connectwallet-to-your-website\") to do this.

By default the network is Mainnet, we need to change it to Goerli

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

### Create todo functionality

Let's now go to `pages/index.tsx` and firstly, get the wallet address of the user like this:

```
const address = useAddress();
```

Then, we are going to check if the address exists and if it does we are going to show a simple input and button to create a new todo:

```
<div>
  {address ? (
    <div>
      <input
        value={input}
        onChange={(e) => setInput(e.target.value)}
        placeholder="Enter todo"
      />

      <Web3Button
        contractAddress={contractAddress}
        action={(contract) => contract.call("setTodo", input)}
      >
        Set Todo
      </Web3Button>
    </div>
  ) : (
    <ConnectWallet  />
  )}
</div>
```

We are using a state to store the value of a state variable and I have also created a variable for `contractAddress` as it will be used in multiple places:

```
  const contractAddress = "0x80ddA9989F272BFB1c53c1A100ff118Fd27dDb59";
  const [input, setInput] = useState("");
```

To get the contract address go to your contract on thirdweb and copy the contract

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950890293/G6rOmEoXB.png align="left")

If you now check out the app, you will be able to add todos!


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950896804/QrLid3xt-.png align="left")


### Read todos functionality

Using the `useContract` and `useContractData` hooks we will get the todos like this:

```
const { contract } = useContract(contractAddress);
const { data, isLoading } = useContractData(contract, "getTodo");
 ```

Now, we will check if the todos are loading and if it is loading we will show a loading screen otherwise we will show the todos:

```
  <div>
            {isLoading ? (
              "Loading..."
            ) : (
              <ul>
                {data.map((item: string, index: number) => (
                  <li key={index}>
                    {item}
                  </li>
                ))}
              </ul>
            )}
          </div>
```

You will now be able to see all the todos 🎉


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662950933100/wm7PBSweX.png align="left")

### Adding delete functionality

Where we are mapping through all the todos we will add another `Web3Button` that will be responsible for deleting the todos:

```
 {data.map((item: string, index: number) => (
                  <li key={index}>
                    {item}
                    <Web3Button
                      contractAddress={contractAddress}
                      action={(contract) => contract.call("deleteToDo", index)}
                    >
                      Delete Todo
                    </Web3Button>
                  </li>
                ))}
```

### Adding some styles

We will add some basic styles to our app so it looks good! So, create a `globals.css` file in the `styles` folder and add the following:

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
  
Now, import it in `_app.tsx` like this:

```
import "../styles/globals.css";
```

Let's style the home page now! Create a file `Home.module.css` and add the following:

```ts
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  width: 100vw;
  min-height: 100vh;
  background-color: #c0ffee;
}

.todo {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  margin-top: 10px;
}

.todo > span > button {
  border: none;
  height: 30px !important;
}

.todoForm {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 10px;
  gap: 10px;
}

.todoForm > span > button {
  border: none;
  height: 20px !important;
}
```

  
Finally, add the classNames:

```
import {
  ConnectWallet,
  useAddress,
  useContract,
  useContractData,
  Web3Button,
} from "@thirdweb-dev/react";
import type { NextPage } from "next";
import { useState } from "react";
import styles from "../styles/Home.module.css";

const Home: NextPage = () => {
  const address = useAddress();
  const contractAddress = "0x80ddA9989F272BFB1c53c1A100ff118Fd27dDb59";
  const [input, setInput] = useState("");
  const { contract } = useContract(contractAddress);
  const { data, isLoading } = useContractData(contract, "getTodo");

  return (
    <div className={styles.container}>
      {address ? (
        <>
          <div className={styles.todoForm}>
            <input
              value={input}
              onChange={(e) => setInput(e.target.value)}
              placeholder="Enter todo"
            />

            <Web3Button
              contractAddress={contractAddress}
              action={(contract) => contract.call("setTodo", input)}
              accentColor="#1ce"
            >
              Set Todo
            </Web3Button>
          </div>

          <div>
            {isLoading ? (
              "Loading..."
            ) : (
              <ul>
                {data.map((item: string, index: number) => (
                  <li key={index} className={styles.todo}>
                    {item}
                    <Web3Button
                      contractAddress={contractAddress}
                      action={(contract) => contract.call("deleteToDo", index)}
                      accentColor="#1ce"
                    >
                      Delete Todo
                    </Web3Button>
                  </li>
                ))}
              </ul>
            )}
          </div>
        </>
      ) : (
        <ConnectWallet accentColor="#1ce" colorMode="light" />
      )}
    </div>
  );
};

export default Home;
```

Now, we have an awesome web3 to-do app ready! 🥳


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1662951004312/wpiwBGroC.png align="left")

## Conclusion

In this guide, we've learned how to build a full-stack web3 todo app! If you did as well pat yourself on the back and share it with us on the [thirdweb discord](https://discord.gg/thirdweb).