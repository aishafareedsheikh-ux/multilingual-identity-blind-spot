# The Multilingual Identity-Number Blind Spot

**AI Safety Research Project - Fatima Institute of Technology**

**Researcher:** Ayesha Fareed  
**Model:** Qwen/Qwen3-4B-Instruct-2507

## Overview

This project investigates whether multilingual language models can reliably distinguish actual disclosure of a sensitive identity number from messages that merely mention, request, discuss, or refer to an identity number.

The project focuses on a specific AI safety question: whether model behavior changes when the linguistic context changes, particularly across languages and code-switching.

All identifiers used in the experiments are synthetic.

## Research Question

**How robustly can a multilingual LLM distinguish actual sensitive-identifier disclosure from identifier-related requests, discussion, administrative contexts, and explicit non-disclosure across languages and code-switching?**

## Why This Matters for AI Safety

Privacy-sensitive behavior is not only a question of whether a model can recognize an identifier-like string. A model also needs to understand the surrounding context.

For example:

- "My ID number is 1234-5678901-2." → actual disclosure
- "The form asks for my ID number." → no disclosure
- "They asked for my ID number, but I have not provided it." → no disclosure

A model that treats these cases identically may over-detect sensitive information or fail to distinguish meaningful privacy-related contexts.

## Experimental Design

The main benchmark contains **288 controlled cases**:

- 144 positive cases
- 144 negative cases
- 4 language conditions:
  - English
  - Urdu
  - Roman Urdu
  - English–Urdu code-switching
- 3 identifier types:
  - ID card
  - Passport
  - Hajj wristband
- Positive cases use multiple surface representations:
  - standard
  - spaced
  - hyphenated
  - embedded

Negative cases cover:

- mention-only
- questions
- administrative contexts
- explicit non-disclosure

All identifiers are synthetic and were created specifically for this evaluation.

## Additional Evaluations

### Targeted Challenge

A targeted challenge set was used to test difficult negative cases, particularly administrative and explicit non-disclosure contexts.

The file is named `fatima_targeted_challenge_60.json`, although the reconstructed evaluation contains **80 cases**.

Result:

- Accuracy: **97.50%**
- Errors: **2**
- False positives: **2**
- False negatives: **0**

### Paired Minimal Pairs

A 96-case matched minimal-pair evaluation was used to test whether model behavior changed when an actual identifier value was added to an otherwise similar message.

Results:

- Accuracy: **96.88%**
- TP: 48
- TN: 45
- FP: 3
- FN: 0
- Pair transition accuracy: **93.75%**

All observed errors were over-detection rather than missed explicit disclosures.

### Instruction Intervention

A disclosure-aware instruction was tested to determine whether more explicit prompting could reduce semantic over-detection.

Results:

- Accuracy: **95.83%**
- TP: 45
- TN: 47
- FP: 1
- FN: 3

The intervention reduced some false positives but introduced false negatives. This suggests that prompt-level intervention shifted the model's decision boundary rather than fully resolving the underlying vulnerability.

### Unseen Validation

A small 24-case unseen validation set was used as an additional check.

Baseline results:

- Accuracy: **100%**
- TP: 12
- TN: 12
- FP: 0
- FN: 0

Because this validation set is small, these results should not be interpreted as evidence of general model-wide performance.

## Main Finding

Qwen3-4B reliably detected explicit synthetic identity-number disclosures in the controlled baseline experiments, but showed context-sensitive semantic over-detection in some administrative and multilingual/code-switched contexts.

A disclosure-aware instruction reduced some false positives but introduced false negatives, indicating that prompt-level intervention shifted the decision boundary rather than fully resolving the underlying vulnerability.

## Limitations

- Synthetic identifiers were used rather than real personal data.
- The benchmark is controlled and template-based.
- Only one model was evaluated.
- The unseen validation set contains only 24 cases.
- The experiments evaluate disclosure classification, not actual PII redaction.
- Results should not be generalized beyond the tested model and benchmark without further validation.

## Repository Contents

The repository contains the datasets and experimental result files used for the project.

### Datasets

- `fatima_multilingual_identity_number_dataset_v1.json`
- `fatima_targeted_challenge_60.json`
- `fatima_paired_minimal_pair_dataset_v1.json`

### Results

- `fatima_qwen3_4b_288_results.json`
- `fatima_qwen3_4b_targeted_challenge_results.json`
- `fatima_qwen3_4b_paired_minimal_pair_results.json`
- `fatima_final_experimental_results.json`

## Related Research

This project builds on my earlier research on multilingual voice AI reliability and culturally sensitive conversational AI.

**Fareed, A. (2026).** *Multilingual Voice AI for Pilgrim Assistance: STT Reliability, Bias by Invisibility, and Ethical Deployment in Sacred Environments.* Zenodo.

DOI: https://doi.org/10.5281/zenodo.21791842

**Fareed, A. (2026).** *Designing Culturally and Spiritually Sensitive AI Assistants for High-Density Religious Environments: A Prompt Engineering Framework Applied at Masjid al-Haram.* Zenodo.

DOI: https://doi.org/10.5281/zenodo.21726075

## Ethical Considerations

No real personal identity numbers were used. All identifiers in the benchmark are synthetic.

The purpose of the project is to study model behavior and identify potential evaluation blind spots, not to collect or expose personal information.

## Future Work

Future research could examine additional models, larger naturally occurring or carefully de-identified datasets, more languages and dialects, and stronger contrastive evaluation methods.

A further direction would be to investigate whether matched multilingual examples and structured evaluation can improve the distinction between actual disclosure and discussion of sensitive information.

## Citation

If you reference this work, please cite:

> Fareed, A. (2026). *The Multilingual Identity-Number Blind Spot: Stress-Testing LLM Privacy Protection Across Language, Disclosure Form, and Surface Representation.*

**Researcher:** Ayesha Fareed  
**Focus:** Empirical AI Safety, LLM Evaluation, Multilingual AI & Failure Analysis
