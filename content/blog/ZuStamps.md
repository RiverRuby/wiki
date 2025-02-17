---
title: ZuStamps v1 Retrospective
date: 2023-06-24
---

Originally posted on https://pse-team.notion.site/Zupass-Stamps-v1-Retrospective-6bd7aa223f4d41cdb59b4ecf8aafb130

built by Vivek, Rachel, Althea

# Overview

In the final 2 weeks of Zuzalu, a PSE and 0xPARC collaborative effort consisting of Althea/Rachel/Vivek launched a set of **stamps** to store in the [Zuzalu Passport](https://zupass.org) that all Zuzalu residents and visitors were able to set up.

In short, these stamps served as **private Zuzalu mementos**, akin to POAPs. They primarily proved participation in different experiences at Zuzalu — attending town halls, hacking in a hackathon, cooking for the community dinner, etc. But unlike POAPs, these mementos were **never on-chain** and **never left a user’s passport**. Setting them up didn’t require a wallet or sending a transaction. And they were kept under the user’s control and no one (not even the passport server) could determine what set of stamps a user collected.

The stamps were generated by **NFC chips** that generated a **unique signature** on every tap, from a [hardware locked](https://github.com/arx-research/libhalo) private/public key pair. Each chip produced a different stamp, which was displayed on the user’s passport by extracting the chip’s public key from the signature. The stamp photo to display for each public key was maintained in a permissioned [Airtable](https://airtable.com/shrSnWKFbecOnzVDM/tblEUyUjq6AyS8PZB) for deploying & operational convenience, but could easily be moved to a smart contract.

Then, if a Zuzalu participant wanted to use these stamps to do something else — claim an NFT, gain attendance to another event, or just standard bragging rights — they could generate a **ZK proof of ownership** of a valid signature from a chip, without revealing the plaintext signature.

![Friends of Zuzu was 1 of 34 collectible stamps which residents could collect if they met Zuzu, who was rescued during Zuzalu and adopted by a Zuzaluan!](Frame_2.png)

Friends of Zuzu was 1 of 34 collectible stamps which residents could collect if they met Zuzu, who was rescued during Zuzalu and adopted by a Zuzaluan!

## Goals

The initial motivation and goals for this project were brainstormed by Rachel and Vivek in a FigJam brainstorm documented [here](https://www.figma.com/file/FVzPknquoDESmafU0UBnhC/NFC-PCD-Explorations?type=whiteboard&node-id=0-1&t=FbVbDdbJAnl8qSAo-0). We determined that the project should help Zuzalu participants in the final two weeks of Zuzalu:

1. Add meaningful stamps to personal passports

2. Prove attendance or participation

3. Own stamps and control when to share them

Notably, it was out of scope to generate ZK proofs over the stamps. Although it would’ve been possible with existing tools like [spartan-ecdsa](https://github.com/personaelabs/spartan-ecdsa), we focused on the stamp collecting experience itself. In addition, we didn’t yet have applications or protocols that were ingesting these stamps to build other experiences.

## Metrics

In the end, we allocated **34** different chips which were stored in plastic white cards the size of a credit card. Each card corresponded to a different memento, and had a (nail-polish) painted design on the front and a custom designed stamp when Zuzalu participants added a stamp to their phone. A read-only list of all the cards, their designs, and their owners can be found [here](https://airtable.com/shrSnWKFbecOnzVDM/tblEUyUjq6AyS8PZB).

By looking at the sizes of people’s encrypted stores, **we can estimate the number of people that collected stamps and the number of total stamps collected**. This is because the size of the encrypted data matches the size of the decrypted data (with 16 bytes of extra data for an authentication tag) with the algorithm we used, [XChaCha20-Poly1305](https://libsodium.gitbook.io/doc/secret-key_cryptography/aead/chacha20-poly1305/xchacha20-poly1305_construction).

A big thank you to Ivan Chub from 0xPARC for helping with collecting these metrics. He is the lead developer of Zupass and helped direct us to a number of relevant code sections.

There are some edge cases when converting `size of encrypted store` into `number of stamps`, so we opted for a more conservative computation of metrics. Thus these are likely underestimates, but still give a good sense of how people interacted with the project:

![[Pasted image 20240111032658.png]]

There were **123 total people** who collected stamps, with 46 individuals collecting 5 or more stamps. In total **500+ stamps were collected**, all of which occurred in the last 10 days of Zuzalu when the least number of residents/visitors were around.

**What we can infer from the metrics**

- People have a real interest in collecting stamps for keepsakes without external incentive or prize for collecting them.

- The UX was simple enough for our two main user personas to operate:

- **26** **card owners** gave out stamps

- **123** **event attendees** were able to collect multiple stamps across different events

- Future deployments at conferences, music festivals, retreats, and other network states could get significant usage if the experience is available from day 1 of the event.

# The Zuzalu Genesis Collection

This is the full set of cards and stamps that were collectable at Zuzalu, forming **The Zuzalu Genesis Collection**. A huge thank you to everyone who [signed up](https://airtable.com/shrvKWBW0FHhiNWvk) to create a stamp and gave it out via the NFC cards!

![Card designs. Made by Althea, with nail polish, superglue, and paper!](Cards.png)

Card designs. Made by Althea, with nail polish, superglue, and paper!

![Stamp designs. Made by Rachel, Mari, Kashushu, Althea and Colton](Stamps.png)

Stamp designs. Made by Rachel, Mari, Kashushu, Althea and Colton

# High-level learnings

**_Reception_**

We found that this experience resonated with people who weren’t familiar with the tech. It was tactile and social, and represented a real experience or relationship. The physical form factor and the fact that cards were only held by a single person made it feel special and real to collect a stamp.

**_Roles_**

It was important to define roles and choose the right people for them. People needed to own the various steps of creating and distributing cards and stamps:

- Card setup - there was an initial bulk setup, but each individual card and stamp also needed to be set up for its new owner.

- Stamp design - people were excited to collect stamps as little digital artworks.

- Card design - the cards themselves being interesting artifacts/tiny works of art made people want to engage with them.

- Evangelism - it was really important to find the right people to own the cards, make sure they had a card/stamp that represented something important to them, and get their buy-in for the idea so that they would put in the effort to give out the stamps and explain their significance.

- Support - there were issues with scanning cards and struggles with the passport app, and having someone available to troubleshoot was the difference between somebody trying it out or giving up.

**_Behind the scenes_**

- Scanning UX could be improved.

- Many people aren’t used to NFC scanning, and there were differences between devices (where the scanner is located, how long it took, whether the phone needed to be locked or unlocked etc). Vivek wrote [instructions](https://www.notion.so/NFC-tapping-instructions-7cd23882b7264c2aae2724f7ac622b52?pvs=21), which ideally would have been attached to the card. Another possibility would be wrapping the card in some sort of container that would make scanning easier.

- The URL that the cards pointed to (eth.vrfy.ch) looked sketchy and wasn’t what people expected.

- Initially, people who scanned a card without being logged in to the passport had a really confusing experience. This was [addressed](https://github.com/proofcarryingdata/zupass/pull/283) by introducing an option for a user who wasn’t logged in to add a stamp later.

- Issues with the passport app, particularly syncing, were a barrier.

- A lot of people didn’t understand how to sync, or even that they needed to, which meant they had to repeatedly sign in (which was annoying) or reset their passports (which meant losing their stamps)

- This would be a high-priority issue to solve for future iterations, either by improving the passport app or using a different interface

- The back end had a lot of dependencies and points of centralization.

- Initial setup for the cards, e.g. changing the URLs, involved coordinating several parties with communication delays at each step.

- The continuing availability of the stamps currently depends on 0xPARC keeping the Zupass server live. A future iteration should use decentralized storage.

- The stamps themselves are stored and linked via an Airtable that can be changed by anyone with access. This is not ideal if the stamps are being used in a context where immutability is important; on the other hand, if stamps are a privacy-preserving way for an individual (e.g. an organizer or artist) to maintain a connection with participants in some unique experience, the potential for stamps to be dynamic could actually be a feature.

- The team had to set up each card for its holder since setup involved accessing a sensitive database - introducing a way for people to set up their own cards would be more scalable and better UX

# Technical implementation

## Zupass

[zupass.org](http://zupass.org) was the passport built for Zuzalu residents and visitors, primarily by 0xPARC. It was directly connected to the [pretix](https://pretix.eu/about/en/) ticketing service used for Zuzalu, and served as a primary mechanism to determine whether someone had an official invite to Zuzalu when checking into various events.

On top of connecting to the ticketing service, Zupass gave everyone a [Semaphore identity](https://semaphore.appliedzkp.org/), a utility built by PSE, that allows for private group signaling. This allowed users to integrate with anonymous apps like [zupoll.org](http://zupoll.org) and [zuca.st](http://zuca.st), which each saw significant usage over the course of Zuzalu.

To sync across devices, the Zupass server stored an E2E encrypted version of user’s data, which could be encrypted/decrypted using the **sync key**, which was randomly generated upon creating a Zupass:

![The modal users would get upon signing up.](zupass_sync_key.png)

The modal users would get upon signing up.

The sync key was an unfamiliar term and UX for folks, and from some user research it seems the majority of Zuzalu attendees didn’t understand what its purpose was. This key serves the same purpose as the **master password** for different password managers like 1Password and BitWarden, which would be a better term for future iterations.

## NFC chips

We were lucky enough to get 50 passive NFC HaLo (Hardware Locked) chips from our friends at [iyk.app](http://iyk.app), who initially sourced them from the good folks at [arx.org](http://arx.org). These chips each have an Ethereum private key, with which an Active NFC reader could perform [any of the following operations](https://github.com/arx-research/libhalo/blob/master/docs/halo-command-set.md).

The main operation we used is [`sign_random`](https://github.com/arx-research/libhalo/blob/master/docs/halo-command-set.md#command-sign_random) which takes the chip’s private key and signs an **internal incrementing nonce** and a set of random numbers. Notably, every tap on the chip increments the nonce and thus each signature that people receive is unique.

![The incrementing nonce is displayed as part of the stamp a user receives.](stamp_mock.png)

The incrementing nonce is displayed as part of the stamp a user receives.

The other interesting thing about the incrementing nonce is that (if you trust the hardware maker, which mirrors trusting POAP to prevent double use of a specific QR code), a signature with nonce `x` must have come earlier in time than a signature with nonce `y > x`. This means there is a built in notion of a more“OG” stamp holders (if your nonce is lower), as well as a way to make different anonymity sets based on how early you got things.

In fact, it means that all chips can be used for an unlimited number of stamps. You can designate that taps with nonces between 5 and 25 get one stamp jpeg. And then stamps from 26 to 80 get another. Or something more exotic like [nouns.wtf](http://nouns.wtf) where every 10th tap can only be used by organizers or something.

## PCD framework

The signature was converted into a PCD, or a **proof-carrying data.** A close description can be found on the [devcon forum](https://forum.devcon.org/t/dip-regarding-pcd-based-ticketing-for-devconnect/3201#pcd-framework-2) or on [github](https://github.com/proofcarryingdata/zupass#zuzalu-passport-cards). The simplest spec for PCD is implemented [here](https://github.com/proofcarryingdata/zupass/blob/main/packages/pcd-types/src/pcd.ts) in TypeScript.

A signature is the simplest form of PCD, and the above `sign_random` command was reimplemented as a PCD [here](https://github.com/proofcarryingdata/zupass/blob/main/packages/halo-nonce-pcd/src/HaLoNoncePCD.ts). This allowed for this data to be displayed in the passport, which only supports data in the “PCD” format!

## Displaying in the passport

The HaLoNoncePCD generated from the chip was displayed according to [this code](https://github.com/proofcarryingdata/zupass/blob/main/packages/halo-nonce-pcd/src/CardBody.tsx). In particular, it read from this [Airtable](https://airtable.com/shrSnWKFbecOnzVDM/tblEUyUjq6AyS8PZB) where Rachel/Althea/Vivek were able to pick a specific image for a specific card public key. We also tracked a number of other things there, like if the card was allocated to its owner and if we posted a Telegram notification about its existence.

![A handful of the cards we deployed from the above Airtable.](all_cards.png)

A handful of the cards we deployed from the above Airtable.
