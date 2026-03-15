# Multitask Punctuation and Capitalization Restoration

This project explores a simple but practical NLP task: restoring punctuation and capitalization in raw English text.

The model takes text where punctuation and case information are missing and predicts them jointly. The approach is based on a multi-task Transformer architecture trained on English Wikipedia.

Example:

Input:

john went to paris yesterday he met alice there

Output:

John went to Paris yesterday. He met Alice there.

This type of model is commonly used in speech recognition pipelines, subtitle generation, and other systems where text arrives without proper formatting.

---

# Overview

The system predicts two things simultaneously:

1. punctuation marks after each token  
2. capitalization of each token

Both tasks are learned together using a shared encoder and separate classification heads.

Architecture:

BERT encoder ├── punctuation classifier └── capitalization classifier


Training both tasks jointly tends to improve stability and allows the model to use shared linguistic signals.

---

# Dataset

The training data comes from English Wikipedia.

Articles are segmented into sentences and converted into a simplified format where punctuation is removed and capitalization is normalized. The model then learns to reconstruct the original structure.

Dataset source:

wikimedia/wikipedia  
20231101.en

Only a small portion of the dataset is used for experiments in this repository.

---

# Model

The model consists of:

- BERT base (cased) encoder
- two token classification heads
- LoRA adapters for parameter-efficient fine-tuning

Using LoRA allows training only a small number of parameters while keeping the base model frozen.

Loss function:

Cross-entropy with class weighting to compensate for the heavy imbalance between punctuation classes.

---

# Training Pipeline

The project is organized as a sequence of notebooks, each covering one stage of the pipeline.

1. 01_dataset_loading.ipynb  
2. 02_sentence_processing.ipynb  
3. 03_label_generation.ipynb  
4. 04_tokenization_and_alignment.ipynb  
5. 05_dataset_and_dataloaders.ipynb  
6. 06_model_architecture.ipynb  
7. 07_training.ipynb  
8. 08_evaluation.ipynb  

The goal was to keep each stage easy to inspect and modify.

---

# Evaluation

The model is evaluated on two tasks separately.

### Punctuation

Classes:

- O (no punctuation)
- comma
- period
- question mark

### Capitalization

Classes:

- lowercase
- capitalized
- uppercase

Example results on the test split:

Punctuation (macro F1):

~0.81

Capitalization (macro F1):

~0.93

The main difficulty comes from commas, which are significantly more context-dependent than sentence boundaries.

---

# Installation

Install dependencies with:


pip install -r requirements.txt


---

# Running the Project

Execute the notebooks in order:

1. 01_dataset_loading.ipynb  
2. 02_sentence_processing.ipynb  
3. 03_label_generation.ipynb  
4. 04_tokenization_and_alignment.ipynb  
5. 05_dataset_and_dataloaders.ipynb  
6. 06_model_architecture.ipynb  
7. 07_training.ipynb  
8. 08_evaluation.ipynb  

---

# Inference Example

Once trained, the model can be used to restore punctuation in plain text.

Example:


input:
john went to paris yesterday he met alice

output:
John went to Paris yesterday. He met Alice.


An example inference script is included in the `inference` directory.

---

# Tech Stack

The project uses:

- PyTorch
- HuggingFace Transformers
- HuggingFace Datasets
- PEFT (LoRA)
- NLTK
- Scikit-learn

---

## Results

The model is evaluated separately on punctuation prediction and capitalization prediction.

### Punctuation Restoration

| Class | Precision | Recall | F1 |
|------|-----------|--------|----|
| O (no punctuation) | 0.98 | 0.98 | 0.98 |
| Comma | 0.78 | 0.73 | 0.75 |
| Period | 0.96 | 0.98 | 0.97 |
| Question mark | 0.72 | 0.41 | 0.52 |

Macro F1: **0.81**

---

### Capitalization Restoration

| Class | Precision | Recall | F1 |
|------|-----------|--------|----|
| Lowercase | 0.98 | 0.99 | 0.99 |
| Capitalized | 0.95 | 0.93 | 0.94 |
| Uppercase | 0.85 | 0.87 | 0.86 |

Macro F1: **0.93**

---

### Observations

The model performs well at identifying sentence boundaries and capitalization patterns.  
The main source of error comes from comma prediction, which typically requires longer contextual information than sentence-ending punctuation.


## Project Summary

This repository implements a Transformer-based system for restoring punctuation and capitalization in English text.  
The model is trained on Wikipedia and uses a multi-task learning setup where both tasks are learned jointly.  

The implementation focuses on clarity and reproducibility: the entire pipeline — from dataset preparation to evaluation — is organized as a sequence of notebooks that document each step of the process.

# Notes

This repository is intended mainly as an educational project exploring:

- multi-task learning with Transformers
- token classification
- parameter-efficient fine-tuning

The implementation is deliberately kept simple and readable so that individual components can be easily modifi
