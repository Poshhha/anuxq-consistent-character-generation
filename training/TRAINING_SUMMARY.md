# Training Summary

## Base Model
Juggernaut XL v9 based on SDXL.

## V1
- 51 training images
- 5 repeats
- 20 epochs
- Approximately 5100 optimization steps
- LoRA rank 64
- Alpha 32
- AdamW8bit optimizer
- FP16 precision

V1 learned recognizable identity characteristics but showed weaker generalization and signs of overtraining.

## V2
- 38 manually curated and recaptioned images
- LoRA rank 32
- Alpha 16
- UNet-only training
- Aspect-ratio bucketing
- Latent caching
- Text encoder output caching
- Cosine learning-rate scheduler
- Min-SNR gamma 5
- AdamW8bit
- FP16
- SDPA attention

Epoch 8 became the preferred LoRA for the final pipeline.

## V3 Identity Weighting Experiment
- 28 face-focused images with 5 repeats
- 10 general images with 2 repeats
- 160 effective samples per epoch
- 12 epochs
- 1920 optimization steps

Rank 64 caused severe GPU memory pressure at around 150 seconds per step. Reducing the LoRA to rank 32 and alpha 16 improved training speed to approximately 1.84 seconds per step.

## Hardware
NVIDIA RTX 5050 Laptop GPU with approximately 8 GB VRAM.
