# DATA MINING ASSIGNMENT - CN01 - GROUP 1

Develop a recommendation system for e-commerce

For evaluation we use leave-one-out approach. For each user, the last item of the interaction sequence is used as the test data, the item before the last one is used as the validation data, and the remaining data is used for training.


**Amazon Beauty**

| **Model**          | **HitRate@10**     | **NDCG@10**        | **Training time** 
|--------------------|--------------------|--------------------|-------------------|
| BERT4Rec           | 0.0522            | 0.0286            | 1346              | 87             |
| SASRec             | 0.0396             | 0.0213             | 230                    |
| SASRec+      | **0.0770**         | **0.0468**         | 251             



## Usage

Install requirements:
```sh
pip install -r requirements.txt
```

For configuration we use [Hydra](https://hydra.cc/). Parameters are specified in [config files](src/configs/), they can be overriden from the command line. Optionally it is possible to use [ClearML](`https://clear.ml/docs/latest/docs`) for experiments logging (`project_name` and `task_name` should be specified in config to use ClearML).

Example of run via command line:
```sh
cd src
python run.py --config-name=SASRec data_path=../data/ml-1m.txt 
```

There is an [Example notebook](notebooks/Example.ipynb), you can try to run it on datasets from `data/` folder or on your data.


Note: currently (in September 2023) this code doesn't work with `python>=3.10`, because `recommenders` library which is used for evaluation needs `python<3.10`.

## Run code

Scripts to reproduce main results:

```sh
cd src/
# Amazon Beauty

# SASRec+
python run.py --config-name=SASRec data_path=../data/beauty2.txt
# SASRec vanilla
python run.py --config-name=SASRec data_path=../data/beauty2.txt +seqrec_module.loss=bce +dataset.num_negatives=1 dataset.full_negative_sampling=True
# BERT4Rec
python run.py --config-name=BERT4Rec data_path=../data/beauty2.txt