<<<<<<< HEAD
# The MultiVerS model

This is the repository for the MultiVerS model for scientific claim verification, described in the NAACL Findings 2022 paper [MultiVerS: Improving scientific claim verification with weak supervision and full-document context](https://arxiv.org/abs/2112.01640).

MultiVers was formerly known as LongChecker. It's the exact same model; we just changed the name to emphasize different aspects of the modeling approach. I'm still in the process of changing the filenames within this repo.

**Repository status**: We provide data, model checkpoints, and inference code for models trained on three scientific claim verification datasets: [SciFact](https://github.com/allenai/scifact), [CovidFact](https://github.com/asaakyan/covidfact), and [HealthVer](https://github.com/sarrouti/HealthVer) (see below for details).

While the SciFact test set is not public, predictions made using the SciFact checkpoint will reproduce the results in the preprint and on the [SciFact leaderboard](https://leaderboard.allenai.org/scifact/submissions/public).

**Update (May 2022)**: Apologies for the delay in getting the training code up. I will make sure that it is available by the time the work is presented at NAACL 2022, if not sooner.

**Disclaimer**: This software is intended to be used as a research protype, and its outputs shouldn't be used to inform any medical decisions.

## Outline

- [Setup](#setup)
- [Running inference](#running-inference)
- [Model checkpoints](#model-checkpoints)
- [Evaluating predictions](#evaluating-predictions)
- [Making predictions for new datasets](#making-predictions-for-new-datasets)

## Setup

We recommend setting up a Conda environment:

```bash
conda create --name multivers python=3.8 conda-build
```

Then, install required packages:

```bash
pip install -r requirements.txt
```

Next, call `conda develop .` from the root of this repository.

Then, download the Longformer checkpoint from which all the fact-checking models are finetuned by doing

```bash
python script/get_checkpoint.py longformer_large_science
```

## Running inference

- First, download the processed versions of the data by running `bash script/get_data.sh`. This will download the CovidFact, HealthVer, and SciFact datasets into the `data` directory.
- Then, download the model checkpoint you'd like to make predictions with using

  ```bash
  python script/get_checkpoint.py [checkpoint_name]
  ```

  Available models are listed in [model checkpoints](#model-checkpoints) section.
- Make predictions using the convenience wrapper script [script/predict.sh](script/predict.sh). This script accepts a dataset name as an argument, and makes predictions using the correct inputs files and model checkpoints for that dataset. For instance, to make predictions on the SciFact test set using the version of MultiVerS trained on Scifact, do:

  ```bash
  bash script/predict.sh scifact
  ```

- For more control over the models and datasets used for prediction, you can use [longchecker/predict.py](longchecker/predict.py).

## Model checkpoints

The following model checkpoints are available. You can download them using `script/get_checkpoint.sh`.

- `fever`: MultiVerS trained on [FEVER](https://fever.ai/).
- `fever_sci`: MultiVerS trained on FEVER, plus two weakly-supervised scientific datasets: [PubMedQA](https://pubmedqa.github.io/) and [Evidence Inference](https://evidence-inference.ebm-nlp.com/).
- `covidfact`: Finetuned on [CovidFact](https://github.com/asaakyan/covidfact), starting from the `fever_sci` checkpoint.
- `healthver`: Finetuned on [HealthVer](https://github.com/sarrouti/HealthVer).
- `scifact`: Finetuned on [SciFact](https://github.com/allenai/scifact).
- `longformer_large_science`: [Longformer](https://github.com/allenai/longformer) pre-trained on a corpus of scientific documents. This model has not been trained on any fact-checking data; it's the starting point for all other models.

You can also download all models by passing `all` to `get_checkpoint.sh`.

## Evaluating predictions

The SciFact test set is private, but the test sets for HealthVer and CovidFact are included in the data download. To evaluate model predictions, use the [scifact-evaluator](https://github.com/allenai/scifact-evaluator) code. Clone the repo, then use the [evaluation script](https://github.com/allenai/scifact-evaluator/blob/master/evaluator/eval.py) located at `evaluator/eval.py`. This script accepts two files:

1. Predictions, as output by `longchecker/predict.py`
2. Gold labels, which are included in the data download.

It will evaluate the predictions with respect to gold and save metrics to a file. See the evaluation script for more details.

## Making predictions for new datasets

You should be able to use one of the MultiVers checkpoints to make predictions for new data. First, you'll need to write a script to convert your dataset to the format described in [data.md](doc/data.md). Then, choose which model you'd like to use. If you don't know which one is best, we'd suggest:

- `fever` for Wikipedia or general text.
- `healthver` for claims specifically about COVID-19.
- `scifact` for biomedical claims generally.

Once you've got your model and dataset chosen, you can make predictions as follows:

```bash
    python longchecker/predict.py \
        --checkpoint_path=checkpoints/[model_name].ckpt \
        --input_file=[path_to_your_claims] \
        --corpus_file=[path_to_your_corpus] \
        --output_file=[output_path]
```
=======
# The MSVEC Dataset
The MSVEC dataset consists of 61 scientific claims obtained from Snopes.com and Politifact.com, their truthfulness labels (true/false/mixed) from these two websites judged by experts, and 61 research papers used for supporting or refuting scientific claims. The search corpus contains 5,244 paper titles and abstracts including 61 papers above. 

# GPT-3.5 Evaluation


# MultiVerS Evaluation and Data

>>>>>>> cc9eb78bf8251a45a5bb92cf45c6bd2fab78747d
