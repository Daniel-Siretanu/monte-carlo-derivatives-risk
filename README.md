# Monte Carlo Pricing and Risk of Derivatives

Monte Carlo pricing of options and interest-rate caps, validated against the Black–Scholes and
Bachelier formulas, with delta-hedging and VaR / Expected Shortfall analysis of the bank's risk.

I built this project to understand how derivatives are priced and, more importantly, what can go
wrong: for the client who buys an option, for the bank that sells it, and when a model's
assumptions stop matching reality.

## What's inside

**1. Pricing a European call**
- Black–Scholes formula vs Monte Carlo simulation.
- Validation: the simulated price stays within two standard errors of the formula, and the error
  shrinks like $1/\sqrt{N}$ as the number of paths grows.

**2. What the option means for the client**
- The price is set in a risk-neutral world, but real outcomes depend on how fast the stock
  actually grows, which can't be observed.
- I compare outcomes across several growth rates:

| | Stock grows 5% | Risk-neutral (10%) | Stock grows 15% | Stock grows 20% |
|---|---|---|---|---|
| Lose the whole premium | 42% | 26% | 14% | 7% |
| Beat a risk-free account | 24% | 39% | 56% | 72% |
| Median return | −80% | −34% | +21% | +84% |
| Mean return | −39% | ≈ 0% | +51% | +115% |

**3. What it means for the bank: delta hedging**
- The bank sells the option and hedges it by holding shares (Delta), rebalancing over time.

| Bank's position (per option) | Average P&L | 99% VaR | 99% Expected Shortfall |
|---|---|---|---|
| Not hedged | −£14.6 | −£142.8 | −£175.9 |
| Hedged weekly | ≈ £0 | −£1.6 | −£2.1 |
| Hedged daily | ≈ £0 | −£0.8 | −£1.0 |
| Hedged weekly, but volatility 30% instead of 20% | −£4.4 | −£11.7 | −£12.9 |

**4. Interest-rate cap with the Bachelier model**
- A company borrows £10m for 3 years and buys a 4% cap to limit its interest rate.
- Interest rates can go negative, which Black–Scholes can't handle, so the cap is priced with the
  Bachelier (normal) model: £95,049, confirmed by a Monte Carlo of 100,000 rate paths.

## Key takeaways

- **Advising a client:** the median describes what typically happens to someone who buys one
  option; the mean is pushed up by a few very large gains. I would show results for a range of
  growth rates, and be clear when a risk-free account is the better choice: if the stock grows at
  5%, three out of four buyers do worse than the account.
- **For the bank:** delta hedging removes almost all the risk, but it relies on the volatility
  being right. When the stock moved more than assumed, the hedged position lost money on almost
  every path.
- **Choosing the model:** Black–Scholes assumes prices can never go below zero. That failed when
  interest rates turned negative, so a model like Bachelier is needed.

## Files

| File | Description |
|---|---|
| `Monte_Carlo_simulations.ipynb` | Notebook with all code, maths and results |
| `monte_carlo_summary.pdf` | One-page summary of the project |

## How to run

Requires Python 3 with `numpy`, `scipy`, `pandas` and `matplotlib`:

```bash
pip install numpy scipy pandas matplotlib
```

Open the notebook and run all cells from top to bottom. The random seed is fixed, so the results
match the tables above.
