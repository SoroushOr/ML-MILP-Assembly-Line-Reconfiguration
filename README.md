# ML-MILP Framework for Automated Assembly Line Reconfiguration

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange)
![Optimization](https://img.shields.io/badge/Optimization-MILP-success)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Scikit--Learn-yellow)
![Statistics](https://img.shields.io/badge/Statistics-SciPy-lightgrey)

## Overview
This repository contains the source code, datasets, and results for a hybrid **Machine Learning (ML)** and **Mixed-Integer Linear Programming (MILP)** framework designed for the optimal reconfiguration of Automated Assembly Lines (ALB). 

Due to the lack of open-source, real-world dynamic setup time datasets, this study employs a **Data-Driven Proxy Simulation** to generate realistic operational noise. The framework utilizes ML regressors to predict dynamic reconfiguration times and feeds these predictions into a Brownfield MILP optimization model to minimize total incurred downtime and disruption penalties. 

## Repository Structure & Compressed Archives
Due to the large volume of instances (over 10,000 files) and to ensure structural integrity, the repository's data and results are provided in four separate compressed archives based on the problem scale ($r$):
- `r=5.zip`
- `r=10.zip`
- `r=15.zip`
- `r=20.zip`

Inside each archive, the directory is cleanly separated into three subfolders for presentation and readability:
* `/src`: Contains the Jupyter Notebooks.
* `/data`: Contains the raw `.alb` instances and input CSVs.
* `/results`: Contains the outputs and final ML predictions.

**⚠️ Important Execution Note regarding Paths:** While the repository is organized into distinct folders for readability, the Python scripts are designed to execute within the exact same directory as the data they are processing. To successfully run any notebook, you must place the script directly alongside the target dataset files. The scripts will read the datasets from their current working directory and generate the outputs locally.

## Pipeline Execution & Data Flow

### 1. Universal Scripts (Identical across all datasets)
Notebooks `01`, `02`, `03`, and `05` are completely universal. They can be applied to any dataset without any internal code modifications.
* **`01_Proxy_Data_Generation.ipynb`**: Simulates the operational scenarios and generates the ground-truth proxy dataset. 
  * **Output:** `Training_Data.csv`
* **`02_Feature_Engineering_and_Clustering.ipynb`**: Parses raw `.alb` instances and applies K-Means clustering to extract equipment complexity and flexibility.
  * **Output:** `*_Features.csv`
* **`03_Machine_Learning_Pipeline.ipynb`**: Trains the ML regressors on the training data and predicts dynamic setup times for the test instances.
  * **Output:** `*_Final_ML_Output.csv`
* **`05_Statistical_Analysis_and_Synthesis.ipynb`**: Aggregates the final actualized costs and performs non-parametric Wilcoxon signed-rank tests.

### 2. Specific Script (Requires minor modification)
* **`04_MILP_Optimization_and_Evaluation.ipynb`**: This is the core mathematical optimization module. 
  * **⚠️ CRITICAL:** Before running this script, you must explicitly define the dataset scale you are evaluating. Go to **Cell 2** and modify the `TARGET_R` variable to match your dataset (i.e., set `TARGET_R = 5`, `10`, `15`, or `20`).

## Dependencies
Ensure you have the following Python libraries installed before executing the notebooks:
```bash
pip install numpy pandas scikit-learn pulp matplotlib scipy seaborn
