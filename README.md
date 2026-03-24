Table of Contents
1) Background: What is P300?
    The P300 is an event-related potential (ERP) — a brain signal that appears roughly 300      milliseconds after a participant detects a rare, task-relevant stimulus.</br>
   
  Time →</br>
    ────────────────────────────────────────</br>
    Stimulus:  S  S  S  T  S  S  T  S  S  S</br>
    ────────────────────────────────────────</br>
   
2) Environment Setup</br>
   Required</br>
     torch>=2.0.0</br>
     numpy>=1.24.0</br>
     scipy>=1.10.0</br>
     matplotlib>=3.7.0</br>
     seaborn>=0.12.0</br>
     scikit-learn>=1.2.0</br>
     gradio>=4.0.0</br>
   
3) Synthetic EEG Data Generation</br>
    1/f pink noise + alpha oscillations (8–12 Hz) → background EEG</br>
    P300 component (targets only) → positive Gaussian ~300 ms, maximal at Pz</br>
    N200 component (targets only) → negative deflection ~200 ms, frontal</br>
   
4) Preprocessing Pipeline</br>
     Bandpass filter	0.5 – 40 Hz</br>
     Notch filter	50 Hz (Thailand / EU)</br>
     Baseline correction	−100 to 0 ms</br>
     Artifact rejection	±100 µV threshold</br>
     Z-score normalization	per channel</br>
   
5) EEGNet Architecture</br>
     EEGNet — Lawhern et al. (2018), J. Neural Eng.</br>
     https://arxiv.org/abs/1611.08024</br>
   
6) Training</br>
     Choice	| Reason</br>
        AdamW optimizer	| Weight decay prevents overfitting</br>
        Cosine annealing | LR	Smooth decay, no manual schedule</br>
        Weighted sampler	| Fixes 4:1 class imbalance</br>
        Noise augmentation	| Improves generalisation on small EEG datasets</br>
        AUC-ROC metric	| Threshold-independent, standard for P300 BCI</br>
   
7) Visualization the Result</br>
      Gradient saliency — partial derivative of the model's P300-class confidence w.r.t.          each input sample. Bright = model was paying attention here.</br>

      Expected result consistent with real neuroscience:</br>
        Brightest channels → P3, Pz, P4 (parietal cluster)</br>
        Brightest time window → 300 – 450 ms</br>
  
8) Gradio Demo App</br>
      Sandbox for you to try to play with parameter</br>

9) Summary</br>
      Channels	21 (full 10-20 system)</br>
      Data	1,000 simulated epochs (800 std / 200 target)</br>
      Preprocessing	Bandpass 0.5–40 Hz · Notch 50 Hz · Baseline · Artifact reject · z-score</br>
      Model	EEGNet · ~2,700 parameters</br>
      Expected AUC	0.88 – 0.95</br>
