## New Benchmarks for Learning on Non-Homophilous Graphs

Here are the codes accompanying our paper. There are codes to load our proposed datasets, compute our measure of homophily, and train various graph machine learning models in our experimental setup.

### Organization
`main.py` (*Coming Soon*) contains the main experimental scripts.

`dataset.py` loads our datasets.

`models.py` contains implementations for graph machine learning models, though C&S (`correct_smooth.py`, `cs_tune_hparams.py`) is in separate files. Also, `ogbn-proteins` contains code for running GCN and GCN+JK on ogbn-proteins. Running several of the GNN models on larger datasets may require at least 24GB of VRAM. 

`homophily.py` contains functions for computing homophily measures, including the one that we introduce in `our_measure`.

### Datasets
As discussed in the paper, our proposed datasets are "twitch-e", "yelp-chi", "deezer", "fb100", "pokec", "ogbn-proteins", "arxiv-year", and "snap-patents", which can be loaded by `load_nc_dataset` in `dataset.py` by passing in their respective string name. Many of these datasets are included in the `data/` directory, but due to their size, yelp-chi, snap-patents, and pokec are automatically downloaded from a Google drive link when loaded from `dataset.py`. The arxiv-year and ogbn-proteins datasets are downloaded using OGB downloaders. `load_nc_dataset` returns an NCDataset, the documentation for which is also provided in `dataset.py`. It is functionally equivalent to OGB's Library-Agnostic Loader for Node Property Prediction, except for the fact that it returns torch tensors. See the [OGB website](https://ogb.stanford.edu/docs/nodeprop/) for more specific documentation. Just like the OGB function, `dataset.get_idx_split()` returns fixed dataset split for training, validation, and testing. 

When there are multiple graphs (as in the case of twitch-e and fb100), different ones can be loaded by passing in the `sub_dataname` argument to `load_nc_dataset` in `dataset.py`.

twitch-e consists of seven graphs ["DE", "ENGB", "ES", "FR", "PTBR", "RU", "TW"]. In the paper we test on DE.

fb100 consists of 100 graphs. We only include ["Amherst41", "Cornell5", "Johns Hopkins55", "Penn94", "Reed98"] in this repo, although others may be downloaded from [the internet archive](https://archive.org/details/oxford-2005-facebook-matrix). In the paper we test on Penn94.



### Installation instructions

1. Create and activate a new conda environment using python=3.8 (i.e. `conda create --name non-hom python=3.8`) 
2. Activate your conda environment
3. Check CUDA version using `nvidia-smi` 
4. In the root directory of this repository, run `bash install.sh cu110`, replacing cu110 with your CUDA version (i.e. UDA 11 -> cu110, CUDA 10.2 -> cu102, CUDA 10.1 -> cu101). We tested on Ubuntu 18.04, CUDA 11.0.


## Running experiments

1. Make sure a results folder exists in the root directory. 
2. Our experiments are in the `experiments/` directory. There are bash scripts for running methods on single and multiple datasets. Please note that the experiments must be run from the root directory, e.g. (`bash experiments/mixhop_exp.sh snap-patents`)