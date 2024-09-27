# Investigating Differential Expression by Sex and Brain Region

## To-Do

## The Project

### Our Proposed Pipeline

#### Data Normalization
For each combination of nuisance factors (lab and chip version) normalize the set of rows corresponding to that combination (e.g. (Davis, v2)) by group mean and standard deviation. This produces our standardaized dataset hopefully rid of lab to lab and chip to chip variation.

####  Getting a List of Genes
I'll describe the process for analyzing sex differences and the brain region differences will largely take the same shape. Given a subset of the data (the cross-validation we will discuss later) group the data by brain region (DLFP, ACC, cerebellum) and within each of these groups find the top `g` differentially expressed genes within each group. Then, concatenate the lists and take the top `g` genes from the concatenated lists. These will be our `g` genes.

#### Cross-Validation for Robustness and Uncertainty Quantification
For a given analysis perform a stratified `k`-fold cross validation such that each fold's variable of interest is split about 50/50. For the `k`th fold, produce `g` genes using the procedure above. Then, produce `g` genes using this same procedure in the test set and find which of the training genes are present in the test set of genes. For any such genes differentially expressed in both sets, we add this to our final list of genes and count the number of folds (1 to `k`) that it appeared in. The number will serve as our confidence metric for the genes.

#### Tuning Parameters to Decide
We need to decide `g` and `k`. Due to our low number of observations, perhaps 4 fold cross validation is appropriate, but we are not locked in here. As for `g`, it should probably be at least 20, but unsure about this. Also, in the `Getting a List of Genes` step we can potentially take some number other than the top `g` genes from the concatenated list: the size of the conditional lists doesn't have to be the same as the final list. This might prove to be a relevant tuning parameter when it comes to deriving a reasonable set of genes.

### The Objective
For both the differential conditions of sex and brain region (ACC vs. DLFPC), find 20 genes of interest and report the reliability of our findings.

### The Data
30 tissue samples taken from:
* 10 recently deceased individuals
* 5 males
* 5 females
3 brain regions
* Anterior cingulate cortex (ACC)
* Dorsolateral prefrontal cortex (DLPFC)
* Cerebellum

for a total of 30 tissue samples assayed at 3 labs. Each of the 30 tissue samples were portioned into 3 aliquots and sent to labs at

* UC Davis
* UC Irvine
* University of Michigan

for a total of 90 microarrays. Note: 6 of the 90 didn't pass quality control, so we only have data for 84. The microarrays were produced by Affymetrix in two slightly different versions.

* HG U95A
* HG U95Av2
* Only genes that appear on both versions are retained in the dataset
* 12,600 genes

Note: technically, probesets not genes, and some are controls.
