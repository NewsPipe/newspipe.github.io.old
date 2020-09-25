---
title: 'gpt2-tfx-pipeline'
type: 'PYTHON & TF'
layout: null
---
## Introduction
Gpt2-tfx-pipeline repository contains code for creating a end-end TFX pipeline for GPT-2. The pipeline contains the needed data preprocessing, exporting data from MongoDB, training with pretrained model and deployment to a MLFlow model registry. In the registry there is a possibility to label all produced models. Thus one can image that a service only gets the production model. All pipelines can be orchestrated with either Airflow, Beam or Kubeflow. Tensorboard is supported and can be used for keeping a track during training. 

## Features
- Loading data from a MongoDB. Can be replaced with any database system you want
- Loading data from [wikidump](https://github.com/NewsPipe/wikidump)
- Loading data from text, csv and json files
- Finetune from OpenAI models alias Transfer Learning
- Train from scatch. It is not recommended to do Transfer Learning, when you want to train on datasets with different languages.
- Monitoring training progress with Tensorboard
- Storing models to a MLFlow model registy
- Start pipeline with Apache Beam, Apache Airflow or Kubeflow

## Getting Started

Install package

```git clone https://github.com/steven-mi/tfx-gpt2.git
cd tfx-gpt2
pip indstall tfx-gpt2```

Run pipeline with Apache Beam

```cd examples
python beam-local-example.py
mlflow server --backend-store-uri sqlite:///mlflow.d --default-artifact-root ./mlruns
tensorboard --logdir ./outputs```

Run with Apache Airflow

```cp airflow-local-example.py $AIRFLOW_HOME/dags
cp -r data $AIRFLOW_HOME/dags
airflow scheduler -D
airflow webserver 
cd $AIRFLOW_HOME/dags
mlflow server --backend-store-uri sqlite:///mlflow.d --default-artifact-root ./mlruns
tensorboard --logdir ./outputs```


## List of available models

Look at the [Paper](https://openai.com/blog/better-language-models/) for performance details. Choose the model depending on your resources. Insert the model name as string value for model_name in your pipeline.

| Model Name        | Layers | Comments                                                     |
| ----------------- | ------ | ------------------------------------------------------------ |
| `117M`<br/>`124M` | 12     | Could be trained on a CPU or a single for a reasonable amount of time |
| `345M`<br/>`355M` | 24     | Could be trained on a single GPU for a reasonable amount of time |
| `774M`            | 36     | Not recommended. If you have enough ressources for training this model, then `1558M` should be fine too. |
| `1558M`           | 48     | Largest model trained. Known for super human performance (see: https://openai.com/blog/better-language-models/) |
