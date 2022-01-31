## How I built Inscribe: Bloggers DAO

Wassup everyone, in this article I wanna showcase a DAO that I built in the first week of January while learning web3! So, I built a bloggers Dao called Inscribe with Next.js, TailwindCSS, and Thirdweb!

## ✒️ What is Inscribe?

Inscribe is a DAO for bloggers. Inscribe lets you connect with like-minded people (bloggers).  Since it is a DAO it is decentralized and based upon democracy.

> Try it [now](https://www.inscribedao.me/)!


## 🪙What is a DAO?

Dao stands for "Decentralized autonomous organization" it is like a decentralized community where you can get an NFT inclusive to the community, vote for certain things in the community, get crypto of the community through AirDrops.

## 🤓 Walkthrough and the code 

### 🤖 Scripts

Firstly, I created some scripts for different things like creating a bundle drop for NFT, creating a claim condition for the NFT, creating a crypto [`$INK`](https://rinkeby.etherscan.io/token/0x4833b336a4c0c61d0ac35bcab772bef7bed86031), creating a voting module, proposals for the voting module, and a few more with the @[thirdweb](@thirdweb) sdk. You can check out all the scripts [here](https://github.com/avneesh0612/Inscribe/tree/main/scripts).

### 💻 Frontend

**Landing page**

The front end is powered by Next.js, TailwindCSS, and Thirdweb! Firstly, the user will see the landing page which is just a static page.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643277658175/SAHKCol39.png)

**Sign in page**

After clicking on the app or Join button, it will show the sign-in page. You can sign in through metamask which is the most commonly used way to authenticate in web3 apps! Thirdweb makes authentication with metamask super easy! It is as simple as this-

```
import { useWeb3 } from "@3rdweb/hooks";

const SignIn = () => {
  const { connectWallet } = useWeb3();

  return (
    <button
      onClick={() => connectWallet("injected")}
      className="group relative mt-5"
    >
      Sign in
    </button>
  );
};

export default SignIn;t
``` 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643278351951/29c9i4N5p.png)

**Switch Network**

If you are on any other chain/Network, like polygon or ETH Mainnet it will ask you to switch to Rinkeby as the app is built on the Rinkeby Network.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643278437144/p6Z7BF59os.png)

**Mint NFT**

After you sign in and are on the Rinkeby network, you will see a screen to mint NFT. It will show you some prompts and a popup to pay gas fees. If you don't have the eth for gas fees, you can get it from [here](https://faucets.chain.link/rinkeby) for free.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643286547086/uat5pwSUB.png)

**Member's list**

The member's list shows the wallet addresses with the amount of INK (crypto of our DAO), they have. It is sorted by the amount everyone has. The tokens are given to people through airdrops (where people randomly get some amounts) or someone else can transfer them to you! If you want some to test, leave your wallet address in the comments 😉. If you want to create your own crypto with Thirdweb, check out this [article](https://blog.avneesh.tech/make-your-first-crypto-with-thirdweb).

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643287207395/iAa826fxv.png)

**Voting on our proposals**

Besides the member list, you will see a proposal(s). You can vote on your polls, most of the features will be implemented after they pass through this poll. So, this helps in making the DAO democratic and community-focused. At the time of publishing this article, we have one proposal of adding "Add blogging feature to Inscribe". You can vote `for`, `against` or `Abstain`. This was made possible by the Vote module of Thirdweb.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643287895520/yODWHEnPk.png)


### Source code

The project is open-source on GitHub, so feel free to contribute, use and star the [repo](https://github.com/avneesh0612/Inscribe)!


## Getting an exclusive role in the discord server

We also have a [discord server](https://discord.gg/NY8XYuJjbw) for Inscribe where you can connect with other people. In this server, we have an exclusive `Member` role for people who have minted the [NFT](https://testnets.opensea.io/assets/0x2e2040BB43ba63AE7E055f28C3F61F2eb9e7B6d4/0).

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643288527843/SkufL8MIq.png)


This role gives you access to special channels! All this is powered by Collabland. To get the role after minting the NFT, simply go to the discord server and inside the `#collabland-join` channel, click on the Let's go button and verify your NFT, you will soon get the role 🥳

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1643288615920/RMiyDsLP6.png)


## Conclusion

This was a fun build to kick off my web3 journey, I couldn't add more features as I had my exams :( but soon, I am going to add more features and functionalities to it! 

Due to thirdweb, I could build this so fast and easily! Join us now on our DAO and the discord server. See ya there ✌️

### Useful Links

[Live Demo](https://www.inscribedao.me/)

[GitHub repo](https://github.com/avneesh0612/Inscribe)

[Let's Connect](https://links.avneesh.tech/)
