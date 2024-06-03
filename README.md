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

kaggle competitions download -c jigsaw-unintended-bias-in-toxicity-classification

```
## Training
 ### Unintended Bias in Toxicicity Challenge

```bash

python preprocessing_utils.py --train_csv jigsaw_data/jigsaw-unintended-bias-in-toxicity-classification/train.csv --test_csv jigsaw_data/jigsaw-unintended-bias-in-toxicity-classification/test.csv --biased_data
python train.py --config configs/Unintended_bias_toxic_comment_classification_RoBERTa_combined.json

```

### Monitor progress with tensorboard

 ```bash

tensorboard --logdir=./saved

```
## Model Evaluation

### Unintended Bias in Toxicicity Challenge

Evaluated on a novel bias metric that combines different AUC scores to balance overall performance. More information on this metric [here](https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/overview/evaluation).

```bash

python evaluate.py --checkpoint saved/lightning_logs/checkpoints/example_checkpoint.pth --test_csv test.csv

# to get the final bias metric
python model_eval/compute_bias_metric.py

```
