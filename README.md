\# Anuxq — Identity-Consistent Photorealistic AI Character Generation



An experimental generative-AI project focused on creating a photorealistic synthetic character whose visual identity remains consistent across different prompts, environments, camera angles and framing.



Rather than relying on a single image-generation workflow, this project explores the complete pipeline from dataset preparation and LoRA fine-tuning to identity conditioning, GPU optimisation and comparative workflow evaluation.



\## Project Objective



The main challenge was not simply generating realistic portraits, but maintaining the same synthetic identity when the character was placed in different scenes, poses and compositions.



The project investigates how custom LoRA training and reference-based identity-conditioning methods can be combined to improve character consistency while retaining natural image quality.



\## Final Pipeline



The final preferred workflow is:



Juggernaut XL / SDXL  

→ Custom V2 LoRA  

→ PuLID Identity Conditioning  

→ KSampler  

→ VAE Decode  

→ Photorealistic Output



The V2 Epoch 8 LoRA combined with PuLID produced the strongest overall balance between identity preservation, natural facial structure and photorealistic image quality.



\## Dataset Development



The original dataset contained approximately 51 images including:



\- close-up portraits

\- three-quarter views

\- side profiles

\- upper-body shots

\- wider/full-body compositions

\- multiple hairstyles and environments



During experimentation, inconsistencies in captions and excessive training were identified as possible causes of poor generalisation.



A cleaner V2 dataset of 38 selected images was therefore created and manually recaptioned.



Captions used a consistent trigger:



`anuxq woman`



Variable attributes such as shot type, camera angle, hairstyle, clothing and environment were then described separately.



Aspect-ratio bucketing was also enabled to preserve image composition rather than forcing every image into identical crops.



\## LoRA Training Experiments



\### V1



Initial training configuration:



\- 51 images

\- 5 repeats

\- 20 epochs

\- approximately 5,100 optimisation steps

\- Network Rank: 64

\- Alpha: 32

\- Resolution: 1024

\- AdamW8bit

\- FP16

\- Juggernaut XL / SDXL base model



V1 learned several recognisable identity characteristics but showed signs of overtraining and weaker generalisation to unseen scenes.



\### V2



The training strategy was redesigned using:



\- 38 curated images

\- Network Rank: 32

\- Alpha: 16

\- UNet-only LoRA training

\- Aspect-ratio bucketing

\- Latent caching

\- Text-encoder output caching

\- Cosine learning-rate scheduler

\- Min-SNR Gamma: 5

\- AdamW8bit

\- FP16

\- SDPA attention



The Epoch 8 V2 checkpoint became the strongest base for the final identity-conditioned workflow.



\### V3 — Identity Weighting Experiment



A separate experiment tested whether additional exposure to face-focused samples could improve identity retention.



Training distribution:



\- 28 face-focused images × 5 repeats

\- 10 general/pose images × 2 repeats

\- 160 effective samples per epoch

\- 12 epochs

\- 1,920 optimisation steps



An initial Rank 64 experiment created severe memory pressure on the available laptop GPU and slowed training to approximately 150 seconds per step.



Reducing the network to Rank 32 / Alpha 16 lowered training speed to approximately 1.84 seconds per step, allowing the full experiment to complete efficiently.



\## Identity-Preservation Experiments



Several approaches were evaluated.



\### LoRA Only



Used as the baseline generation pipeline.



It produced realistic character images but facial identity could drift when prompts moved significantly away from the training distribution.



\### IPAdapter FaceID



Reference-based FaceID conditioning improved some facial characteristics, but strong conditioning occasionally produced exaggerated eyes, eyebrows and a more artificial appearance.



\### PuLID



PuLID produced the strongest overall balance between identity retention and natural generation.



Different weight, fidelity and denoising ranges were tested to determine how strongly the reference identity should influence generation.



\### ReActor / InsightFace



ReActor with `inswapper\_128` was tested as a post-generation identity correction method.



It could improve direct facial similarity but sometimes reduced facial texture quality and introduced softness in high-resolution portraits.



\### FaceDetailer



FaceDetailer and an Ultralytics face detector were added after ReActor to restore local facial detail.



Although this improved sharpness, excessive refinement could also alter identity characteristics.



\## Saved Workflows



The repository includes several ComfyUI workflows representing different experimental stages.



\- `W1\_V2\_Epoch8\_PuLID\_MAIN` — final preferred workflow

\- `B1\_LoRA\_ONLY\_BASELINE` — LoRA-only baseline

\- `E1\_LORA\_REACTOR` — ReActor experiment

\- `E2\_PULID\_REACTOR\_FACEDETAILER` — advanced hybrid experiment

\- `A1\_IPADAPTER\_FACEID\_OLD` — earlier FaceID experiment



\## Final Validation



The final workflow was evaluated using different compositions:



\- frontal close-up portrait

\- three-quarter environmental / café portrait

\- medium and full-body compositions



Identity preservation was strongest in close-up and medium portraits.



Greater identity drift was observed in distant/full-body compositions where the face occupies significantly fewer pixels.



\## Technologies



Python  

PyTorch  

SDXL  

Juggernaut XL  

LoRA  

Kohya\_ss  

ComfyUI  

PuLID  

InsightFace  

IPAdapter FaceID  

ReActor  

FaceDetailer / ComfyUI Impact Pack  

AdamW8bit  

FP16  

SDPA



\## Hardware



Training and inference were performed locally using an NVIDIA RTX 5050 Laptop GPU with approximately 8 GB VRAM.



The project therefore also involved optimising SDXL training around limited GPU memory.



\## Key Engineering Challenges



\- Preparing and manually captioning a consistent character dataset

\- Selecting suitable LoRA training hyperparameters

\- Identifying overtraining and undertraining behaviour

\- Managing SDXL training within limited VRAM

\- Comparing multiple identity-conditioning approaches

\- Balancing identity preservation against photorealism

\- Debugging ComfyUI, CUDA and model-loading issues

\- Evaluating identity consistency across different compositions



\## Limitations



The system currently performs best for close-up and medium portraits.



Identity consistency decreases in distant/full-body scenes, and aggressive identity-conditioning can introduce unnatural facial structure.



The evaluation conducted so far is primarily qualitative rather than based on a formal facial-embedding benchmark.



\## Future Work



Future development may include:



\- quantitative identity-similarity evaluation

\- improved full-body identity consistency

\- multi-reference identity conditioning

\- further dataset balancing

\- automated workflow benchmarking

\- temporally consistent AI character video generation



\## Note



This project integrates and experimentally evaluates existing technologies including SDXL, PuLID, InsightFace, IPAdapter and ReActor.



The project contribution lies in dataset preparation, LoRA fine-tuning, experimentation, optimisation, workflow engineering and evaluation rather than claiming authorship of the underlying foundation models.

