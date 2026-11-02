# Packaging Standardization — BabyShip

> How much billable weight does a seven-box catalog add when it must fit 17 products?

**Optimization study · recorded solver output**

A mixed-integer model chooses shared packaging and product assignments from 43 candidate boxes, making the shipping-weight tradeoff explicit.

[Case study](https://daniel-kang-vs.github.io/projects/babyship.html) · [Source evidence](https://github.com/daniel-kang-vs/stroller-box-optimization/blob/main/IP_modeling.ipynb) · [Live dashboard](https://daniel-kang-vs.github.io/stroller-box-optimization/)

[![Dashboard preview](https://raw.githubusercontent.com/daniel-kang-vs/daniel-kang-vs.github.io/main/pngs/project-ip-optimization.png)](https://daniel-kang-vs.github.io/stroller-box-optimization/)

## Highlights

- **17 products · 43 candidates**
- **7 selected box types**
- **6.49% added billable weight**

## The problem

A smaller packaging catalog simplifies stocking, but oversized boxes increase dimensional weight. The decision is which boxes to keep under a seven-type cap.

## Data & scope

Product dimensions, weights, and the candidate box catalog are embedded in the notebook. The model adds 0.5 inches to each product dimension and uses a dimensional-weight divisor of 250.

## How it works

1. Calculate feasible product–box assignments under the notebook’s orientation and clearance rules.
2. Define billable weight as the maximum of actual weight and box volume divided by 250.
3. Minimize the sum of billable weights, with one assignment per product and at most seven active box types.
4. Compare the solution with each product’s independently best feasible catalog box.

## Key technical decision

Use an explicit catalog cap and physical fit constraints. Billable weight is a transparent proxy, but it is not a carrier invoice or a measured dollar saving.

## Results & interpretation

Recorded output selects seven types with total billable weight of 1,718.016 lb, versus 1,613.312 lb for the unconstrained per-product benchmark. The added 104.704 lb is 6.49%. Lotus Travel Crib and City Mini GT2 account for 67.712 lb, or about 64.7% of that penalty.

## Limitations & next steps

This is a modeled, equally weighted product comparison. It does not include shipment frequency, negotiated carrier rates, box purchase costs, damage, or measured warehouse labor. A 17-product baseline does not establish that 17 distinct boxes were previously stocked.

## Evidence & reproducibility

IP_modeling.ipynb contains the model and stored solver output. Running the optimization requires a suitable Gurobi license.

## Repository guide

- [`IP_modeling.ipynb`](IP_modeling.ipynb) — analysis implementation.
- [`docs/`](docs/) — static dashboard, styles, and browser-side analysis.

## Run or inspect

Clone this repository and run from its root:

```bash
pip install jupyter pandas numpy gurobipy
jupyter notebook IP_modeling.ipynb
```

Notebook commands require the referenced CSV files and compatible dependencies; recorded results have not been retrained for this documentation refresh.

## Technology

Python · Gurobi · pandas · Optimization
