# Explainability-Guided Weak Localization for Chest X-Ray Interpretation

This repository contains the implementation and experimental results for a Master's research project on explainability-guided weak localization of thoracic pathologies in chest X-rays.

The study investigates the impact of:

* ImageNet vs. RadImageNet pretraining
* Grad-CAM vs. Grad-CAM++ explainability methods
* Post-processing Tuning for (Threshold and Box strategy)

using DenseNet-121 models trained on CheXpert and evaluated on CheXLocalize.

## Project Structure

```text
xai_results/          # Evaluation results, figures, and comparisons
model_outputs/        # Trained models and checkpoints
csvs/                 # Dataset split files
weights_of_DenseNet121/  # RadImageNet pretrained weights
train_imagenet.ipynb
train_radimagenet.ipynb
xai_pipeline.ipynb
config.yaml
requirements.txt
```

## Datasets

This project uses:

* CheXpert:https://stanfordmlgroup.github.io/competitions/chexpert/
* CheXLocalize:https://stanfordaimi.azurewebsites.net/datasets/23c56a0d-15de-405b-87c8-99c30138950c

Please download the datasets from their official sources and configure the paths accordingly.

## Installation

```bash
git clone https://github.com/Afrah319/Explainability-Guided-Weak-Localization-for-Chest-X-Ray.git
cd Explainability-Guided-Weak-Localization-for-Chest-X-Ray

pip install -r requirements.txt
```

## Usage

### Train Models

```bash
jupyter notebook train_imagenet.ipynb
```

or

```bash
jupyter notebook train_radimagenet.ipynb
```

### Run Explainability Evaluation

```bash
jupyter notebook xai_pipeline.ipynb
```

## Repository Contents

* Model training pipelines
* Explainability evaluation pipeline
* Localization analysis and comparisons
* Experimental results and visualizations

## License

This repository is provided for research and educational purposes.
