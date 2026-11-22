# PolyGuard Finance Pilot

This repository contains a reproducible pilot comparing 0%, 50%, and 100% replacement of unsafe training texts with their stored rewritten versions.

The base cases, training size, class balance, model settings, and decision threshold remain fixed.

## Notebook

[Open the notebook in Colab](https://colab.research.google.com/github/STATS201-Autumn2026/Qingyue-PS1-polyguard-finance/blob/main/PolyGuard_Finance_Original_and_Rewritten_Training_Text_Qingyue.ipynb)

Notebook filename:

`PolyGuard_Finance_Original_and_Rewritten_Training_Text_Qingyue.ipynb`

## Reproduce the Pilot

1. Open the notebook in Google Colab.
2. Use a standard CPU runtime.
3. Run the setup cell to install the specified packages.
4. Restart the session if requested, then run the remaining cells in order.
5. Review the data checks, validation results, test results, and plot.
6. Run the final cell to download the output ZIP.

No GPU, API key, or Hugging Face token is required.

The notebook reads public data and writes files in its runtime. It does not upload to or modify remote repositories.

## Environment

The setup cell installs:

- NumPy 2.2.6
- pandas 2.2.3
- scikit-learn 1.6.1
- Matplotlib 3.10.3
- requests 2.32.4

The recorded execution environment is available in `results/experiment_config.json`.

## Data and Versions

Processed course dataset:

[DKU-STATS201-Autumn2026/Qingyue-PS1-polyguard-finance](https://huggingface.co/datasets/DKU-STATS201-Autumn2026/Qingyue-PS1-polyguard-finance/tree/30b7ebca5c79287d7ca03359995e588d377ab018)

Processed dataset revision:

`30b7ebca5c79287d7ca03359995e588d377ab018`

The notebook currently reconstructs the processed release from the original financial input files:

- Original dataset: [Virtue-AI-HUB/PolyGuard](https://huggingface.co/datasets/Virtue-AI-HUB/PolyGuard)
- Original revision: `fa5b3948e6e23f0611e2382561c1434971550b65`
- Selected directory: `finance/Input`

The executable notebook and uploaded pilot outputs were inspected at code revision:

`0da356e4dff786bd3fddde10daf9cb7334cb8ed1`

This revision identifies the code snapshot before this README update.

## Method

- Model: TF-IDF with Logistic Regression.
- Random seed: 42.
- Decision threshold: 0.5.
- Training replacement ratios: 0%, 50%, and 100% of unsafe cases.
- Held-out unit: normalized safety rule.
- Original and rewritten versions of a case remain in the same split.
- Safe texts remain unchanged across conditions.

Only selected text is used as a predictive feature. Identifiers, source names, rule identifiers, and rewrite availability are not model inputs.

## Expected Checks

A successful run should load:

| Split | Safe | Unsafe | Total |
|---|---:|---:|---:|
| Train | 2,470 | 2,470 | 4,940 |
| Validation | 525 | 525 | 1,050 |
| Test | 535 | 535 | 1,070 |

The notebook should print:

```text
Checks passed: unique cases, complete text, balanced labels, and zero rule overlap.
```

The three conditions should contain 0, 1,235, and 2,470 rewritten unsafe training cases, respectively.

## Evaluation and Outputs

Primary metrics:

- Unsafe recall: TP / (TP + FN).
- Safe false-positive rate: FP / (FP + TN).

AUC is a secondary diagnostic. An always-safe baseline is included.

The `results` directory contains:

- `training_conditions.csv`
- `training_assignments.csv`
- `validation_results.csv`
- `test_results.csv`
- `validation_predictions.csv`
- `test_predictions.csv`
- `experiment_config.json`
- `pilot_results.png`

The output archive also contains processed data files and provenance information.

## Limitations

This is an exploratory, single-seed pilot using synthetic examples.

Only unsafe inputs have stored rewritten versions. Rewrites have not been independently verified as perfectly meaning-preserving, and no expert audit of all labels was conducted.

These results do not establish an optimal replacement ratio for real-world financial deployment.

## Data Attribution

The underlying dataset was created by the original PolyGuard authors. The processed data follow the source dataset's CC BY 4.0 license.

Associated paper:

Kang et al. (2025). [PolyGuard: Massive Multi-Domain Safety Policy-Grounded Guardrail Dataset](https://proceedings.nips.cc/paper_files/paper/2025/hash/11ed9cdc955e23684a1beae9cb8da059-Abstract-Datasets_and_Benchmarks_Track.html). NeurIPS 2025 Datasets and Benchmarks Track.

The current paper PDF uses the name GUARDSET-X.
