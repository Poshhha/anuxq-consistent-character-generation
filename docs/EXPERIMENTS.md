# Experiment Log

Multiple identity-preservation strategies were evaluated.

## LoRA Only
Used as the baseline. Generated realistic images but identity could drift across new scenes.

## IPAdapter FaceID
Improved reference identity in some cases but stronger conditioning produced exaggerated facial characteristics and an artificial appearance.

## PuLID
Produced the best overall balance between identity consistency and photorealism.

## ReActor
InsightFace face swapping improved direct facial similarity but sometimes introduced softness and lower facial texture quality.

## FaceDetailer
Used after ReActor to recover facial detail. Strong refinement could also alter identity characteristics.

## Final Selection
The V2 Epoch 8 LoRA combined with PuLID was selected as the main pipeline because it gave the strongest overall balance between identity retention, realism and workflow stability.
