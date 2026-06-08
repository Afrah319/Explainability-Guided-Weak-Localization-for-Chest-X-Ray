# Explainability-Guided-Weak-Localization-for-Chest-X-Ray
Explainability-guided weak localisation of thoracic pathologies on chest X-rays. Compares ImageNet vs RadImageNet pretraining and Grad-CAM vs Grad-CAM++ on CheXpert/CheXLocalize using a calibrated post-processing pipeline.

A systematic 2×2 factorial study comparing ImageNet vs. RadImageNet pretraining and Grad-CAM vs. Grad-CAM++ explainability methods for weak localization of thoracic pathologies on chest X-rays, evaluated against radiologist-annotated ground truth from the CheXLocalize dataset.

Overview
This repository contains the full experimental pipeline for a Master's thesis investigating whether domain-specific pretraining (RadImageNet) improves saliency-based localization over general-purpose pretraining (ImageNet) in a DenseNet-121 classifier trained on CheXpert pathologies.
The study evaluates four explainability pipelines across six pathologies from CheXLocalize, using both pointing-game and overlap-based metrics. A calibrated post-processing threshold selection procedure is applied to ensure fair cross-pipeline comparison.

Study Design
FactorLevelsPretrainingImageNet · RadImageNetExplainerGrad-CAM · Grad-CAM++Pipelines4 combinations
Evaluated Pathologies (from CheXLocalize)

Atelectasis
Cardiomegaly
Edema
Enlarged Cardiomediastinum
Lung Opacity
Pleural Effusion

Evaluation Metrics

Pointing Game (Hit Rate) — energy-based and centroid-based
IoU — Intersection over Union against radiologist segmentations
Energy-Based Pointing Game — proportion of saliency energy within the ground-truth bounding box
Recall — bounding box coverage of saliency peaks


Repository Structure
├── xai_results/
│   ├── comparison/             # Cross-pipeline statistical comparisons
│   │   ├── charts/             # Publication-ready figures
│   │   ├── extra_analysis/     # Threshold sensitivity, energy analysis, bbox comparisons
│   │   └── qualitative/        # Qualitative saliency overlays per pathology
│   ├── imagenet/               # ImageNet pretraining results
│   │   ├── heatmaps/
│   │   │   ├── val_gradcam/    # Grad-CAM saliency maps (.npy)
│   │   │   └── val_gradcampp/  # Grad-CAM++ saliency maps (.npy)
│   │   └── results/            # Per-class and per-pair evaluation CSVs
│   └── radimagenet/            # RadImageNet pretraining results
│       ├── heatmaps/
│       │   ├── val_gradcam/
│       │   └── val_gradcampp/
│       └── results/
├── model_outputs/
│   ├── imagenet/seed_42/
│   │   ├── checkpoints/        # best.pt — trained DenseNet-121 (ImageNet)
│   │   └── results/            # Classification metrics, training history, class distributions
│   └── radimagenet/seed_42/
│       ├── checkpoints/        # best.pt — trained DenseNet-121 (RadImageNet)
│       └── results/
├── weights_of_DenseNet121/
│   └── DenseNet121.pt          # RadImageNet pretrained backbone weights
├── csvs/
│   ├── train_visualCheXbert.csv
│   ├── valid.csv
│   └── test_labels.csv
├── config.yaml                 # Training configuration
├── requirements.txt
├── train_imagenet.ipynb        # Training notebook — ImageNet backbone
├── train_radimagenet.ipynb     # Training notebook — RadImageNet backbone
└── xai_pipeline.ipynb          # Full XAI evaluation pipeline 

Model Architecture

Backbone: DenseNet-121
Pretraining Sources: ImageNet-1k · RadImageNet (medical imaging database)
Classifier Head: Multi-label sigmoid (14 CheXpert pathologies)
Calibration: Temperature scaling applied post-training
Seed: 42 (all experiments)


Datasets
CheXpert
Training and classification evaluation dataset.

Download from the official source:
https://stanfordmlgroup.github.io/competitions/chexpert/

Place the downloaded dataset such that the paths in config.yaml resolve correctly. The CSV splits used in this study are included in the csvs/ directory.
CheXLocalize
Radiologist-annotated segmentation dataset used for localization evaluation.

Download from the official repository:
https://github.com/rajpurkarlab/cheXlocalize

Place the downloaded data at the path expected by xai_pipeline.ipynb.
Expected data structure after setup:
data/
├── CheXpert/
│   ├── train/
│   ├── valid/
│   └── test/
└── chexlocalize/
    ├── CheXpert/
    └── gt_annotations_val.json

Installation
bashgit clone https://github.com/Afrah319/Explainability-Guided-Weak-Localization-for-Chest-X-Ray.git
cd Explainability-Guided-Weak-Localization-for-Chest-X-Ray

pip install -r requirements.txt
Requirements: Python 3.10 · PyTorch 2.x · torchvision · numpy · pandas · matplotlib · scikit-learn · opencv-python · pycocotools

Usage
1. Training
Run either notebook depending on the pretraining backbone:
bashjupyter notebook train_imagenet.ipynb
# or
jupyter notebook train_radimagenet.ipynb
Training configuration (learning rate, batch size, epochs, augmentations) is defined in config.yaml.
2. XAI Evaluation Pipeline
The full evaluation pipeline is implemented in xai_pipeline.ipynb — a 50-cell notebook covering:

Saliency map generation (Grad-CAM and Grad-CAM++)
Per-pipeline threshold optimization (IoU-vs-threshold curves)
Bounding box strategy comparison
Pointing game and IoU computation against CheXLocalize annotations
Bootstrap statistical tests (pretraining effect · explainer effect)
Publication-ready figure generation

bashjupyter notebook xai_pipeline.ipynb
3. Using Pretrained Weights
The RadImageNet pretrained DenseNet-121 backbone is included in weights_of_DenseNet121/DenseNet121.pt.
Fine-tuned model checkpoints (after CheXpert training) are in model_outputs/.

Results Summary
Evaluation was performed on the CheXLocalize validation set against radiologist-annotated bounding boxes.
PipelinePointing Game (mean)ImageNet + Grad-CAMsee xai_results/comparison/ImageNet + Grad-CAM++see xai_results/comparison/RadImageNet + Grad-CAMsee xai_results/comparison/RadImageNet + Grad-CAM++see xai_results/comparison/
Full quantitative results, confidence intervals, and statistical test outputs are available in:

xai_results/comparison/set1_pretraining_BOOTSTRAP.csv
xai_results/comparison/set2_explainer_BOOTSTRAP.csv
xai_results/comparison/phase7_test_perpair_master.csv

Qualitative saliency overlays for all six pathologies are in xai_results/comparison/qualitative/.

Statistical Analysis

Pretraining effect: Paired bootstrap test (ImageNet vs. RadImageNet, matched by explainer)
Explainer effect: Paired bootstrap test (Grad-CAM vs. Grad-CAM++, matched by pretraining)
Threshold selection: Grid search over the validation set; locked pipeline applied to test set
Robustness check: Rank stability analysis across threshold perturbations


Acknowledgements

CheXpert — Stanford ML Group
CheXLocalize — Saporta et al., Nature Machine Intelligence (2022)
RadImageNet — Mei et al., Radiology: Artificial Intelligence (2022)

