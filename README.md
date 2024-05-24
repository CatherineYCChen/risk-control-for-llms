# risk-control-for-llms
Calibrating Generative Models to achieve safety guarantee with limited annotated data

## Install Detoxify

Install dependencies
```bash
# clone project

git clone https://github.com/unitaryai/detoxify

# create virtual env

python3 -m venv toxic-env
source toxic-env/bin/activate

# install project
pip install -e detoxify

# or for training
pip install -e 'detoxify[dev]'

cd detoxify

 ```

## Download Kaggle Datasets

 - Create New API Token in your Kaggle account - this will download a kaggle.json file

 - make sure the file is located in ~/.kaggle

 ```bash

# create data directory

mkdir jigsaw_data
cd jigsaw_data

# download data

kaggle competitions download -c jigsaw-toxic-comment-classification-challenge

kaggle competitions download -c jigsaw-unintended-bias-in-toxicity-classification

kaggle competitions download -c jigsaw-multilingual-toxic-comment-classification

```
## Training
 ### Toxic Comment Classification Challenge

```bash

# combine test.csv and test_labels.csv
python preprocessing_utils.py --test_csv jigsaw_data/jigsaw-toxic-comment-classification-challenge/test.csv --update_test

python train.py --config configs/Toxic_comment_classification_BERT.json

```
 ### Unintended Bias in Toxicicity Challenge

```bash

python train.py --config configs/Unintended_bias_toxic_comment_classification_RoBERTa_combined.json

```
 ### Multilingual Toxic Comment Classification

```bash

# combine test.csv and test_labels.csv
python preprocessing_utils.py --test_csv jigsaw_data/jigsaw-multilingual-toxic-comment-classification/test.csv --update_test

python train.py --config configs/Multilingual_toxic_comment_classification_XLMR.json

```

### Monitor progress with tensorboard

 ```bash

tensorboard --logdir=./saved

```
## Model Evaluation

### Toxic Comment Classification Challenge

Evaluated on the mean AUC score of all the labels.

```bash

python evaluate.py --checkpoint saved/lightning_logs/checkpoints/example_checkpoint.pth --test_csv test.csv

```
### Unintended Bias in Toxicicity Challenge

Evaluated on a novel bias metric that combines different AUC scores to balance overall performance. More information on this metric [here](https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/overview/evaluation).

```bash

python evaluate.py --checkpoint saved/lightning_logs/checkpoints/example_checkpoint.pth --test_csv test.csv

# to get the final bias metric
python model_eval/compute_bias_metric.py

```
### Multilingual Toxic Comment Classification

Evaluated on the AUC score of the main toxic label.

```bash

python evaluate.py --checkpoint saved/lightning_logs/checkpoints/example_checkpoint.pth --test_csv test.csv

```
