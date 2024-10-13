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

Algorithms used:<br>
•	KNN: scikit-learn library (version 1.5.1) [[Python code]](https://github.com/migleapa/AML-Patient-Profiling-Using-Unsupervised-Clustering/blob/main/DSS%20dataset%20Imputation%20with%20KNN.ipynb).<br>
•	RF: missForest package (version 4.4.0) [[R code]](https://github.com/migleapa/AML-Patient-Profiling-Using-Unsupervised-Clustering/blob/main/missForest%20imputation%20code.Rmd).<br>

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

Algorithm used:<br>
•	KMeans: scikit-learn library (version 1.5.1) [[Python code]](https://github.com/migleapa/AML-Patient-Profiling-Using-Unsupervised-Clustering/blob/main/DSS%20data%20UMAP%20and%20K-means%20clustering.ipynb).<br>

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

Feature Selection:<br>
•	Phosphoproteins selected based on their effectiveness in separating patient groups from prior PCA analysis.<br>

Labels:<br>
•	Labels were derived from drug response data for 318 drugs that showed significant differential responses between groups.<br>

Dataset Split:<br>
•	Divided into an 80/20 ratio for training and test sets.<br>

Model Optimization:<br>
•	Performed using 5-fold cross-validation.<br>
•	Key optimized parameters:n_estimators: 200, min_samples_split: 2,max_features: 'auto'.<br>
  
Model Evaluation:<br>
•	Performance was evaluated using Mean Squared Error (MSE).<br>

Algorithm Used:<br>
•	RandomForestRegressor from the scikit-learn library (version 1.5.1) in Python (version 3.11.4) [[Python code]](https://github.com/migleapa/AML-Patient-Profiling-Using-Unsupervised-Clustering/blob/main/RF%20Regressor%20(Multioutput)%20-%20drug%20response%20prediction.ipynb).<br>

### Survival analysis and validation:<br>
Survival Analysis:<br>
•	Kaplan-Meier survival probabilities were computed, and survival plots were generated using the survminer R package (version 3.7.0).<br>

Survival model validation:<br>
•	The survival prediction model was validated using the external dataset for trancriptomic and phosphoproteomic markers.<br>
•	The initial dataset's marker values were averaged to define classification thresholds for patient groups. These thresholds were then applied to classify new patients from external datasets based on their marker expressions.<br>
•	Kaplan-Meier survival analysis was performed again on this new set of classified patients to assess survival differences between the newly classified groups.<br>


## Results

### Unsupervised clustering identifies two AML subgroups:

![image](https://github.com/user-attachments/assets/8e9d79bf-e523-4a2f-a6f3-9e44a7644f5f)

K-means clustering of DSS data with k=2. 
<br>
<br>
### Identification of drug-sensitive and drug-resistant phenotypes:

![image](https://github.com/user-attachments/assets/bf3af380-cc79-4286-886f-5f39e3073ff8)

Volcano plot of drug responses in the drug-resistant group relative to the drug-sensitive group, categorized by functional drug class. The x-axis displays the log2 fold change, while the y-axis represents the -log10 transformed adjusted p-value. 
<br>
<br>
### Comparison of Cell Proliferation Rates Between Resistant and Sensitive Groups:

![image](https://github.com/user-attachments/assets/c03d024b-bee1-4254-a682-18673eebe936)

a) Boxplot depicting the distribution of data points across the two groups, along with statistical comparisons between them. b) A scatter plot showing the data points for both
<br>
<br>
### KSEA identifies significant differences in kinase activity related to cell proliferation, growth, stress response, apoptosis and survival:

![image](https://github.com/user-attachments/assets/8488a18d-cdb1-419d-8f9f-c18edecd73bc)

a) Limma analysis showing the top 12 differentially expressed phosphoproteins between the groups used for PCA and drug prediction analysis. The phosphoproteins were selected based on their statistical significance. b) SEA: Comparison of kinase enrichment between the experimental groups. c) PCA of the top 12 differentially expressed phosphoproteins. Each data point represents an individual patient, with points coloured according to their respective group. The plot illustrates the separation of the groups based on the phosphoprotein expression profiles.
<br>
<br>
### Higher prevalence of TP53 and RUNX1 mutations and exclusive del5 abnormality in drug-resistant group:

![image](https://github.com/user-attachments/assets/425c9a78-fdca-4940-a249-ca8f265342de)

a) The breakdown of gene mutations and the fraction of each within the drug-sensitive and drug-resistant groups. b) The breakdown of karyotypes and the fraction of each within the drug-sensitive and drug-resistant groups
<br>
<br>
### Transcriptomic markers predict worse clinical survival in drug-sensitive patients:

![image](https://github.com/user-attachments/assets/ceb20689-a7f9-4ce9-81e2-f52e37d25f61)

a) Survival plot and risk table comparing drug-resistant and drug-sensitive groups. b) Survival model evaluation using external dataset with selected markers CLIC5, LZTS3, HEPACAM2, COL13A1, HBG1, TFP12, and HEPH. c) Survival model evaluation using HEPH as individual predictor on external dataset.
<br>
<br>
## Summary of findings

The study revealed two distinct groups: one exhibiting greater drug sensitivity and the other demonstrating resistance following ex vivo profiling. Phosphoproteomic data revealed that the drug-sensitive group displayed elevated expression of kinases implicated in cell proliferation, differentiation, survival, apoptosis and stress response, while transcriptomics data identified a less active immune phenotype compared to drug-drug-resistant group. Furthermore, the drug-resistant group had a higher incidence of TP53 and RUNX1 mutations, with the del(5q) abnormality being exclusively present in this group. Kaplan Meyers survival analysis further revealed that the drug-sensitive group was associated with significantly poorer clinical survival. Additionally, several transcriptomic markers related to tumour suppression, iron and oxygen transport, cell adhesion and extracellular matrix were associated with survival outcomes. These markers were further validated using an external transcriptomic dataset, highlighting their potential as survival predictors and therapeutic targets in AML.


























