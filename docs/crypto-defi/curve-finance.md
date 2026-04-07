---
tags:
  - DeFi
  - Curve
  - AMM
  - veCRV
  - Stablecoins
date: 2026-04-07
---

# Curve Finance & veCRV

## Summary

Curve Finance is the dominant automated market maker (AMM) for stablecoin and like-kind asset swaps. Its StableSwap invariant allows low-slippage trades between pegged assets. The veCRV governance and fee distribution model makes Curve one of the most studied tokenomics systems in DeFi.

---

## StableSwap Mechanics

Curve uses a hybrid invariant that blends the constant-sum formula (x + y = k) with the constant-product formula (xy = k). The result is a curve that behaves like a constant-sum AMM near the peg (low slippage) and like a constant-product AMM at the extremes (protecting liquidity providers from full depletion).

The amplification coefficient **A** controls how flat the curve is near the peg. Higher A = lower slippage for in-range trades, but more exposure if the peg breaks.

---

## veCRV Tokenomics

| Mechanic | Detail |
|---|---|
| Lock period | 1 week to 4 years |
| veCRV decay | Linear decay to zero at lock expiry |
| Boost | Up to 2.5x on LP rewards |
| Fee share | 50% of trading fees distributed to veCRV holders |
| Gauge voting | veCRV holders direct CRV emissions to pools |

**Key insight:** veCRV is non-transferable. This creates a captive governance system where long-term holders have disproportionate influence over emissions — the foundation of the "Curve Wars."

---

## The Curve Wars

Protocols that need deep stablecoin liquidity (Frax, MIM, LUSD, etc.) compete to accumulate veCRV or bribe veCRV holders via platforms like **Votium** and **Hidden Hand** to direct gauge emissions toward their pools. This created an entire meta-economy around CRV voting power.

**Convex Finance** emerged as the dominant aggregator — pooling veCRV into cvxCRV, giving users liquid exposure to boosted yields while Convex votes the underlying veCRV. At peak, Convex controlled >50% of all veCRV.

---

## Competitive Landscape

- **Uniswap v3**: Concentrated liquidity competes on volatile pairs; less efficient for stables
- **Balancer**: Weighted pools, some stable pools, but smaller TVL in stables
- **Velodrome / Aerodrome**: ve(3,3) model on L2s (Optimism, Base), gaining share
- **DODO / Platypus**: Niche stablecoin AMMs with alternative designs

---

## Related Research

- YieldBasis — Michael Egorov's protocol building on Curve primitives
- GENIUS Act — stablecoin legislation implications for Curve pool demand

---

*Published: April 7, 2026*
