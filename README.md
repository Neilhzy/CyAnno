# CyAnno

**About CyAnno**
The 'ungated' cells represent the undefined cells after all mutually exclusive series of ‘gating’ events are performed. These ‘ungated’ cells are present in a substantially large number within the pool of live cells generated by CyTOF. The primary reason that makes the identification of the ‘ungated’ class of cells difficult is their ambiguous marker expression profile that mimics the expression profile of gated cell types.
CyAnno (*Cy*ToF *Anno*tator) is a novel semi-automated Machine Learning (ML) based computational framework that can effectively classify live cells to one of the gated cell types. The unique approach includes a systematic way for incorporating the marker expression level features of 'ungated' cells for building an optimized ML classification model. By accounting for the ‘ungated’ class of cells in the datasets, CyAnno can predict cell labels with high accuracy.

**Applications**
1. CyAnno is especially designed to analyze large scale immunological studies to predict cell labels of closely related (but mutually exclusive) cell types, after manually gating only a few samples.
2. CyAnno performed exceptionally good in the identification of rare cells or cell types which are very small in terms of population size.
3. CyAnno can be used to train and predict even a single cell type, irrespective of its population size. This unique feature of CyAnno can be useful for a broad range of studies; e.g. when only one (or few) cell types are of clinical interest, especially those rare cell types that cannot be clustered with unsupervised clustering. 
4. CyAnno can be used for cell label prediction of 'gated' cell type even in the datasets with more than 20 cell types, including closely related cell types. 

**Other important considerations**
1. CyAnno is _not_ an alterative of unsupervised clustering, rather it learns the features of manually gated and mutually exclusive cell types from few samples and predicts the similar cells from the pool of live cells using FCS file(s).
2. Cell types must be mutually exclusive and both parent and child gated cell types can _not_ be used together for cell type prediction.
3. The prediction accuracy of the algorithm may depend upon the choice of training set, and we recommend the inclusion of samples processed under different batches or stimulation for training the models, to keep the overall training unbiased for any given batch or stimulation. 
4. CyAnno prediction accuracy has not been tested on samples with strong stimulation that can lead to aberrant marker expression profile.
5. Other factor that can affect the prediction accuracy includes the ambiguity in marker expression profile of different cell types.

# How to run
It essentially requires three inputs:
1. **Training dataset**: 
  
  a.) [Hand-gated] List of FCS/CSV file(s), each representing a cell type manually gated from clean pool of live cells (non-debris; non-doublets). At least one FCS file per cell type is mandatory. The choice of samples selected for manually gating is important. We recommend the inclusion of atleast one sample from each batch or stimulation used in the study, to keep the overall training unbiased for any given batch or stimulation.
  
  b.) [All live cells] List of orignal FCS/CSV file(s) with all the live cells that were used for manual gating the cell types. The algorithm uses these cells to find out cells not the part of any of the gated cell type, i.e. 'ungated' cells (only if Findungated=True).
  
2. **Unlabelled dataset**: List of FCS/CSV file(s) that are required to be labelled. These are the CyTOF samples (clean live cells: non-debris; non-doublets etc) for which you need to predict cell labels from one of the gated cell types.

3. **Lineage markers**: Marker names which were used for manually gating the given cell types.

### Training dataset 
CyAnno requires two files training the models:
1. [Hand-gated]
List of FCS/CSV file containig the marker expression profile of each handgated cell from each training sample. This is a three column csv file:

Column 1: Path of FCS/CSV manually gated file from a live cell sample.

Column 2: Name of the cell type. You can choose any name of the celltype. However, name must be exactly same for the FCS/CSV files representing the given cell type.

Column 3: Identifier of the sample from which the given FCS file is manually gated out. All the cell types manually gated from a given sample will have same value of column 3. 

Example file can be found in **_example/handgated.csv_**

2. [All live cells]
This contains the list of CSV/FCS files which were used for manual gating, i.e. training samples. It contains only the live cells (non-debris; no dublets). This is a two column csv file:

Column 1: Path of FCS/CSV file with live cells (non-debris; non-doublets) which will be cell labelled by CyAnno.

Column 2: Identifier of the sample. In case, this sample is also used for manual gating to generate training dataset, the identifier must match with column 3 of training dataset.

Example file can be found in **_example/LivecellsTraining.csv.csv_**

### Unlabelled dataset
This contains the list of CSV/FCS files in which every cell has to be annotated. The format is same as [All live cells]. This is a two column csv file:

Column 1: Path of FCS/CSV file with live cells (non-debris; non-doublets) which will be cell labelled by CyAnno.

Column 2: Identifier of the sample. This Identifier can be any unique name assigned to each sample. Output labelled file will be written with this identifier used.

Example file can be found in **_example/Livecells.csv_**

### Methods
CyAnno uses three different ML algorithms for building prediction models, i.e. extreme gradient boosting (XGboost; fastest), Multi-Layer Perceptron (MLP; slow) and Support Vector Machine (SVM; very slow). For each of the ML algorithms, the hyper-parameters are optimized using random grid search with cross validation analysis, wherein the large search space are defined for different parameter options for each ML algorithm. These algorithms can be called independently or they all can be used together for predicting cell labels using an ensemble model (very slow but less likely to build overfit model), which combined the results of multiple base estimators and provide consensus results by majority voting approach. 

# Installation
CyAnno does not require any installation and can be executed with Command Line Interface (CLI). However, python (3.0 or above) is required with numpy, pandas, scikit-learn, [faiss](https://github.com/facebookresearch/faiss "Facebook nearest neighbourhod approximation"), scipy, [xgboost](https://anaconda.org/conda-forge/xgboost), [pickle](https://docs.python.org/3/library/pickle.html), [fcsparser](https://github.com/eyurtsev/fcsparser) and itertools libraries. For convinience, we recommend [anaconda](https://anaconda.org/anaconda/python) for python and library installation, as it comes with most of the libraries preloaded. 

# Usage 

All the required parameters and arguments, including path to the input files can be specified together in 'CyAnno.py' file using any text editor or CLI based editor like nano or vim. Finally, CyAnno can be executed as:

```
python CyAnno.py
```


# Example
### Multi-center dataset
The example run, which includes Multi-Center dataset can be executed via CLI (provided all prerequisite libraries are installed):
```
git clone https://github.com/abbioinfo/CyAnno.git
cd CyAnno
python CyAnno.py
```

