# Avellaneda-Stoikov Market Making & Beta Hedging Simulation

This repository contains a quantitative finance simulation implementing high-frequency market making dynamics based on the seminal **Avellaneda-Stoikov model** (2008), along with an inventory **beta-hedging** strategy using a correlated asset.

---

## 📌 Overview

Market makers face two fundamental risks:
1. **Adverse Selection / Price Risk:** The asset's mid-price moves against accumulated inventory.
2. **Execution Risk:** Quoting prices too far from the mid-price lowers fill probabilities, while quoting too close increases unwanted inventory accumulation.

This project implements:
- The classical **Avellaneda-Stoikov optimal control framework** to dynamically adjust the reservation price and optimal bid-ask spread according to current inventory and risk aversion.
- A **continuous inventory-driven market simulation** where limit order execution follows a Poisson arrival process with exponential fill probabilities.
- A **Beta-Hedging extension** where the market maker hedges directional inventory exposure in real time via a correlated instrument following an Ornstein-Uhlenbeck tracking error process, significantly reducing PnL variance.

---

## 📐 Mathematical Formulation

### 1. Avellaneda-Stoikov Framework

* **Reservation Price ($r(s, q, t)$):**
  $$r(s, q, t) = s - q \gamma \sigma^2 \frac{T - t}{T}$$
  * $s$: Current mid-price (modeled as geometric Brownian motion)
  * $q$: Current inventory level
  * $\gamma$: Risk aversion parameter
  * $\sigma$: Asset volatility
  * $T - t$: Remaining time horizon

* **Optimal Spread ($\delta^a + \delta^b$):**
  $$s(q, t) = \gamma \sigma^2 \frac{T - t}{T} + \frac{2}{\gamma} \ln\left(1 + \frac{\gamma}{\kappa}\right)$$
  * $\kappa$ (or order arrival intensity $\lambda$): Order book liquidity parameter.

* **Execution Probability:**
  $$P(\text{execution}) = \exp(-\alpha \cdot \text{distance})$$
  Orders further away from the mid-price have an exponentially decaying fill probability.

### 2. Beta Hedging Mechanism

To mitigate directional inventory swings:
* A correlated asset with price $S_{\text{hedge}} = \beta \cdot S + \epsilon_t$ is traded.
* $\epsilon_t$ follows a mean-reverting **Ornstein-Uhlenbeck (OU)** process:
  $$d\epsilon_t = -\kappa_{\text{OU}} \epsilon_t dt + \sigma_{\text{OU}} dW_t$$
* When a fill changes inventory by $\Delta q$, a hedge position $-\frac{\Delta q}{\beta}$ is executed in the hedging instrument.

---

## 📊 Features & Results

The simulation runs Monte Carlo trials (e.g., $N = 1,000$ iterations over 1-day trading horizons with 30-second time steps):

* **Pure Avellaneda-Stoikov Market Making:**
  * Captures the bid-ask spread while controlling inventory swings via skewing.
  * PnL exhibits moderate volatility due to unhedged residual directional inventory.
* **Market Making with Beta Hedging:**
  * Real-time hedging significantly shrinks the PnL distribution's standard deviation (e.g., standard deviation reduced by more than 70%), stabilising cumulative returns.

---

## 🛠️ Requirements & Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/](https://github.com/)<Lucille865>/<Monte-Carlo-simulation-of-Avellaneda-Stoikov-market-making model>.git
   cd <Monte-Carlo-simulation-of-Avellaneda-Stoikov-market-making model>
