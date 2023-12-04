# ClipShot: Fine tuning CLIP for Few-Shot VQA


### Useful resources:

https://huggingface.co/alan-turing-institute/mt5-large-finetuned-mnli-xtreme-xnli

https://visualqa.org/download.html

### Abbrevations

VLU - Visual Language Understanding

VQA - Visual Question Answering

TAP-C: Template Answer Prompt then CLIP method

NLI: Natural Language Inference

### About

Visual entailment tasks require the understanding of both the visual world and the language world. Previous works related to this involve human annotated datasets that are expensive or require expert knowledge. Hence, the data available for visual entailment is nothing as compared to the text corpora

CLIP consists of a visual encoder and a text encoder and it learns visual representations by aligning images and texts through contrastive loss

There are two major differences between CLIP and previous visual encoders: 

1) it is trained on much larger yet noisy web data, hence promises the generalization ability of CLIP 

2) it has a shallow interaction between vision and language, equips alignment ability across modalities.

To help CLIP answer questions about images, the authors use a two-part method. First, they change the questions into statements that can have parts of them "filled in". Then, they use another model called T5 to fill in those parts and get possible answers.

Instead of training with images, the authors train with textual descriptions of those images. Only a small part of the system (a layer) is changed or "updated" when they train with text descriptions. Even though they trained with text descriptions, they test the system's understanding using both the original images and their descriptions.

Found that optimizing only bias and normalization (BiNor) parameters would make better use of
limited examples and yield better results than the latest few-shot model Frozen.
