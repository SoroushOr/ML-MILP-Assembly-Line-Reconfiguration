````markdown
# ML-Integrated MILP Framework for Robotic Assembly Line Reconfiguration

This repository contains the source code, processed datasets, and computational results for the paper:

**A Machine Learning-Integrated Mixed-Integer Linear Programming Framework for Robotic Assembly Line Reconfiguration under Dynamic Reconfiguration Times**

## Repository Package

The main replication package is provided as a compressed `ML-MILP-Assembly-Line-Reconfiguration.rar` file.  
Please download and extract the archive before running the codes or checking the result files.

After extraction, the package contains the following main folders:

```text
.
├── r=5/
├── r=10/
├── r=15/
├── r=20/
└── cross_r_comparison/
````

The folders `r=5`, `r=10`, `r=15`, and `r=20` contain the complete data, codes, and results for each benchmark group.
The folder `cross_r_comparison` contains the final cross-r aggregation code and the final comparison outputs used in the paper.

---

## Folder Structure

Each `r` folder has the following structure:

```text
r=5/
├── Code_01_...
├── Code_02_...
├── Code_03_...
├── Code_04_...
├── Code_05_...
├── input/data files
└── results/
```

The same structure is repeated for:

```text
r=10/
r=15/
r=20/
```

The `results/` folder inside each `r` directory contains all output files generated for that specific benchmark group, including optimization outputs, baseline comparisons, sensitivity analysis results, and statistical evaluation files.

The `cross_r_comparison/` folder contains:

```text
cross_r_comparison/
├── Code_06_...
└── results/
```

This folder aggregates the outputs from all four r-levels and generates the final cross-r comparison tables used in the paper.

---

## Code Description

The computational workflow consists of six main codes.

### Code 1 — Benchmark Data Preparation

Code 1 reads and prepares the ALB benchmark instance data for the selected r-level.

Main tasks:

* Reads the benchmark input files.
* Extracts task-related information.
* Extracts equipment-related information.
* Extracts cycle time, processing times, investment costs, processing costs, saving costs, task types, and precedence relations.
* Builds the task-equipment feasibility matrix.
* Prepares the structured input data required for the next steps.

This code should be run separately inside each r-level folder:

```text
r=5/
r=10/
r=15/
r=20/
```

---

### Code 2 — Feature Engineering and Clustering

Code 2 generates equipment-level features and applies clustering to characterize equipment flexibility and setup complexity.

Main tasks:

* Computes equipment-level features such as:

  * task coverage,
  * average processing time,
  * processing-time variability,
  * investment cost,
  * processing cost,
  * task-type diversity.
* Applies K-Means clustering.
* Interprets cluster centroids to define flexibility and setup-complexity levels.
* Generates the processed feature dataset used in the ML stage.

The clustering labels should not be interpreted directly by their numerical values.
Instead, the centroid-based interpretation is used to assign meaningful flexibility and setup-complexity levels.

---

### Code 3 — ML-Based Dynamic Reconfiguration-Time Estimation

Code 3 trains supervised machine learning regression models to estimate dynamic reconfiguration times.

Main tasks:

* Uses the processed equipment-level features.
* Trains regression models for dynamic reconfiguration-time estimation.
* Evaluates model performance using train-test validation and cross-validation.
* Selects the best-performing regression model based on prediction error.
* Generates predicted dynamic reconfiguration-time values for the ML-integrated MILP.

Important note:

The dynamic reconfiguration-time variable used in this project is a controlled proxy and was not collected from real shop-floor sensor data.
Therefore, the ML component should be interpreted as a proof-of-concept mechanism for integrating prediction-based reconfiguration-time parameters into an optimization model.

---

### Code 4 — MILP Optimization and Evaluation

Code 4 solves the reconfiguration optimization problem for the selected r-level.

Main tasks:

* Builds the classical fixed-time MILP formulation.
* Builds the ML-integrated MILP formulation.
* Solves both formulations using identical solver settings.
* Compares the classical MILP and ML-MILP on paired instances.
* Records objective values, solver statuses, task assignments, equipment decisions, and reconfiguration decisions.
* Saves the main optimization outputs in the `results/` folder.

The cost comparison is performed only for paired instances in which both formulations reach optimality.

---

### Code 5 — Sensitivity Analysis and Statistical Testing

Code 5 performs sensitivity analysis and statistical evaluation for the selected r-level.

Main tasks:

* Tests different values of the reconfiguration cost rate `CR`.
* Tests different values of the task-disruption penalty coefficient `gamma`.
* Compares the classical MILP and ML-MILP under each sensitivity scenario.
* Computes average cost savings.
* Applies paired statistical tests.
* Reports Wilcoxon signed-rank test results and paired t-test results.
* Saves sensitivity and statistical output files in the `results/` folder.

This code is used to evaluate whether the ML-MILP becomes beneficial under different economic parameter settings.

---

### Code 6 — Cross-r Comparison

Code 6 is located in the `cross_r_comparison/` folder.

Main tasks:

* Reads the result files generated from all four r-level folders:

  * `r=5`,
  * `r=10`,
  * `r=15`,
  * `r=20`.
* Aggregates baseline comparison results.
* Aggregates sensitivity analysis results.
* Aggregates statistical testing results.
* Generates the final cross-r comparison tables.
* Produces the summary files used in the paper.

The main outputs of Code 6 include files such as:

```text
Cross_R_All_Sensitivity_Scenarios.csv
Cross_R_Sensitivity_Comparison.csv
Final_Cross_R_Comparison_Table_For_Paper.csv
```

These files summarize the final computational evidence reported in the paper.

---

## Recommended Execution Order

To reproduce the complete workflow, run the codes in the following order:

```text
Step 1: Extract the RAR archive.

