---
description: Democratized DeFi Liquidations
cover: ../../.gitbook/assets/orca_heading.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# ORCA (Liquidations)

[https://orca.kujira.app/](https://orca.kujira.app/) or [https://orca.kujira.network/](https://orca.kujira.network/)

### Liquidate multiple assets across the whole of Cosmos at the click of a button. No bots, no code, no problems.

Built on Kujira, ORCA is the world's first public marketplace for bidding on liquidated collateral. By effectively participating in a Dutch auction, users are able to obtain liquidated collateral at up to a 30% discount. To bid on liquidated collateral on ORCA, users can select the desired market and premium, input the amount they want to bid, and hit the "Place My Bid" button.

Now that [GHOST](../ghost-money-market/) is live, users can also use ORCA to sell yield bearing assets at local tops up to 43% _above_ the market price. Read more [here](basics/lending-markets/xasset-lending-markets.md#sell-yield-bearing-assets-up-to-43-above-local-market-tops).&#x20;

## How it works

ORCA operates using a queue-based and anti-bot approach, filling bids from the smallest discount to the biggest. This helps to find an optimal discount rate and prevents immediate selling and damage to the market by bot operators. Instead, liquidated assets are purchased by community members who often have a stake in the longer-term success of partner platforms and may increase their ownership rather than immediately selling.

## Advantages

In this way, ORCA offers several benefits for partner money markets and users, including a user-friendly interface, transparent bidding process, near instant settlement of liquidations, robust yet capital efficient approach, ability to [earn yield while waiting for liquidations](basics/lending-markets/xasset-lending-markets.md), and strong community. Users can report bugs and give feedback on our Telegram or Discord.

## Analytics

Furthermore, at present we have basic ORCA liquidation analytics available for all users on all of our ORCA markets. In the future, we will offer more sophisticated liquidation analytics as well.

## 1st Party Integrations

FIN, ORCA, GHOST, and USK all have co-integrations. USK relies on ORCA as a liquidation settlement layer. You can learn more [here](../usk-stablecoin.md). GHOST also relies on ORCA to handle associated liquidations.

## Audit

Our ORCA smart contracts have been audited. You can take a detailed look [here](https://drive.google.com/file/d/1UzZQ3kD7a4PAooeO6GMhxZOH1TYr41r3/view).

Read more on Medium [here](https://medium.com/team-kujira/testnet-usk-minting-orca-liquidation-bids-4f1215e9677b). Happy hunting.
