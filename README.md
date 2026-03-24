Table of Contents
1) Background: What is P300?
    The P300 is an event-related potential (ERP) — a brain signal that appears roughly 300      milliseconds after a participant detects a rare, task-relevant stimulus.
   
  Time →
    ────────────────────────────────────────
    Stimulus:  S  S  S  T  S  S  T  S  S  S
               ↑           ↑
            Standard    Target
            (no P300)  (P300!)
    ────────────────────────────────────────
   
2) Environment Setup
   Required
     torch>=2.0.0
     numpy>=1.24.0
     scipy>=1.10.0
     matplotlib>=3.7.0
     seaborn>=0.12.0
     scikit-learn>=1.2.0
     gradio>=4.0.0
   
3) Synthetic EEG Data Generation
    1/f pink noise + alpha oscillations (8–12 Hz) → background EEG
    P300 component (targets only) → positive Gaussian ~300 ms, maximal at Pz
    N200 component (targets only) → negative deflection ~200 ms, frontal
   
4) Preprocessing Pipeline
     Bandpass filter	0.5 – 40 Hz
     Notch filter	50 Hz (Thailand / EU)
     Baseline correction	−100 to 0 ms
     Artifact rejection	±100 µV threshold
     Z-score normalization	per channel
   
5) EEGNet Architecture
     EEGNet — Lawhern et al. (2018), J. Neural Eng.
     https://arxiv.org/abs/1611.08024
   
6) Training
     Choice	| Reason
        AdamW optimizer	| Weight decay prevents overfitting
        Cosine annealing | LR	Smooth decay, no manual schedule
        Weighted sampler	| Fixes 4:1 class imbalance
        Noise augmentation	| Improves generalisation on small EEG datasets
        AUC-ROC metric	| Threshold-independent, standard for P300 BCI
   
7) Visualization the Result
      Gradient saliency — partial derivative of the model's P300-class confidence w.r.t.          each input sample. Bright = model was paying attention here.

      Expected result consistent with real neuroscience:
        Brightest channels → P3, Pz, P4 (parietal cluster)
        Brightest time window → 300 – 450 ms
  
8) Gradio Demo App
      Sandbox to adjust parameter

9) Summary
      Channels	21 (full 10-20 system)
      Data	1,000 simulated epochs (800 std / 200 target)
      Preprocessing	Bandpass 0.5–40 Hz · Notch 50 Hz · Baseline · Artifact reject · z-score
      Model	EEGNet · ~2,700 parameters
      Expected AUC	0.88 – 0.95
