# Dashboard data provenance

Product names, per-product billable weights, and optimized assignments are transcribed from `IP_modeling.ipynb` and its stored output. `data.json` contains the exact displayed series.

- Independent best feasible catalog assignments: 1,613.312 lb in total.
- Seven-box solution: 1,718.016 lb in total.
- Difference: 104.704 lb, or 6.4900% of the baseline.
- Lotus Travel Crib: 36.608 lb extra. City Mini GT2: 31.104 lb extra.

The comparison weights every product once. No shipment-frequency weighting, warehouse-complexity index, carrier invoice, or financial saving is represented. The model adds 0.5 inches per dimension; it does not add 0.5 inches on each side.

To reproduce the optimizer, run the notebook in a Gurobi-enabled environment. If assumptions change, regenerate the assignment outputs and update both chart series and narrative; the dashboard is a presentation of recorded results, not a live solver.