Step 2: Go to each r-level folder:
        r=5
        r=10
        r=15
        r=20

Step 3: Inside each r-level folder, run:
        Code 1
        Code 2
        Code 3
        Code 4
        Code 5

Step 4: After all four r-level folders are completed, go to:
        cross_r_comparison/

Step 5: Run:
        Code 6
```

Code 6 should be executed only after the results from all four r-level folders have been generated.

---

## Results

Each r-level folder contains a `results/` directory.
This directory includes the outputs generated for that specific benchmark group.

Typical result files include:

* optimization output files,
* solver status files,
* baseline comparison files,
* sensitivity analysis files,
* statistical testing files,
* decision-comparison files.

The `cross_r_comparison/results/` folder contains the final aggregated outputs across all r-levels.

---

## Main Findings

The computational results show that the ML-integrated MILP does not universally outperform the classical fixed-time MILP under all baseline settings.

However, sensitivity analysis shows positive cost-saving opportunities across all r-levels under selected economic conditions.

The best observed average sensitivity-based saving is:

```text
5.81% for r = 15
```

This indicates that ML-predicted dynamic reconfiguration times can be useful when reconfiguration-time-related costs strongly influence the total objective function.

---

## Important Limitations

The dynamic reconfiguration-time variable used in this project is a controlled proxy and was not collected from real shop-floor sensor data.

Therefore:

* the ML results should not be interpreted as evidence of industrial ML generalization;
* the framework should be interpreted as a reproducible proof of concept;
* future work should validate the approach using real operational data.

The purpose of the repository is to show how prediction-based dynamic reconfiguration-time parameters can be structurally integrated into a MILP reconfiguration model and evaluated across multiple benchmark groups.

---

## Citation

If you use this repository, please cite the associated paper:

```text
B. Vahedi Nouri, S. Oroojloo, and S. S. Mousavi,
"A Machine Learning-Integrated Mixed-Integer Linear Programming Framework for Robotic Assembly Line Reconfiguration under Dynamic Reconfiguration Times,"
submitted to IEEM 2026.
```

---

## Repository Link

```text
[[Repository link]
](https://github.com/SoroushOr/ML-MILP-Assembly-Line-Reconfiguration/tree/main)```

جای دقیقش: **آخر Conclusion، قبل از References**.
