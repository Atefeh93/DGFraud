<p align="center">
    <br>
    <a href="https://image.flaticon.com/icons/svg/1671/1671517.svg">
        <img src="https://github.com/safe-graph/DGFraud/blob/master/DGFraud_logo.png" width="400"/>
    </a>
    <br>
<p>
<p align="center">
    <a href="https://travis-ci.org/github/safe-graph/DGFraud">
        <img alt="PRs Welcome" src="https://travis-ci.org/safe-graph/DGFraud.svg?branch=master">
    </a>
    <a href="https://github.com/safe-graph/DGFraud/blob/master/LICENSE">
        <img alt="GitHub" src="https://img.shields.io/github/license/safe-graph/DGFraud">
    </a>
    <a href="https://github.com/safe-graph/DGFraud/archive/master.zip">
        <img alt="Downloads" src="https://img.shields.io/github/downloads/safe-graph/DGFraud/total">
    </a>
    <a href="https://github.com/safe-graph/DGFraud/releases">
        <img alt="GitHub release" src="https://img.shields.io/github/v/release/safe-graph/DGFraud?include_prereleases">
    </a>
</p>

<h3 align="center">
<p>A Deep Graph-based Toolbox for Fraud Detection
</h3>

**Introduction** 

**DGFraud** is a Graph Neural Network (GNN) based toolbox for fraud detection. It integrates the implementation & comparison of state-of-the-art GNN-based fraud detection models. It also includes several utility functions such as graph preprocessing, graph sampling, and performance evaluation. The introduction of implemented models can be found [here](#implemented-models). <!-- (Add introduction blogs links). -->

We welcome contributions on adding new fraud detectors and extending the features of the toolbox. Some of the planned features are listed in [TODO list](#todo-list). 

If you use the toolbox in your project, please cite the [paper](https://arxiv.org/abs/2005.00625) below and the [algorithms](#implemented-models) you used :
```bibtex
@inproceedings{liu2020alleviating,
  title={Alleviating the Inconsistency Problem of Applying Graph Neural Network to Fraud Detection},
  author={Liu, Zhiwei and Dou, Yingtong and Yu, Philip S. and Deng, Yutong and Peng, Hao},
  booktitle={Proceedings of the 43nd International ACM SIGIR Conference on Research and Development in Information Retrieval},
  year={2020}
}
```

**Useful Resources**
- [Graph-based Fraud Detection Paper List](https://github.com/safe-graph/graph-fraud-detection-papers) 
- [Awesome Fraud Detection Papers](https://github.com/benedekrozemberczki/awesome-fraud-detection-papers)
- [Attack and Defense Papers on Graph Data](https://github.com/safe-graph/graph-adversarial-learning-literature)
- [PyOD: A Python Toolbox for Scalable Outlier Detection (Anomaly Detection)](https://github.com/yzhao062/pyod)
- [PyODD: An End-to-end Outlier Detection System](https://github.com/datamllab/pyodds)
- [DGL: Deep Graph Library](https://github.com/dmlc/dgl)
- [Outlier Detection DataSets (ODDS)](http://odds.cs.stonybrook.edu/)

**Table of Contents**
- [Installation](#installation)
- [User Guide](#user-guide)
- [Implemented Models](#implemented-models)
- [Model Comparison](#model-comparison)
- [TODO List](#todo-list)
- [How to Contribute](#how-to-contribute)


## Installation
```bash
git clone https://github.com/safe-graph/DGFraud.git
cd DGFraud
python setup.py install
```
### Requirements
```bash
* python 3.6, 3.7
* tensorflow>=1.14.0,<2.0
* numpy>=1.16.4
* scipy>=1.2.0
```
### Dataset

#### DBLP
We uses the pre-processed DBLP dataset from [Jhy1993/HAN](https://github.com/Jhy1993/HAN)
You can run the FdGars, Player2Vec, GeniePath and GEM based on the DBLP dataset.
Unzip the archive before using the dataset:
```bash
cd dataset
unzip DBLP4057_GAT_with_idx_tra200_val_800.zip
```

#### Example dataset
We implement example graphs for SemiGNN, GAS and GEM in `data_loader.py`. Because those models require unique graph structures or node types, which cannot be found in opensource datasets.


#### Yelp dataset
For [GraphConsis](https://arxiv.org/abs/2005.00625), we preprocessed [Yelp Spam Review Dataset](http://odds.cs.stonybrook.edu/yelpchi-dataset/) with reviews as nodes and three relations as edges. There are two kinds of node features, the 100-dimension Bag-of-words feature and 32-dimension handcrafted feature from [SpEagle paper](https://www.andrew.cmu.edu/user/lakoglu/pubs/15-kdd-collectiveopinionspam.pdf).

The dataset is located at `/dataset/YelpChi.zip`. It is organized as follows:
- `adj_lists.pickle` contains a list `[rur, rtr, rsr]` which includes adjacency lists of three homo-graphs defined in [GraphConsis](https://arxiv.org/abs/2005.00625) paper;
- `bow_feature.pickle` contains the Bag-of-words features in `scipy.sparse.coo_matrix` format;
- `hand_feature.pickle` contains the handcrafted features in `scipy.sparse.coo_matrix` format;
- `labels.pickle` contains the ground truth of reviews in `numpy.array` format. `1` represents spam and `0` represents benign.

To get the complete metadata of the Yelp dataset, please send an email to [ytongdou@gmail.com](mailto:ytongdou@gmail.com) for inquiry.

## User Guide

### Running the example code
You can find the implemented models in `algorithms` directory. For example, you can run Player2Vec using:
```bash
python Player2Vec_main.py 
```
You can specify parameters for models when running the code.

### Running on your datasets
Have a look at the load_data_dblp() function in utils/utils.py for an example.

In order to use your own data, you have to provide:
* adjacency matrices or adjlists (for GAS);
* a feature matrix
* a label matrix
then split feature matrix and label matrix into testing data and training data.

You can specify a dataset as follows:
```bash
python xx_main.py --dataset your_dataset 
```
or by editing xx_main.py

### The structure of code
The repository is organized as follows:
- `algorithms/` contains the implemented models and the corresponding example code;
- `base_models/` contains the basic models (GCN);
- `dataset/` contains the necessary dataset files;
- `utils/` contains:
    * loading and splitting the data (`data_loader.py`);
    * contains various utilities (`utils.py`).


## Implemented Models

| Model  | Paper  | Venue  | Reference  |
|-------|--------|--------|--------|
| **SemiGNN** | [A Semi-supervised Graph Attentive Network for Financial Fraud Detection](https://arxiv.org/pdf/2003.01171)  | ICDM 2019  | [BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/semignn.txt) |
| **Player2Vec** | [Key Player Identification in Underground Forums over Attributed Heterogeneous Information Network Embedding Framework](http://mason.gmu.edu/~lzhao9/materials/papers/lp0110-zhangA.pdf)  | CIKM 2019  | [BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/player2vec.txt)|
| **GAS** | [Spam Review Detection with Graph Convolutional Networks](https://arxiv.org/abs/1908.10679)  | CIKM 2019 | [BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/gas.txt) |
| **FdGars** | [FdGars: Fraudster Detection via Graph Convolutional Networks in Online App Review System](https://dl.acm.org/citation.cfm?id=3316586)  | WWW 2019 | [BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/fdgars.txt) |
| **GeniePath** | [GeniePath: Graph Neural Networks with Adaptive Receptive Paths](https://arxiv.org/abs/1802.00910)  | AAAI 2019 | [BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/geniepath.txt)  |
| **GEM** | [Heterogeneous Graph Neural Networks for Malicious Account Detection](https://arxiv.org/pdf/2002.12307.pdf)  | CIKM 2018 |[BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/gem.txt) |
<!--| **HACUD** | [Cash-Out User Detection Based on Attributed Heterogeneous Information Network with a Hierarchical Attention Mechanism](https://aaai.org/ojs/index.php/AAAI/article/view/3884)  | AAAI 2019 |  Bibtex |-->
<!--| **GraphConsis** | Alleviating the Inconsistency Problem of Applying Graph Neural Network to Fraud Detection  | SIGIR 2020  | [BibTex](https://github.com/safe-graph/DGFraud/blob/master/reference/graphconsis.txt) |-->

## Model Comparison
| Model  | Application  | Graph Type  | Base Model  |
|-------|--------|--------|--------|
| **SemiGNN** | Financial Fraud  | Heterogeneous   | GAT, LINE, DeepWalk |
| **Player2Vec** | Cyber Criminal  | Heterogeneous | GAT, GCN|
| **GAS** | Opinion Fraud  | Heterogeneous | GCN, GAT |
| **FdGars** |  Opinion Fraud | Homogeneous | GCN |
| **GeniePath** | Financial Fraud | Homogeneous | GAT  |
| **GEM** | Financial Fraud  | Heterogeneous |GCN |
<!--| **HACUD** |  |  |   |-->
<!--| **GraphConsis** | Opinion Fraud  | Homogeneous   | GraphSAGE |-->

## TODO List
- Integrating the Yelp dataset with multiple models
- GraphConsis Implementation
- Implementing mini-batch training
- The log loss for GEM model
- Time-based sampling for GEM
- Add sampling methods
- Benchmarking SOTA models
- Scalable implementation
- TensorFlow 2.0+ implementation
- Pytorch version

## How to Contribute
You are welcomed to contribute to this open-source toolbox. The detailed instructions will be released soon. Currently, you can create issues or send email to [ytongdou@gmail.com](mailto:ytongdou@gmail.com) for inquiry.

