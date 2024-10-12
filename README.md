# Ex Vivo Drug Response Profiling Reveals Two AML Subgroups with Distinct Kinase Activity, Immune Profiles and Gene Expression Linked to Clinical Survival Outcomes

## Overview

This study employs unsupervised clustering techniques to profile patients with acute myeloid leukemia (AML) based on their ex vivo responses to various chemotherapeutic and targeted oncology agents. Corresponding multi-omics datasets are utilized to establish a direct link between the molecular phenotypes of AML and their respective drug response profiles. Furthermore, survival analyses are conducted on the identified patient groups, with validation performed using an external dataset.

## Materials and Methods

### Datasets:<br>
•	Drug Sensitivity Scores (DSS) - measuring the percentage of relative cell growth inhibition.<br>
•	Proliferation rates - representing the change in cell proliferation over time from untreated to treated conditions.<br>
•	Proteomic and phosphoproteomic datasets - samples were analysed using an LC-MS/MS system.<br>
•	Genomic dataset - generated through targeted DNA NGS, focusing on 54 genes commonly mutated in AML.<br>
•	Transcriptomics dataset – consisting of gene counts.<br>

### Data pre-processing:<br>
#### The DSS dataset was subjected to missing data imputation.<br>

Two Methods Tested:<br>
•	K-Nearest Neighbours (KNN)<br>
•	Random Forest (RF)<br>

 Evaluation Process:<br>
•	Systematically removed 5% of randomly selected data points.<br>
•	Predicted missing values were compared to actual removed values.<br>

Performance Metrics:<br>
•	Normalized Root Mean Square Error (NRMSE) - assessed average discrepancy between predicted and observed values.<br>
•	R-squared Value - evaluated model fit.<br>

Results:<br>
•	RF outperformed KNN, achieving lower NRMSE and higher R-squared, leading to its selection.<br>

RF Algorithm Details:<br>
•	Utilizes regression trees to impute missing values independently for each drug.<br>
•	Considers the entire patient profile during imputation.<br>
•	Initial missing values filled with mean estimates, refined over multiple iterations.<br>

Optimized Parameters:<br>
•	ntrees: Set to 200 (number of trees in the forest).<br>
•	maxiter: Set to 15 (maximum iterations).<br>

Software Used:<br>
•	KNN: R caret package (version 6.0.94).<br>
•	RF: missForest package (version 4.4.0).<br>

#### DSS Dataset Preprocessing:<br>
•	Log2 Transformation - applied to correct skewness in the dataset.<br>
•	Min-Max Scaling - standardized feature ranges to ensure equal contribution to Euclidean distance calculations for clustering analysis.<br>

#### Proteomic and Phosphoproteomic Datasets:<br>
•	Log2 Transformation - conducted to stabilize variance and achieve homoscedasticity, essential for linear modeling (e.g., Linear Models for Microarray Data - limma).<br>
•	Min-Max Scaling -ensured uniform weighting of all proteins or phosphorylation sites during modeling.<br>

#### Transcriptomic Dataset Processing:<br>
•	Utilization of DESeq2 - processed directly on raw count data without prior transformation or scaling.<br>
•	Filtering Step - excluded genes with fewer than 10 counts across all samples to enhance statistical power and reduce analysis noise.<br>



