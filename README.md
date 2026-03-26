# Auditory P300 Detection with EEGNet

Detecting P300 brain responses from 21-channel EEG using a compact CNN — built as an AI Builders tutorial.


## What it does

- Simulates auditory oddball EEG (21ch, 256 Hz, 1-second epochs)
- Preprocesses with bandpass filter, baseline correction, and z-score normalization
- Trains **EEGNet** (~2,700 parameters) to classify single trials as P300 or non-P300
- Evaluates with AUC-ROC, confusion matrix, and gradient saliency maps
- Includes an interactive Gradio demo for retraining with different hyperparameters


## Reference

Lawhern et al. (2018). EEGNet: A Compact Convolutional Neural Network for EEG-based Brain-Computer Interfaces. *Journal of Neural Engineering*, 15(5).
