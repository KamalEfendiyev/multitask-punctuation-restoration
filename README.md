# multitask-punctuation-restoration
Multi-task Transformer model for punctuation and capitalization restoration trained on Wikipedia using BERT and LoRA.
This project implements a multi-task Transformer model for punctuation and capitalization restoration in English text.

The model is trained on Wikipedia data and jointly predicts:
- punctuation marks (comma, period, question mark)
- capitalization (lowercase, capitalized, uppercase)

Architecture:
- BERT encoder
- two classification heads
- LoRA adapters for parameter-efficient fine-tuning

Dataset:
- English Wikipedia (wikimedia/wikipedia)

Tasks:
- punctuation restoration
- capitalization restoration
