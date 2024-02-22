---
title: "Creating an NFT minting Farcaster frame on Solana"
seoTitle: "Creating an NFT minting Farcaster frame on solana"
seoDescription: "In this blog, we will create a Farcaster frame that allows users to mint NFTs on solana to their wallets or email after recasting and liking the cast."
datePublished: Thu Feb 22 2024 17:28:20 GMT+0000 (Coordinated Universal Time)
cuid: clsxhyjx0000009jy0o1a8ydi
slug: farcaster-frames-solana
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1708621224064/7d08b54b-e7f8-4148-8787-1ad75b452ec6.png
tags: solana, nft, farcaster, farcaster-frames

---

## Introduction

In this blog, we will create a Farcaster frame that allows users to mint NFTs on solana directly to their wallets or email after recasting and liking the cast. Try out the [live demo here](https://warpcast.com/avneesh/0x7dc70d75)! Check out the [video demo here](https://twitter.com/avneesh0612/status/1760722748429472132).

### What is a Farcaster frame?

A Frame empowers you to transform any cast into an interactive application. Seamlessly integrate features like polls, live feeds, galleries, NFT mints, etc. into Warpcast or any other Farcaster client. Frames extend the OpenGraph standard and turn static embeds into interactive experiences.

### Tools

We're going to use various tools to build this:

- [Next.js](https://nextjs.org/): We'll use next.js as our framework for creating the frame.
- [Crossmint](https://www.crossmint.com/): To mint NFTs on solana to the user's wallet address or email, we'll use crossmint.
- [Neynar](https://neynar.com/): We will use neynar to check whether the user has recasted and liked the cast.
- [Onchainkit](https://onchainkit.xyz/): We will use the onchainkit to create the frame, and get the message from the frame on our server.

## Setting up our NFT collection

Head over to [Crossmint](https://staging.crossmint.com/) (staging for devnet), then head over to the Collections tab and click on "New Collection".

![Create a new collection on crossmint](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609039219/e217451c-f10e-4dbd-aa67-ed0769aa7a88.png align="left")

Then, fill in the metadata for your collection, and click on Next:

![Enter metadata for your NFT Collection](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609046333/679cdaab-e5bf-4a4e-ac6f-b9f458c212ba.png align="left")

You can either select to create a new contract or import an existing one. I don't have a contract created so I'll create a new one:

![Create or import an existing contract](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609050504/15deba6b-d84d-45a9-baed-6adc7ef1dd79.png align="left")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609055599/e2ddb3f2-1acb-4a8a-80e4-8c340bc2b400.png align="left")

Since we are going to be deploying on Solana choose Solana Devnet:

![Choose network you want to deploy on, in this case solana](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609061200/86b2341b-b059-466d-a455-8276720fbaf6.png align="left")

Here, you can enter any amount, since we won't be doing a claim but minting the NFTs to the user's wallet for free

![Payment settings for NFT collection on crossmint](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609069789/72be7004-98bc-4ce5-a2b8-ddc55ad10469.png align="left")

Finally, confirm the details, and click on Create Collection:

![Deploy your program](https://cdn.hashnode.com/res/hashnode/image/upload/v1708609071666/297692eb-c4bd-407f-9971-11c44fc40ada.png align="left")

## Creating the frame

### Creating a new next.js app

First, we need to create a new next.js app. We can do this by running the following command:

```powershell
npx create-next-app farcaster-frame
```

This will create a new next.js app in the `farcaster-frame` directory. So, navigate to the `farcaster-frame` directory, and open it in your code editor.

Then, we need to install the packages we need:

```powershell
npm install @coinbase/onchainkit @bonfida/spl-name-service @neynar/nodejs-sdk @solana/web3.js # npm

yarn add @coinbase/onchainkit @bonfida/spl-name-service @neynar/nodejs-sdk @solana/web3.js # yarn
```

### Creating the frame

Firstly, let's clean the `src/app/layout.tsx` file since we don't need any styling or metadata in the layout:

```typescript
export const viewport = {
  width: "device-width",
  initialScale: 1.0,
};

export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  );
}
```

Then, head over to the `page.tsx` file and replace the content with the following:

```typescript
import { getFrameMetadata } from "@coinbase/onchainkit";
import type { Metadata } from "next";

const NEXT_PUBLIC_URL = process.env.NEXT_PUBLIC_URL;

const frameMetadata = getFrameMetadata({
  buttons: [
    {
      label: "Mint an NFT",
      action: "post",
    },
  ],
  image: {
    src: `${NEXT_PUBLIC_URL}/default.png`,
    aspectRatio: "1.91:1",
  },
  input: {
    text: "enter...",
  },
  postUrl: `${NEXT_PUBLIC_URL}/api/frame`,
});

export const metadata: Metadata = {
  title: "Mint an NFT",
  description: "Mint an NFT",
  openGraph: {
    title: "Mint an NFT",
    description: "Mint an NFT",
    images: [`${NEXT_PUBLIC_URL}/default.png`],
  },
  other: {
    ...frameMetadata,
  },
};

export default function Page() {
  return (
    <>
      <h1>Mint an NFT</h1>
    </>
  );
}
```

Here, we are creating the `frameMetadata` which contains the buttons, image, input, and postUrl. We just have a single button which creates an API request to the `/api/frame` endpoint. And, we take in the wallet address, domain or email as input.

Then, we export the metadata which contains the `frameMetadata`, and some other metadata like title, description, and openGraph.

Finally, we are returning a simple h1 tag with the text "Mint an NFT". This will be displayed on the website, but this has nothing to do with the frame.

As you can see we are using a `default.png` image. This is the image which will be displayed in the frame at the beginning. You can replace this with your image. You can take a look at the image I am using [here](TODO).

### Creating the API endpoint

Now, we will create the API endpoint which will get called once the user hits the mint button. Create a new file called `route.ts` in the `app/api/frame` directory, and add the following content:

```typescript
import {
  FrameRequest,
  getFrameMessage,
  getFrameHtmlResponse,
} from "@coinbase/onchainkit";
import { NextRequest, NextResponse } from "next/server";
import { getDomainKeySync, NameRegistryState } from "@bonfida/spl-name-service";
import { Connection, PublicKey } from "@solana/web3.js";

export async function POST(req: NextRequest): Promise<Response> {
  let input: string | undefined = "";
  let recipientAddress = "";
  const NEXT_PUBLIC_URL = process.env.NEXT_PUBLIC_URL;
  let isEmail = false;
  const body: FrameRequest = await req.json();

  try {
    const { message } = await getFrameMessage(body, {
      neynarApiKey: "NEYNAR_ONCHAIN_KIT",
    });

    if (message?.input) {
      input = message.input;
    }

    if (!input) {
      return new NextResponse(
        getFrameHtmlResponse({
          image: {
            src: `${NEXT_PUBLIC_URL}/error2.png`,
          },
          ogTitle: "Error",
        })
      );
    }

    if (input.slice(-4) === ".sol") {
      const { pubkey } = getDomainKeySync(input);
      const connection = new Connection("https://api.mainnet-beta.solana.com");
      const { registry } = await NameRegistryState.retrieve(connection, pubkey);

      const pubKey = new PublicKey(registry.owner);
      recipientAddress = pubKey.toBase58();
    } else if (input.includes("@")) {
      isEmail = true;
      recipientAddress = `email:${input}:solana`;
    } else {
      recipientAddress = `solana:${input}`;
    }

    return new NextResponse(
      getFrameHtmlResponse({
        image: {
          src: `${NEXT_PUBLIC_URL}/success.png`,
        },
      })
    );
  } catch (error) {
    return new NextResponse(
      getFrameHtmlResponse({
        image: {
          src: `${NEXT_PUBLIC_URL}/error.png`,
        },
        ogTitle: "Error",
      })
    );
  }
}

export const dynamic = "force-dynamic";
```

A lot is going on here, so let's break it down:

- We are getting the frame message from the request body using the onchainkit and then getting the input from the message.
- If there's no input, we return an error image. I have created a simple error image which you can use, or you can create your own or even use an image generator to create specific errors but for now, I will use the images that I have created.
- Then, we determine whether the input is a domain, email, or wallet address. If it's a domain, we get the wallet address from the domain using Bonfida. If it's an email, we set the recipient address to `email:${input}:solana`. So, the user can view it in his Crossmint wallet. If it's a wallet address, we set the recipient address to `solana:${input}`.

Now, let's mint the NFT to the user's wallet using crossmint. To do that add the following:

```typescript
const env = process.env.CROSSMINT_ENV || "staging";
const crossmintURL = `https://${env}.crossmint.com/api/2022-06-09/collections/ENTER_YOUR_COLLECTION_ID/nfts`;
const crossmintOptions = {
  method: "POST",
  headers: {
    accept: "application/json",
    "content-type": "application/json",
    "x-api-key": process.env.CROSSMINT_API_KEY!,
  },
  body: JSON.stringify({
    recipient: recipientAddress,
    metadata: {
      name: "Cool NFT 😎",
      image: `${NEXT_PUBLIC_URL}/nft.png`,
      description: "This is my cool NFT",
    },
  }),
};
```

Replace `ENTER_YOUR_COLLECTION_ID` with your collection ID. You can get your collection ID from the collections tab in Crossmint. Also, replace the metadata and `nft.png` with the image you want to use for the NFT. I have added the image to the `public` directory in the next.js app.

We are using the `CROSSMINT_API_KEY` env variable here, I'll go over where to get it in the getting to production section.

Then, replace `return new NextResponse` with the following:

```typescript
return new NextResponse(
  getFrameHtmlResponse({
    image: {
      src: `${NEXT_PUBLIC_URL}/success.png`,
    },
    buttons: [
      isEmail
        ? {
            label: "View your NFT on crossmint",
            action: "link",
            target: `https://${env}.crossmint.com/user/collection`,
          }
        : {
            label: "Your NFT will arrive soon",
          },
    ],
  })
);
```

Once the API call is successful, the user will see a success image, and a button to view the NFT on Crossmint. If the recipient address is an email, the user will see a button to view the NFT on Crossmint. If the recipient address is a wallet address, the user will see a message that the NFT will arrive soon.

### Checking whether the user has recasted and liked the cast

This is an optional step but if we want to get some more engagement on the cast, we can make it compulsory for the users to recast and like the cast before they can mint the NFT. To do that, we can use neynar. Add the following to the `route.ts` file:

```typescript
if (process.env.WARPCAST_HASH && process.env.NEYNAR_API_KEY) {
  const neynarURL = `https://api.neynar.com/v2/farcaster/cast?identifier=${process.env.WARPCAST_HASH}&type=hash`;

  const neynarResponse = await fetch(neynarURL, {
    headers: {
      api_key: process.env.NEYNAR_API_KEY,
      "content-type": "application/json",
    },
    method: "GET",
  });

  const data = await neynarResponse.json();

  const reactions = await data.cast.reactions;

  const hasRecasted = reactions.recasts.some(
    (recast: { fid: Number }) => recast.fid === message?.interactor.fid
  );
  const hasLiked = reactions.likes.some(
    (likes: { fid: Number }) => likes.fid === message?.interactor.fid
  );

  if (!hasRecasted || !hasLiked) {
    return new NextResponse(
      getFrameHtmlResponse({
        image: {
          src: `${NEXT_PUBLIC_URL}/error1.png`,
        },
        postUrl: `${NEXT_PUBLIC_URL}/api/frame`,
        buttons: [
          {
            label: "Try again",
            action: "post",
          },
        ],
      })
    );
  }
}
```

Here, we are making a call to Neynar's api and getting the cast. We are then checking whether the array of recasts and likes contains the user's fid. If it doesn't, we return an error image and a button to try again.

The final `route.ts` file should look something like this:

```typescript
import {
  FrameRequest,
  getFrameMessage,
  getFrameHtmlResponse,
} from "@coinbase/onchainkit";
import { NextRequest, NextResponse } from "next/server";
import { getDomainKeySync, NameRegistryState } from "@bonfida/spl-name-service";
import { Connection, PublicKey } from "@solana/web3.js";

export async function POST(req: NextRequest): Promise<Response> {
  let input: string | undefined = "";
  let recipientAddress = "";
  const NEXT_PUBLIC_URL = process.env.NEXT_PUBLIC_URL;
  let isEmail = false;
  const body: FrameRequest = await req.json();

  try {
    const { message } = await getFrameMessage(body, {
      neynarApiKey: "NEYNAR_ONCHAIN_KIT",
    });

    if (message?.input) {
      input = message.input;
    }

    if (!input) {
      return new NextResponse(
        getFrameHtmlResponse({
          image: {
            src: `${NEXT_PUBLIC_URL}/error2.png`,
          },
          ogTitle: "Error",
        })
      );
    }

    if (process.env.WARPCAST_HASH && process.env.NEYNAR_API_KEY) {
      const neynarURL = `https://api.neynar.com/v2/farcaster/cast?identifier=${process.env.WARPCAST_HASH}&type=hash`;

      const neynarResponse = await fetch(neynarURL, {
        headers: {
          api_key: process.env.NEYNAR_API_KEY,
          "content-type": "application/json",
        },
        method: "GET",
      });

      const data = await neynarResponse.json();

      const reactions = await data.cast.reactions;

      const hasRecasted = reactions.recasts.some(
        (recast: { fid: Number }) => recast.fid === message?.interactor.fid
      );
      const hasLiked = reactions.likes.some(
        (likes: { fid: Number }) => likes.fid === message?.interactor.fid
      );

      if (!hasRecasted || !hasLiked) {
        return new NextResponse(
          getFrameHtmlResponse({
            image: {
              src: `${NEXT_PUBLIC_URL}/error1.png`,
            },
            postUrl: `${NEXT_PUBLIC_URL}/api/frame`,
            buttons: [
              {
                label: "Try again",
                action: "post",
              },
            ],
          })
        );
      }
    }

    if (input.slice(-4) === ".sol") {
      const { pubkey } = getDomainKeySync(input);
      const connection = new Connection("https://api.mainnet-beta.solana.com");
      const { registry } = await NameRegistryState.retrieve(connection, pubkey);

      const pubKey = new PublicKey(registry.owner);
      recipientAddress = pubKey.toBase58();
    } else if (input.includes("@")) {
      isEmail = true;
      recipientAddress = `email:${input}:solana`;
    } else {
      recipientAddress = `solana:${input}`;
    }

    const crossmintURL = `https://${env}.crossmint.com/api/2022-06-09/collections/56d9e10d-3caf-46a8-80a3-a507d034e51f/nfts`;
    const crossmintOptions = {
      method: "POST",
      headers: {
        accept: "application/json",
        "content-type": "application/json",
        "x-api-key": process.env.CROSSMINT_API_KEY!,
      },
      body: JSON.stringify({
        recipient: recipientAddress,
        metadata: {
          name: "Cool NFT 😎",
          image: `${NEXT_PUBLIC_URL}/nft.png`,
          description: "This is my cool NFT",
        },
      }),
    };

    const response = await fetch(crossmintURL, crossmintOptions);
    await response.json();

    return new NextResponse(
      getFrameHtmlResponse({
        image: {
          src: `${NEXT_PUBLIC_URL}/success.png`,
        },
        buttons: [
          isEmail
            ? {
                label: "View your NFT on crossmint",
                action: "link",
                target: `https://${env}.crossmint.com/user/collection`,
              }
            : {
                label: "Your NFT will arrive soon",
              },
        ],
      })
    );
  } catch (error) {
    return new NextResponse(
      getFrameHtmlResponse({
        image: {
          src: `${NEXT_PUBLIC_URL}/error.png`,
        },
        ogTitle: "Error",
      })
    );
  }
}

export const dynamic = "force-dynamic";
```

That's it! We have created the frame. Now, let's get to production.

## Getting to production

Create a new [GitHub repository](https://github.com/new) and push the code to the repository.

```powershell
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/<username>/<repo_name>.git
git branch -M main
git push -u origin main
```

Then, head over to [Vercel](https://vercel.com/) and import the repository. Once you have imported the repository, head over to the environment variables and add the following environment variables:

- `NEXT_PUBLIC_URL`: The URL of your next.js app.
- `CROSSMINT_API_KEY`: The API key for crossmint. Head over to the [API keys section](https://staging.crossmint.com/console/projects/apiKeys) in the Crossmint dashboard and create a new server key with permission to mint NFTs. Then, add the key as an environment variable.
- `CROSSMINT_ENV`: If you switch over your app from testnet to mainnet, you can add the environment variable `CROSSMINT_ENV` and set it to `www`.

If you want to check whether the user has recasted and liked the cast, you can add the following environment variables:

- `WARPCAST_HASH`: The hash of the cast which you want to check has been recasted and liked.
- `NEYNAR_API_KEY` The API key for neynar. You can get it from the [neynar dashboard](https://dev.neynar.com/).

Then, head over to the domains tab and add your domain.

That's it! You have created a frame that allows users to mint NFTs on solana directly to their wallets or email after recasting and liking the cast.

### Test the frame

You can go to the [Frames validator](https://warpcast.com/~/developers/frames) on Warpcast and test your frame. If everything looks good, make a cast with the URL of your frame and share it with your friends to test it out!

## Conclusion

This guide taught us how to create a Farcaster frame that allows users to mint NFTs on solana directly to their wallets or email after recasting and liking the cast. You learned a lot, now pat yourself on the back and share your amazing apps with me!

### Useful links

[GitHub Repo](https://github.com/avneesh0612/farcaster-frame-solana)

[Farcaster](https://farcaster.com/)

[Crossmint](https://crossmint.com/)

[Neynar](https://neynar.com/)

[Connect with me](https://links.avneesh.tech/)
