# PolyGuard Finance Pilot

This project examines how the proportion of rewritten unsafe training texts affects a financial safety classifier's performance.

The pilot compares 0%, 50%, and 100% replacement of unsafe training texts with their stored rewritten versions. Base cases, training sample size, class balance, model settings, and decision threshold remain fixed.

## Research Question

At a fixed training sample size, how does the proportion of rewritten unsafe training texts affect:

- Correct identification of unsafe information?
- Incorrect classification of safe information as unsafe?
- Generalization to original and rewritten text versions?

## Notebook

[Open the notebook in Colab](https://colab.research.google.com/github/STATS201-Autumn2026/Qingyue-PS1-polyguard-finance/blob/main/PolyGuard_Finance_Original_and_Rewritten_Training_Text_Qingyue.ipynb)

Notebook filename:

`PolyGuard_Finance_Original_and_Rewritten_Training_Text_Qingyue.ipynb`

## Reproduce the Pilot

1. Open the notebook in Google Colab.
2. Select a standard CPU runtime.
3. Run Section 1 to install the specified packages.
4. Restart the session after installation.
5. Run Section 2 onward in order, including all plotting cells.
6. Review the data checks, validation results, test results, and four figures.
7. Run Section 9 to save the results and configuration.
8. Run Section 10 to create and download the output ZIP.

If automatic downloading does not start, open Colab's left Files panel, expand `assignment/4`, and download the ZIP whose filename matches the printed output.

Use File → Download → Download .ipynb to save the executed notebook.

No GPU, API key, or Hugging Face token is required. The notebook reads public data and writes local runtime files. It does not modify remote repositories.

## Environment

The setup cell specifies:

- NumPy 2.2.6
- pandas 2.2.3
- scikit-learn 1.6.1
- Matplotlib 3.10.3
- requests 2.32.4

The uploaded `results/experiment_config.json` currently records NumPy 2.4.6 and requests 2.32.3, which differ from the setup specifications. The archived outputs therefore should not be described as having been generated under an exactly matching environment.

For a new run, restart the session after installation and check the recorded versions before comparing outputs.

## Data and Versions

### Processed Course Dataset

[DKU-STATS201-Autumn2026/Qingyue-PS1-polyguard-finance](https://huggingface.co/datasets/DKU-STATS201-Autumn2026/Qingyue-PS1-polyguard-finance/tree/30b7ebca5c79287d7ca03359995e588d377ab018)

Processed dataset revision:

`30b7ebca5c79287d7ca03359995e588d377ab018`

### Original Dataset

[Virtue-AI-HUB/PolyGuard](https://huggingface.co/datasets/Virtue-AI-HUB/PolyGuard)

Original dataset revision:

`fa5b3948e6e23f0611e2382561c1434971550b65`

Selected directory:

`finance/Input`

The notebook reconstructs the processed data from the pinned original files rather than loading the processed course release directly.

Each processed row represents one base case. Original and rewritten texts are stored together when a rewritten version is available.

Main variables:

- `case_id`: base-case identifier.
- `rule_id`: normalized safety-rule identifier.
- `source`: policy-source group.
- `label`: 0 for safe and 1 for unsafe.
- `original_text`: original input text.
- `rewritten_text`: stored rewritten input text; unavailable for safe cases.
- `split`: training, validation, or test assignment.

### Code Snapshot

The notebook and uploaded outputs reviewed before this README update are available at:

[97d765a935ee6dbd1299c40ff79e51fee609f33b](https://github.com/STATS201-Autumn2026/Qingyue-PS1-polyguard-finance/tree/97d765a935ee6dbd1299c40ff79e51fee609f33b)

This identifies the code and output snapshot before the README replacement. Later code or output changes require a new revision reference.

## Data Preparation and Checks

The notebook:

- Loads financial input examples from the pinned source revision.
- Assigns labels from the source safe/unsafe files.
- Keeps original and rewritten versions together.
- Normalizes safety-rule text and creates rule identifiers.
- Splits data by normalized rule.
- Checks unique cases, required text, label balance, and rule overlap.

Expected case counts:

| Split | Safe | Unsafe | Total |
|---|---:|---:|---:|
| Train | 2,470 | 2,470 | 4,940 |
| Validation | 525 | 525 | 1,050 |
| Test | 535 | 535 | 1,070 |

A successful run should print:

```text
Checks passed: unique cases, complete text, balanced labels, and zero rule overlap.
```

## Method

- Model: TF-IDF with Logistic Regression.
- TF-IDF features: word unigrams and bigrams.
- Minimum document frequency: 2.
- Maximum feature count: 15,000.
- Logistic Regression solver: liblinear.
- Random seed: 42.
- Decision threshold: 0.5.
- Held-out unit: normalized safety rule.

The three training conditions replace 0%, 50%, or 100% of unsafe original texts with their stored rewrites.

Every condition contains 4,940 training cases, including 2,470 safe and 2,470 unsafe cases. The numbers of rewritten unsafe training texts are 0, 1,235, and 2,470.

Safe texts remain unchanged. Each unsafe case contributes one selected text version per condition.

Only selected text is used as a predictive feature. Identifiers, source names, rule identifiers, and rewrite availability are not model inputs.

## Evaluation

Primary metrics:

- Unsafe recall: `TP / (TP + FN)`. Higher values indicate fewer missed unsafe cases.
- Safe false-positive rate: `FP / (FP + TN)`. Lower values indicate fewer safe cases incorrectly flagged as unsafe.

AUC is a secondary diagnostic. An always-safe baseline is included.

The original test view uses original texts. The rewritten test view substitutes rewritten unsafe texts while retaining the same safe texts.

Consequently, safe false-positive rates must match between the two test views for each fitted model.

The threshold is fixed before evaluation. The pilot evaluates all three predefined training conditions.

## Figures and Outputs

The four proposal figures are:

| File | Content |
|---|---|
| `figure_1_training_composition.png` | Training composition under the three replacement ratios |
| `figure_2_source_coverage.png` | Source coverage across training, validation, and test splits |
| `figure_3_training_lengths.png` | Training text lengths by label and text version |
| `pilot_results.png` | Unsafe recall and safe false-positive rates across replacement ratios |

Figures have descriptive titles. Figure numbering and captions can be added separately in the proposal.

`results/figure_4_pilot_results.png` is a legacy export. Use `results/pilot_results.png` for the current fourth figure.

Additional files in `results` include:

- `training_conditions.csv`
- `training_assignments.csv`
- `validation_results.csv`
- `test_results.csv`
- `validation_predictions.csv`
- `test_predictions.csv`
- `experiment_config.json`

The output ZIP contains processed data and provenance under `data/`, and figures, metrics, predictions, and configuration under `results/`.

## Limitations

This is an exploratory, single-seed pilot using synthetic examples.

Only unsafe cases have stored rewrites. The study therefore does not evaluate rewriting both classes.

Rewrites have not been independently verified as perfectly meaning-preserving, and no expert audit of all labels was conducted.

Original and rewritten texts may differ in length and style. Performance differences may reflect these properties as well as changes in wording.

Rule-level separation reduces direct rule overlap but does not guarantee that related topics or linguistic patterns are absent across splits.

The results do not establish a universally optimal replacement ratio or real-world financial deployment reliability.

## Data Attribution and License

The underlying data were created by the original PolyGuard authors. The processed release follows the source dataset's CC BY 4.0 license.

Associated paper:

Kang et al. (2025). [PolyGuard: Massive Multi-Domain Safety Policy-Grounded Guardrail Dataset](https://proceedings.nips.cc/paper_files/paper/2025/hash/11ed9cdc955e23684a1beae9cb8da059-Abstract-Datasets_and_Benchmarks_Track.html). NeurIPS 2025 Datasets and Benchmarks Track.

The paper PDF uses the name GUARDSET-X.
