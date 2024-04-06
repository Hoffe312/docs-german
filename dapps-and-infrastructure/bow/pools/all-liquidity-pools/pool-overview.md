---
cover: ../../../../.gitbook/assets/Kujira BOW (1).png
coverY: 0
---

# Pool Overview

## Overview

For a specific liquidity pool, in addition to the basic header contained in the 'Overview', 'Provide', and 'Stake' pages), the 'pool overview' page contains corresponding information on that pool's current existing FIN spread for that token pair, BOW's strategy for that specific pool, detailed information about withdrawing liquidity, providing liquidity, staking LP tokens, links for the token pair on FIN, the liquidity pool on FINDER, and the pool's incentive contract on FINDER, the spread of active marketing making bids (both size and price), as well as detailed charts of the incentive release schedule over time. &#x20;

<figure><img src="../../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>General info on the ATOM/USK pair and its BOW algo settings</p></figcaption></figure>

<figure><img src="../../../../.gitbook/assets/image (9).png" alt=""><figcaption><p>KUJI Incentives to LPers on the ATOM/USK pair</p></figcaption></figure>

## Live Market Making View

For every liquidity pair, BOW's market maker activity in the FIN orderbook is displayed for the base asset on top in red and for the quote asset on bottom in green as well as the current pair spread.&#x20;

This eponymous view is BOW's namesake. Unlike AMMs (automatic market makers) or hybrid decentralized exchanges, [FIN](../../../fin/) is a central limit order book (CLOB) where both buy and sell orders must exist for transactions to take place. This allows BOW to be a hyper efficient market maker as FIN only requires liquidity around the range of active FIN limit orders.

<figure><img src="../../../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Live BOW orders on FIN covering both sides of the pair.</p></figcaption></figure>

Like a curved bow, the majority of active liquidity is at the edges (highest and lowest prices), which minimizes the amount of liquidity necessary to create a deep trading experience around each token pair.

## Incentive Release Schedule

Each liquidity pool's 'Pool Overview' page contains a detailed breakdown of the pool's incentive release schedule over time. The same incentive can be provided multiple times for the same pool and even partially overlap overlap in their provision. For each pool, every available active incentive's schedule will be displayed in various charts. Each chart has a start and end date, the number of tokens to be released in total, and a graph showing the approximate of token incentives of that type being released on a specific day (from that specific provision) across all staked LPers.&#x20;

Incentives can either be provided as one of two types: decreasing or fixed.

<figure><img src="../../../../.gitbook/assets/image (11).png" alt=""><figcaption><p>Decreasing incentives</p></figcaption></figure>

Decreasing incentives linearly reduce from the start to finish of a provision period. The total amount of remaining incentives can be calculated using the triangle area rule 1/2\*base\*height. The remaining incentives are shaded in green.

<figure><img src="../../../../.gitbook/assets/image (12).png" alt=""><figcaption><p>Fixed incentives</p></figcaption></figure>

Fixed incentives are consistent from start to finish of a provision period. The total of amount of remaining incentives can be calculated using the rectangle area rule length\*width. The remaining incentives are shaded in green.&#x20;

New incentives can be added at any time, but each additional provision gets its own emission chart.&#x20;
