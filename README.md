# Packaging Standardization — BabyShip

![Python](https://img.shields.io/badge/Python-3.10+-blue?logo=python) ![Gurobi](https://img.shields.io/badge/Gurobi-ILP-e85d04) ![pandas](https://img.shields.io/badge/pandas-NumPy-150458?logo=pandas)

> **Decision:** pick at most 7 shipping boxes for 17 strollers. A unique box per product is the cheapest on dimensional weight and the worst thing to stock. The solver keeps the billable-weight penalty to **6.5%** while cutting box variety by **59%**.

**[Live demo](https://daniel-kang-vs.github.io/stroller-box-optimization/)**

---

## Project Highlights

- Integer program over **43 candidate boxes** and **17 products**, with a hard cap of **7 box types**
- Fit is physical, not approximate: a stroller fits a box only if length, width, and height all clear with **0.5 inch** of padding
- **774 binary variables**, **1,394 constraints**, proven optimum in **0.02 seconds**
- Compared against a one-box-per-product baseline so the standardization penalty is explicit, not implied

---

## Problem Statement

Carriers bill **dimensional weight**, `L × W × H / 250`, whenever that exceeds the product’s actual weight. Empty space inside an oversized box becomes cost.

Two extremes both fail:

| Policy | Shipping weight | Warehouse problem |
|--------|-----------------|-------------------|
| One perfect box per stroller | Lowest (1,613.3 lb) | 17 SKUs to stock, pick, and store |
| One box for everything | Highest | One SKU, a lot of paid air |
| **At most 7 box types** | **1,718.0 lb** | Variety cut by 59% |

The question is which 7 commercial boxes to keep, and which stroller goes in which box, so total billable weight stays close to the perfect-fit baseline.

---

## Data

| Input | Count | Source |
|-------|-------|--------|
| Strollers | 17 | Manufacturer dimensions and weights |
| Candidate boxes | 43 | Walmart, Uline S-codes, and standard carton sizes |
| Fit rule | 0.5 in padding | All three dimensions must clear |

A product-box pair is feasible only when the stroller fits. Infeasible pairs are excluded before the solver runs, so the optimizer never assigns a box that cannot physically hold the product.

---

## Formulation

```
Decision variables
  y_b    = 1 if box type b is stocked
  x_pb   = 1 if product p ships in box b

Constraints
  each product assigned to exactly one feasible box
  x_pb ≤ y_b                         a product cannot use a box that is not stocked
  sum(y_b) ≤ 7                       warehouse variety cap

Objective
  minimize total billable weight
  billable weight = max(actual weight, L × W × H / 250)
```

The baseline is the same objective with the variety cap removed, which collapses to one box per product.

---

## Results

| Metric | Value |
|--------|-------|
| Box types selected | 7 |
| Optimized billable weight | 1,718.0 lb |
| Perfect-fit baseline | 1,613.3 lb |
| Standardization penalty | 104.7 lb (**6.5%**) |
| Variety reduction | 17 → 7 types (**59%**) |
| Model size | 774 binaries, 1,394 constraints |
| Solve time | 0.02 s to proven optimum |

---

## Key Technical Decisions

1. **Cap variety, do not cap weight.** The business constraint is the number of cartons the warehouse will stock. Putting that in the constraint and weight in the objective makes the tradeoff a number someone can accept or reject.

2. **Padding is a constraint, not a fudge factor.** A 0.5 inch clearance on every side stops the solver from calling a snug CAD fit a shippable box.

3. **Report the penalty against a perfect baseline.** “7 boxes” is not a result by itself. The result is how many extra pounds that cap costs versus giving every stroller its own box.

4. **The objective is billable weight, not a carrier invoice.** Lane, zone, and contract rates are not in the model. A next pass would price the same assignments in dollars.

---

## Repository Structure

```
├── IP_modeling.ipynb    # Gurobi model, fit matrix, and baseline comparison
├── docs/                # Live demo (GitHub Pages)
│   ├── index.html
│   ├── style.css
│   └── app.js
└── README.md
```

---

## How to Run

Gurobi requires a local license (free academic license works).

```bash
pip install pandas numpy gurobipy matplotlib seaborn
jupyter notebook IP_modeling.ipynb
```

The demo site does not need Gurobi. It is the static page in `docs/`.

---

## Tech Stack

| Library | Purpose |
|---------|---------|
| gurobipy | Integer program and proven optimum |
| pandas / NumPy | Product and box tables, fit matrix |
| matplotlib / seaborn | Weight and assignment charts in the notebook |
| Chart.js | Charts on the live demo |
