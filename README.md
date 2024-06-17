# risk-control-for-llms
Calibrating Generative Models to achieve safety guarantee with limited annotated data

## Install Detoxify


### Install dependencies
```bash
# clone project

git clone https://github.com/unitaryai/detoxify

# create virtual env

python3 -m venv toxic-env
source toxic-env/bin/activate

# install project for training
pip install -e 'detoxify[dev]'

cd detoxify

 ```
### Download Kaggle Datasets

 - Create New API Token in your Kaggle account - this will download a kaggle.json file

 - make sure the file is located in ~/.kaggle

 ```bash

# create data directory

mkdir jigsaw_data
cd jigsaw_data

# download data

kaggle competitions download -c jigsaw-unintended-bias-in-toxicity-classification

```
### Training
#### Unintended Bias in Toxicicity Challenge

```bash

# generate a biased sample for re-training
python preprocessing_utils.py --train_csv jigsaw_data/jigsaw-unintended-bias-in-toxicity-classification/train.csv --test_csv jigsaw_data/jigsaw-unintended-bias-in-toxicity-classification/test_public_expanded.csv --biased_data
# Note: modified preprocessing_utils.py

# re-train biased model
python train.py --config configs/Unintended_bias_toxic_comment_classification_RoBERTa_combined.json
# Note: modified config file: Unintended_bias_toxic_comment_classification_RoBERTa_combined.json

```

#### Monitor progress with tensorboard

 ```bash

tensorboard --logdir=./saved

```
### Model Evaluation

#### Unintended Bias in Toxicicity Challenge

Evaluated on a novel bias metric that combines different AUC scores to balance overall performance. More information on this metric [here](https://www.kaggle.com/c/jigsaw-unintended-bias-in-toxicity-classification/overview/evaluation).

```bash

python evaluate.py --checkpoint saved/lightning_logs/checkpoints/example_checkpoint.pth --test_csv test.csv

# to get the final bias metric
python model_eval/compute_bias_metric.py

```

## Install Llama
Follow steps in [meta-llama repository](https://github.com/meta-llama/llama) 

## Prompt Risk Control
Create environment and install dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt --no-deps
```

### Install Crossing Probability
```bash
git clone git@github.com:mosco/crossing-probability.git
```
(For Mac) If you don't have swig and libfftw3 installed, install it with `brew update && brew install swig fftw`.

(For Ubuntu) If you don't have swig and libfftw3 installed, install it with `sudo apt update && sudo apt install -y swig libfftw3-dev`.

Note: Add
```c++
#include <limits>
```
to `src/common.cc` and
```c++
#include <iostream>
#include <iterator>
```
to `src/common.hh` to get it to compile.

Make the C++ library and install the Python package:
```bash
# make the C++ Library

cd crossing-probability
make

# install the Python package

make python
pip install . 
```
