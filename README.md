# CyAnno (Version 1.0)

**About CyAnno**
The 'ungated' cells represent the undefined cells after all mutually exclusive series of ‘gating’ events are performed. These ‘ungated’ cells are present in a substantially large number within the pool of live cells generated by CyTOF. The primary reason that makes the identification of the ‘ungated’ class of cells difficult is their ambiguous marker expression profile that mimics the expression profile of gated cell types.
CyAnno (*Cy*ToF *Anno*tator) is a novel semi-automated Machine Learning (ML) based computational framework that can effectively classify live cells to one of the gated cell types. The unique approach includes a systematic way for incorporating the marker expression level features of 'ungated' cells for building an optimized ML classification model. By accounting for the ‘ungated’ class of cells in the datasets, CyAnno can predict cell labels with high accuracy.

**Applications**
1. CyAnno is especially designed to analyze large scale immunological studies to predict cell labels of closely related (but mutually exclusive) cell types, after manually gating only a few samples.
2. CyAnno is especially useful for large scale studies in which it can be used to label the cell type of every single live cell. To do that, CyAnno needs training data to first learn the features/properties of each cell type. For this training, CyAnno requires handgated cell types as FCS or CSV files from only a small number of samples. Once it learned the features of the provided cell types, it can predict which cells of an ungated FCS file (only gated for live cells) belong to what cell type.
3. CyAnno can be used to train and predict even a single cell type, irrespective of its population size. This unique feature of CyAnno can be useful for a broad range of studies; e.g. when only one (or few) cell types are of clinical interest, especially those rare cell types that cannot be clustered with unsupervised clustering.
4. CyAnno performed exceptionally good in the identification of rare cells or cell types which are very small in terms of population size.
5. CyAnno can be used for cell label prediction of 'gated' cell type even in the datasets with more than 20 cell types, including closely related cell types. 

**Other important considerations**
1. CyAnno is _not_ an alterative of unsupervised clustering, rather it learns the features of manually gated and mutually exclusive cell types from few training samples and predicts the cell type of live cells from unlabelled FCS file(s).
2. Cell types must be mutually exclusive and both parent and child gated cell types can _not_ be used together for cell type prediction.
3. Each training sample must export all the given cell types. 
4. The prediction accuracy of the algorithm may depend upon the choice of training set, and we recommend the inclusion of samples processed under different batches or stimulation for training the models, to keep the overall training unbiased for any given batch or stimulation. 
5. CyAnno prediction accuracy has not been tested on samples with strong stimulation that can lead to aberrant marker expression profile.
6. Other factor that can affect the prediction accuracy includes the ambiguity in marker expression profile of different cell types.

# Installation and dependencies
CyAnno does not require any installation and can be executed with Command Line Interface (CLI). However, python (3.6 or above) is required with following packages:

```
Session information
-------------
package             version
-------------
numpy               1.18.1
pandas              0.25.3
sklearn             0.20.1
faiss(CPU version)  1.5.1
scipy               1.4.1
xgboost             0.90
pickle
fcsparser           0.2.0          
-------------
Python 3.7.1 (default, Dec 14 2018, 13:28:58) [Clang 4.0.1 (tags/RELEASE_401/final)]
Darwin-18.7.0-x86_64-i386-64bit
4 logical CPU cores, i386
-------------
```
For convinience, we recommend [anaconda](https://anaconda.org/anaconda/python) for python and library installation, as it comes with most of the libraries preloaded. 

# Usage 

All the required parameters and arguments, including path to the input files can be specified together in 'CyAnno.py' file using any text editor or CLI based editor like nano or vim. Finally, CyAnno can be executed as:

```
python CyAnno.py
```

# Step-by-Step instructions
The complete user guide with all the details for running the CyAnno along with the different parameters and input/output file formats are available in [wiki page](https://github.com/abbioinfo/CyAnno/wiki/CyAnno).


# Running example
### Multi-center dataset
An example utilizing the Multi-Center dataset can be executed via CLI (provided all prerequisite libraries are installed):
```
git clone https://github.com/abbioinfo/CyAnno.git
cd CyAnno
python CyAnno.py
```
The expected output of the above code is available [here](https://github.com/abbioinfo/ExpectedOutputs/tree/master/Multi_center_Expected_output).

### Samusik dataset
The following project allows you to download and process the Samusik dataset (10 samples) from public domain in a format required by CyAnno. Total three training samples are used and cell label prediction is performed on remaining seven samples.

```
git clone https://github.com/abbioinfo/CyAnnoSamusik.git
cd CyAnnoSamusik
Rscript Process.R
python CyAnno.py
```
Details can be found [here](https://github.com/abbioinfo/CyAnnoSamusik)

# Cite
If you use CyAnno for your research. Please [cite](https://www.biorxiv.org/content/10.1101/2020.08.28.272559v1)

```
CyAnno: A semi-automated approach for cell type annotation of mass cytometry datasets
Abhinav Kaushik, Diane Dunham, Ziyuan He, Monali Manohar, Manisha Desai, Kari C Nadeau, Sandra Andorf

bioRxiv 2020.08.28.272559; doi: https://doi.org/10.1101/2020.08.28.272559 
```
