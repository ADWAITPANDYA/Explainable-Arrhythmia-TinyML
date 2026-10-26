# RMS v1 — Explainable Arrhythmia Detection with TinyML

**An Explainable ECG Arrhythmia Classification Framework Using the MIT-BIH Arrhythmia Database, SHAP-Based Model Interpretation, and INT8 Quantization for Edge Deployment**

[![Python 3.10+](https://img.shields.io/badge/Python-3.10%2B-blue)](https://www.python.org/)
[![TensorFlow](https://img.shields.io/badge/TensorFlow-TFLite-orange)](https://www.tensorflow.org/)
[![Dataset](https://img.shields.io/badge/Dataset-MIT--BIH-green)](https://physionet.org/content/mitdb/1.0.0/)
[![Explainability](https://img.shields.io/badge/Explainability-SHAP-purple)](https://shap.readthedocs.io/)
[![Deployment](https://img.shields.io/badge/Edge%20AI-ESP32%20%7C%20Raspberry%20Pi-lightgrey)](https://www.espressif.com/)

---

## Overview

**RMS** is a research project focused on ECG heartbeat classification, model explainability, and lightweight inference for resource-constrained edge devices.

The framework uses the **MIT-BIH Arrhythmia Database** to develop and evaluate an arrhythmia classification model. It also incorporates SHAP-based explainability to investigate which portions of an ECG heartbeat contribute to model predictions and an INT8 TensorFlow Lite model for exploring TinyML deployment.

### Key Components

| Component | Purpose |
|---|---|
| **RMS Classifier** | Classifies ECG heartbeat segments into five AAMI-style heartbeat groups |
| **MIT-BIH Dataset** | Provides ECG recordings and annotated heartbeats for model development and evaluation |
| **SHAP Explainability** | Investigates feature contributions to individual model predictions |
| **INT8 TinyML Model** | Provides a quantized TensorFlow Lite model for resource-constrained inference experiments |
| **ESP32 Integration** | Targets deployment on an ESP32 development board, subject to runtime and resource compatibility |
| **Raspberry Pi Integration** | Provides a potential edge-inference platform for model evaluation |

The project combines classification, explainability, and model compression in an experimental ECG analysis pipeline. Hardware compatibility, inference performance, and clinical validity must be established through appropriate testing rather than assumed from model conversion or offline results.

---

## Features

- ECG heartbeat classification using the MIT-BIH Arrhythmia Database
- Five-class heartbeat categorization
- Model training and evaluation through Google Colab notebooks
- SHAP-based interpretation of model predictions
- Prediction analysis and export of evaluation results
- INT8 TensorFlow Lite model for lightweight inference experiments
- ESP32-oriented TinyML model development
- Raspberry Pi inference exploration
- Reproducible experimentation through notebooks and saved model artifacts

---

## Project Structure

```text
Explainable-Arrhythmia-TinyML/
├── README.md
├── dataset/
│   └── mit-bih-arrhythmia-database-1.0.0.zip
├── models/
│   └── rms_arrhythmia_int8.tflite
└── Notebooks/
    ├── final_rms_model_2.ipynb
    ├── RMS_ESP32_TinyML_CNN_Colab.ipynb
    └── RMS_SHAP_MITBIH_Explainability.ipynb
```

---

## Project Workflow

1. **Dataset preparation:** Load ECG recordings and annotations from the MIT-BIH Arrhythmia Database.
2. **Preprocessing:** Prepare heartbeat segments using the preprocessing and normalization implemented in the training pipeline.
3. **Model training and evaluation:** Train or load the RMS classifier and evaluate its predictions on the configured dataset split.
4. **Explainability analysis:** Use SHAP to investigate the contributions of ECG input features to model predictions.
5. **Model compression:** Convert the compatible model to TensorFlow Lite INT8 format for lightweight inference experiments.
6. **Edge deployment:** Test the exported model on the intended hardware and measure prediction consistency, latency, memory use, and compatibility.

---

## Output Classes

The model uses five heartbeat categories commonly associated with AAMI-style ECG classification.

| Label | Description |
|---|---|
| `N` | Normal and other normal-like beats grouped into N |
| `S` | Supraventricular ectopic beats |
| `V` | Ventricular ectopic beats |
| `F` | Fusion beats |
| `Q` | Unknown, paced, or otherwise unclassifiable beats grouped into Q |


---

## Explainability with SHAP

The notebook `RMS_SHAP_MITBIH_Explainability.ipynb` provides the explainability workflow for inspecting RMS predictions.

The workflow is intended to:

- Load the saved model and compatible evaluation samples.
- Generate predictions for ECG heartbeat segments.
- Apply SHAP to investigate which input features contribute to the model's output.
- Examine explanations for selected predictions and heartbeat classes.
- Export prediction results and explanation artifacts where configured.

SHAP explanations help describe the behavior of a trained model. They do not establish physiological causality, clinical validity, or diagnostic reliability.

For reproducibility, document the explainer used, background/reference samples, preprocessing, target output, and class mapping.

---

## TinyML and Edge Deployment

The project includes the quantized TensorFlow Lite model:

`models/rms_arrhythmia_int8.tflite`

The model is intended for experiments involving lightweight ECG inference on resource-constrained hardware.

### Target Platforms

- **ESP32:** Evaluate compatibility with the selected TensorFlow Lite Micro runtime, supported operators, flash capacity, and available RAM.

### Deployment Validation

Before claiming successful hardware deployment, verify:

- Model loading and interpreter initialization
- Input tensor shape, data type, scaling, and preprocessing
- Output tensor interpretation and class mapping
- Prediction agreement with the reference model
- Inference latency and peak memory usage
- Firmware/runtime compatibility and available flash/RAM

A successful `.tflite` conversion alone does not establish that the model runs correctly on an ESP32 or Raspberry Pi. Report hardware results only after running and validating the model on the actual device.

---

## Dataset

This project uses the **MIT-BIH Arrhythmia Database**, distributed through PhysioNet.

Official dataset: https://physionet.org/content/mitdb/1.0.0/

Review the dataset's usage conditions and citation requirements before redistribution. If the dataset archive is included in this repository, confirm that its redistribution is permitted under the applicable terms. Otherwise, provide the official source link and instructions for obtaining it.

---

## Reproducibility and Evaluation

Run the relevant notebooks in Google Colab or a compatible local Python environment. Install the dependencies required by each notebook and use the correct dataset and model paths.

For a reproducible research evaluation, document:

- Dataset version and citation
- Preprocessing, heartbeat segmentation, and normalization
- Training, validation, and test split methodology
- Patient-level separation or other leakage-prevention measures, where applicable
- Class distribution and any resampling methods used
- Per-class precision, recall, F1-score, and confusion matrix
- Overall evaluation metrics and the evaluation dataset
- Model size, quantization configuration, and inference requirements
- SHAP explainer configuration and reference samples
- Hardware specifications and measured deployment results, if tested

---

## Limitations and Research Disclaimer

RMS is an experimental research prototype for ECG classification and explainability. It is **not a certified medical device** and must not be used to diagnose, treat, or make clinical decisions.

Model predictions may be incorrect, and performance on a research dataset does not guarantee performance on unseen patients or real-world ECG recordings. Further validation, independent testing, and appropriate clinical evaluation would be required before any medical application.

---

## License and Attribution

Choose and include a license appropriate for the project code. The MIT-BIH Arrhythmia Database and any third-party software or models may have separate terms and attribution requirements. A repository license does not override those terms.

Please cite the original dataset and relevant model or explainability methods when using this project in research.
