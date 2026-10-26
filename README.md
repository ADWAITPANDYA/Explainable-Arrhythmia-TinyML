# RMS — Explainable Arrhythmia Detection with TinyML

A research and edge-deployment project for classifying ECG heartbeats using the MIT-BIH Arrhythmia Database, with an explainability workflow and an INT8 TensorFlow Lite model intended for resource-constrained deployment.

> **Research prototype only.** This project is not a medical device and must not be used to diagnose, treat, or make clinical decisions. Model performance and hardware deployment must be independently validated before any real-world use.

## Contents

- **RMS classifier:** the finalized trained model/package and training/evaluation notebook.
- **TinyML export:** `rms_arrhythmia_int8.tflite`, a compact INT8 TensorFlow Lite model intended for ESP32-class deployment.
- **Explainability:** `RMS_SHAP_MITBIH_Explainability.ipynb`, which evaluates the saved model and uses SHAP to inspect beat-level predictions.
- **Dataset:** MIT-BIH Arrhythmia Database. See the dataset section before adding any dataset files to this public repository.

## Repository structure

```text
RMS/
├── README.md
├── LICENSE
├── .gitignore
├── requirements.txt
├── data/
│   └── README.md                   # Dataset download/source instructions
├── models/
│   ├── README.md                   # Model provenance and expected files
│   ├── rms_arrhythmia_int8.tflite  # TinyML INT8 model
│   └── RMS_FINAL_MODEL_PACKAGE.zip # Optional model package, if appropriate to publish
├── notebooks/
│   ├── final_rms_model_2.ipynb
│   ├── RMS_ESP32_TinyML_CNN_Colab.ipynb
│   └── RMS_SHAP_MITBIH_Explainability.ipynb
├── results/
│   └── README.md                   # Optional metrics/predictions/plots
└── docs/
    └── README.md                   # Hardware and deployment notes
```

The files shown above are the intended layout. Add the actual files you have verified; do not commit placeholder files in place of model artifacts.

## Project workflow

1. **Prepare ECG data** from the official MIT-BIH Arrhythmia Database source.
2. **Train/evaluate RMS** using the training notebook and documented preprocessing.
3. **Export the TinyML model** as a TensorFlow Lite INT8 `.tflite` file.
4. **Explain predictions** with the SHAP notebook using the saved model and test examples.
5. **Validate deployment** on the target board, measuring inference correctness, latency, flash use, and RAM use.

## Output classes

The five AAMI-style heartbeat groups used in this project are:

| Label | Meaning |
|---|---|
| `N` | Normal and other normal-like beats grouped into N |
| `S` | Supraventricular ectopic beats |
| `V` | Ventricular ectopic beats |
| `F` | Fusion beats |
| `Q` | Unknown, paced, or otherwise unclassifiable beats grouped into Q |

Confirm that the class mapping in the published model, notebook, and inference code is identical before release.

## Explainability

The notebook `notebooks/RMS_SHAP_MITBIH_Explainability.ipynb` is intended to:

- load the saved RMS/TFLite model and the matching evaluation samples;
- calculate or inspect predictions without retraining the model;
- use SHAP to estimate which portions of an ECG beat contribute to the model output;
- save explanation artifacts such as plots and prediction tables where configured.

SHAP explanations describe model behavior; they do not establish clinical validity or causality. Include the exact SHAP explainer, background/reference sample strategy, preprocessing, and class mapping in the notebook when publishing.

## Dataset

The model work uses the **MIT-BIH Arrhythmia Database**. Use the official PhysioNet page to review its access conditions, citation requirements, and terms:

- https://physionet.org/content/mitdb/1.0.0/

Prefer linking to the official dataset rather than mirroring the ZIP in this repository unless you have confirmed redistribution is permitted. If redistribution is permitted and you choose to include the archive, note the source, version, checksum, and license/terms in `data/README.md`. Avoid committing extracted records, generated caches, or patient-related data beyond what the dataset terms permit.

## Model files and limitations

- `models/rms_arrhythmia_int8.tflite`: compact INT8 model intended for TinyML inference.
- `models/RMS_FINAL_MODEL_PACKAGE.zip`: optional package of the RMS model and related artifacts. Inspect its contents and remove duplicate checkpoints, temporary files, or anything not intended for public release.
- The repository should document the input shape, sample/window length, sampling rate, normalization, output tensor interpretation, class order, quantization scale/zero-point, and expected preprocessing. Do not guess these values; copy them from the actual exported model and inference code.
- A successful conversion to `.tflite` does **not** by itself prove the model runs on ESP32. Validate operator support, memory, and on-device predictions on the exact board and firmware.

## Reproducibility

Use the notebooks in `notebooks/` with Google Colab or a compatible Python environment. Install only the dependencies required by the notebook. A starter `requirements.txt` is included; pin versions after verifying a clean run.

For a public release, record:
- dataset version and citation;
- train/validation/test split and leakage-prevention method;
- class distribution and any resampling such as SMOTE;
- per-class precision, recall, F1, confusion matrix, and overall metrics;
- model size and quantization details;
- hardware, firmware/runtime version, latency, and memory measurements if edge testing has actually been performed.

Do not claim a target accuracy or on-device performance unless the corresponding experiment has been run and the results are included.

## Suggested citation

If you use the dataset, cite the official MIT-BIH Arrhythmia Database publication and PhysioNet resource as specified on the official dataset page. Add the project paper/preprint citation here when available.

## License

The code and project documentation can be licensed under the repository's chosen license. Dataset and third-party model/data rights are separate; a repository license does not override their terms. Review the license file and dataset conditions before publishing.
