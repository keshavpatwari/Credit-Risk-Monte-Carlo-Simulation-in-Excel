# Credit Risk Simulation Model (Excel)

A Merton-style Monte Carlo credit risk model built in Excel. It estimates **Probability of Default (PD)**, **Loss Given Default (LGD)** and a **portfolio loss distribution** for four listed Indian companies.

**Companies:** TVS Motors · Adani Enterprises · Sequent Scientific · Vodafone Idea


---

## What the model does

1. **Inputs:** 21 years of balance-sheet data per company (Mar '05 to Mar '25): Total Debt, Total Assets and Contingent Liabilities, in ₹ crore.
2. **Asset returns:** annual growth in total assets gives 20 observations. The mean and standard deviation of these returns drive the simulation.
3. **Default simulation (PD):** 1,000 one-year asset-value paths per company, using `NORM.INV(RANDARRAY())`. A path counts as a default if simulated assets fall below the default threshold:

   `Threshold = (Total Debt + Contingent Liabilities) × 1.1`

   `PD = number of defaults ÷ 1,000`
4. **LGD:** for each default, `LGD = (Threshold − Simulated Assets) ÷ Threshold`. The average across defaults is the mean LGD.
5. **LGD distribution:** LGD is modelled with a Beta distribution, with α = 10 × mean LGD and β = 10 − α, so the mean matches the simulated LGD.
6. **Portfolio loss:** for each of 1,000 trials, defaults are drawn per company from its PD. Loss is `Default × EAD × Beta-distributed LGD`. The four losses are summed to give the total portfolio loss, and a 40-bucket histogram shows the loss distribution.

## Workbook structure

| Sheet | Contents |
|---|---|
| `TVS Motors`, `Adani Enterprises`, `Sequent Scientific`, `Vodafone Idea` | Historical data, asset returns, 1,000 simulations, default flags, LGD, PD and Beta parameters |
| `calc` | Summary table: current assets, debt threshold, mean and standard deviation of returns per company |
| `loss_distribution` | Portfolio-level simulation, total loss and loss histogram |
| `Diagram` | Distribution chart of asset values against the default threshold for a selected company |

## Key results

Results come from a random simulation and change slightly each time the workbook recalculates.
Vodafone Idea is the highest-risk name. Its current assets are already below its default threshold, so its PD is far above the other three.

## Assumptions and limitations

- **Book values, not market values.** Asset returns come from growth in book total assets, with only 20 annual observations. This is not a market-implied asset volatility as in a full Merton model, so the volatilities and PDs are high for some companies.
- **Default threshold is a judgement call.** `(Debt + Contingent Liabilities) × 1.1` is a simplifying assumption, not a regulatory definition.
- **Beta parameters.** α + β = 10 is a fixed assumption that sets the spread of the LGD distribution.
- **Independence.** Defaults are drawn independently across the four companies, so portfolio tail risk is likely understated compared with a model that includes default correlation.
- **Simulation noise.** With 1,000 trials, PD estimates carry sampling error of roughly ±1 to 1.5 percentage points.
- **Small portfolio.** Four companies are used to demonstrate the method, not to represent a real loan book.


## Tools

Microsoft Excel 365: `RANDARRAY`, `NORM.INV`, `BETA.INV`, `LET`, `XLOOKUP`, `SEQUENCE`, `FREQUENCY`, charts

## Author

**Keshav Patwari** ·
