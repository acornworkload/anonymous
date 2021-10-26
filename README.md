# Accurate Open-set Recognition for Memory Workload
These are codes and datasets for "Accurate Open-set Recognition for Memory Workload", submitted to PAKDD, 2022.

## Dataset
We used 2 real-world workload sequence datasets in the experiment.
The following table describes datasets used in our experiment.  

| dataset       | # of known | # of unknown | # of train | # of test of known | # of test of unknown |
|---------------|-----------:|-------------:|-----------:|-------------------:|---------------------:|
| SEC-seq       |         40 |            4 |    586,885 |            293,444 |               93,491 |
| Memtest86-seq |         31 |            3 |    433,334 |            216,696 |               77,018 |

Due to the size limit (about 2TB) and the anonymous policy, we cannot upload the entire raw data for the second data.
We upload the sample raw data and their feature vectors of the second dataset (Memtest86-seq).
 * `raw_sample`: [\[Download\]](https://drive.google.com/file/d/1_NEuUuMqG36v7P7R_XCOl7guPSFYGyXP/view?usp=sharing)
 * `features_intermediate`:[\[Download\]](https://drive.google.com/file/d/1GvE65M-Mz3WI_wBkMeogOpPOMq34wR25/view?usp=sharing)
 * `features_sample`:[\[Download\]](https://drive.google.com/file/d/1Q-RQU-FhQUkfgESu9nDFMTHDPKMSRGk2/view?usp=sharing)

We also upload the full feature vectors of the second dataset (Memtest86-seq).
If you train and test our model, we recommend using the full version of our feature vectors.
* `features_full`:[\[Download\]](https://drive.google.com/file/d/18f0IUuMsPgIjPwdXpwvqxOEmcGIgqUx4/view?usp=sharing)

## Model
We provide  MLP and SVD-based detectors trained for the second dataset in `models` directory.
* `model_mlp.pt`: a trained 2-layer MLP model
* `detectors.npy`: svd-based detectors for all class
* `detectors_threshold.npy`: thresholds for all class


## Code Information
Codes in this directory are implemented by Python 3.7.
This repository contains the code for Acorn, an ACcurate Open-set recognizer for woRkload sequeNce. 
The required Python packages are described in ./requirments.txt.
If pip3 is installed on your system, you can type the following command to
install the required packages.

* The code of Acorn is in this directory.
    * `main.py`: the code related to training a classification model, constructing unknown class detectors, and measuring accuracies for our metrics.
    * `svd_detector.py`: the code related to constructing unknown class detectors and evaluating test samples with the detectors.
    * `model.py`: the code related to a known classification model (MLP).
    * `preprocess.py`: the code that preprocesses raw data and creates feature vectors.
    * `utils/workload_to_sep.py`: the code that takes the cmd field from the raw workloads for the convenience of usage in further steps.
    * `utils/calculate_ngrams.py`: the code that calculates n-grams for the training samples.
    * `utils/distributed_coverage_search.py`: the code that creates n-gram set and calculates n-gram vectors.
    * `utils/bank_access_count.py`: the code that counts access to each bank and creates bank access vectors.
    * `utils/row_col_address_access.py`: the code that counts address access and creates address access vectors.
    * `utils/dataloader.py`: the code related to loading workload sequence data, and extracting feature vectors.

## How to extract feature vectors for sample data

Type the following command to install packages used in our codes:
```bash
    pip install -r requirements.txt
```

Type the following command to extract feature vectors for the cmd field and the address-related fields:
```bash
    python preprocess.py
```
The script will create the following two folders:
```
current directory
├── final_data
└── intermediate_data
```
and reprocessed files are stored in:
```
current directory
└── final_data
    ├── 7-grams
    ├── 11-grams
    ├── 15-grams
    ├── data_split_ids
    ├── bank_access_counts
    └── row_col_address_access_counts
```


## How to train and test unknown workload detection for sample data

Type the following command for training and testing new workload detection:  
```bash
    python main.py --data './final_data_original' --batch_size 128 --learning_rate 0.0001 --alpha 2 --only_test false
```
If you already have models and want to test them, type the following command:
```bash
    python main.py --data './final_data_original' --batch_size 128 --learning_rate 0.0001 --alpha 2 --only_test true
``` 

