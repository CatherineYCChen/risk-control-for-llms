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


