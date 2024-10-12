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

### Clustering of patients:<br>
Dimensionality reduction techniques evaluated:<br>
•	Methods: Uniform Manifold Approximation and Projection (UMAP), t-distributed Stochastic Neighbor Embedding (t-SNE), Principal Component Analysis (PCA).<br>
•	Selection - UMAP was chosen for clustering due to its ability to provide distinct visual separation of clusters.<br>

UMAP parameter optimization:<br>
•	n_neighbors - set to 4, controlling the number of neighboring points for local manifold approximations.<br>
•	n_components - set to 2, defining the dimensions in the reduced space.<br>
•	min_dist - set to 0.3, regulating the minimum distance between points in the low-dimensional space.<br>
•	R package used - UMAP R package (version 0.2.10).<br>

Determination of optimal clusters (k):<br>
•	Method - Silhouette score calculated using the cluster R package (version 2.1.6).<br>

Clustering implementation:<br>
•	Algorithm used - KMeans algorithm from the stats R package (version 4.4.0).<br>

### Clustering validation:<br>
A hypergeometric test was conducted to evaluate clustering bias:<br>
•	Most drugs showing differential responses between the clusters identified by the limma package (version 3.60.2) were kinase inhibitors.<br>
•	The analysis assessed whether the significant overrepresentation of kinase inhibitors in clustering results was due to bias toward this majority drug class or occurred by chance.<br>
•	The p-value served as the determining metric: a high p-value indicated that the observed number of kinase inhibitors in the cluster was consistent with chance expectations, while a low p-value suggested potential bias.<br>
•	The phyper function from the stats package in R (version 4.4.0) was used for hypergeometric testing.<br>

To evaluate whether patient clustering reflected true biological differences in drug responses and not cell proliferation rates:<br>
•	The t-test was performed using the t.test function from the base R package (version 4.4.0) to compare proliferation rates between the identified groups.<br>

### Statistical analysis of omics datasets:<br>
Proteomic and phosphoproteomic analysis:<br>
•	Differential expression analysis conducted using the limma package (version 3.60.2).<br>
•	Upregulated and downregulated expressions were identified using log2 fold change (log2FC).<br>
•	The most significantly expressed phosphoproteins were further analyzed using Kinase-Substrate Enrichment Analysis (KSEA).<br>
•	Phosphosite annotation performed using the PhosphoSitePlus® Kinome Scan database.<br>

Transcriptomic analysis:<br>
•	Conducted using the DESeq2 package (version 1.44.0).<br>
•	The most significantly differentially expressed genes were used for pathway enrichment analysis.<br>
•	Gene Ontology (GO) enrichment analysis for biological process pathways was performed using GSEA in the fgsea package (version 1.30.0).<br>

Principal component analysis (PCA):<br>
•	PCA plots were generated to assess the separation of groups based on transcriptomic and phosphoproteomic markers.<br>
•	PCA computation was done using the stats package (version 4.4.0).<br>

Data visualization:<br>
•	Visualization achieved using the ggplot2 (version 3.5.1) and ComplexHeatmap (version 2.18.0) packages to illustrate differences between the groups.<br>

### Evaluation of phosphoprotein markers as drug response predictors:<br>
Algorithm Used:<br>
•	Employed the RandomForestRegressor algorithm from the scikit-learn library (version 1.5.1) in Python (version 3.11.4).<br>

Feature Selection:<br>
•	Phosphoproteins selected based on their effectiveness in separating patient groups from prior PCA analysis.<br>

Labels:<br>
•	Labels were derived from drug response data for 318 drugs that showed significant differential responses between groups.<br>

Dataset Split:<br>
•	Divided into an 80/20 ratio for training and test sets.<br>

Model Optimization:<br>
•	Performed using 5-fold cross-validation.<br>
•	Key optimized parameters:<br>
  o	n_estimators: 200 (number of trees in the forest)<br>
  o	min_samples_split: 2 (minimum number of samples to split an internal node)<br>
  o	max_features: 'auto' (number of features considered for the best split).<br>
  
Model Evaluation:<br>
•	Performance was evaluated using Mean Squared Error (MSE).<br>











