---
description: >-
  Synthetic tokens are fungible tokens whose price tracks the floor price of an
  underlying NFT collection.
---

# Synthetic Tokens (sTokens)

## Properties

### Symbol

Symbol describes the underlying NFT collection whose floor price will be tracked by the synthetic token.

### Price

Floor price of the underlying NFT collection. Price feed is provided by an Oracle pulling the latest data from NFT marketplaces like OpenSea and LooksRare. The price feed is updated every 15 minutes.

### Minimum Collateral Ratio

The minimum collateral to debt ratio $$r_{min}$$. If the collateral to debt ratio falls below the minimum collateral ratio, the debt position will be at risk of being liquidated.

### Discount Rate

When a debt position is being liquidated, liquidators can purchase the minter's debt with a discount rate $$\gamma$$.

## Lifecycle

### Mint

Users can mint synthetic tokens by over-collateralizing ETH on NFTSy. The synthetic tokens then become minter's debt afterwards. Let $$Q_c$$ be the amount of collateral, let $$P_n, Q_n$$ be the floor price of the NFT collection denominated in the collateral and the number of synthetic tokens being minted. The effective collateral ratio $$r_t$$ is&#x20;

$$r_t = \frac{Q_c}{P_n Q_n}$$

Minters need to make sure $$r_t > r_{min}$$, otherwise, their debt positions will be subject to liquidation. Minters can deposit extra collaterals or withdraw collaterals as long as after withdrawal the remaining collateral ratio is still above the minimum collateral threshold.

Minters can pair their synthetic tokens with ETH to provide liquidities, or directly sell their synthetic tokens to a Dex. The later is equivalent to taking a short position on the underlying NFT collection.

### Buy

Users can buy and sell synthetic tokens on any existing decentralized exchanges like Uniswap.

### Provide Liquidity

Liquidity providers can provide liquidities for synthetic tokens by acquiring synthetic tokens first, either through minting or purchasing synthetic tokens from a Dex, and then pair synthetic tokens with ETH to inject into a Dex liquidity pool.

### Burn

Minters can burn synthetic tokens to unlock their collaterals. Let $$Q_m, Q_b$$ be the number of synthetic tokens minter has minted and attempts to burn, and let $$Q_c$$ be the number of collateral minter has deposited. The total amount of collateral unlocked $$Q_u$$ is

$$Q_u = \frac{Q_b}{Q_m} * Q_c$$

### Liquidate

A debt position with effective collateral ratio $$r_t < r_{min}$$ is subject to liquidation. Liquidators can purchase minter's collateral with a discount by burning synthetic tokens until the minter's effective collateral ratio recovers above the minimum collateral ratio again.

Let $$P_n, Q_n$$ be the floor price of underlying NFT collection denominated in the collateral and the amount of debt being liquidated, let $$\gamma$$ be the discount rate where $$1 + \gamma < r_{min}$$. Then the amount of collateral bought by the liquidator is&#x20;

$$Q_n P_n (1 + \gamma)$$

Let $$Q_c$$ be the total amount of collateral minter has deposited. Let $$P_d, Q_d$$ be the floor price of the underlying NFT collection denominated in the collateral and minter's total amount of debt. Then the maximum amount of debt that can be liquidated is

$$\max (0,  \frac{Q_c r_{min} - P_d Q_d}{ r_{min} - (1 + \gamma)})$$

For example, if the minimum collateral ratio $$r_{min} = 160\%$$, discount rate $$\gamma = 120\%$$, total collateral $$Q_c = 300$$, total minted debt $$Q_d = 2$$, floor price of underlying NFT collection $$P_d = 100$$. Then effective collateral ratio $$r_t = 150\%$$, the maximum amount of debt that can be burnt $$Q_b = 0.5$$. Liquidators receive 60 ETH in return by burning 0.5 synthetic tokens, netting a profit of 10 ETH. Minter's remaining debt after liquidation $$Q'_d = 1.5$$, remaining collateral $$Q'_c = 240$$, resulting in an effective collateral ratio $$r'_t = 150 \% = r_{min}$$.
