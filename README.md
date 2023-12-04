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

Found that optimizing only bias and normalization (BiNor) parameters would make better use of limited examples and yield better results than the latest few-shot model Frozen.

CLIP stands for Contrastive Language-Image Pretraining. It consists of a visual encoder(V) and a text encoder (T). The output from both these encoders is multiplied and the result is used as an alignment score between the input image and the text. Because of the pretraining it distinguishes aligned image-text pairs from randomly combined ones by a contrastive loss. 

The task of VQA requires the model to answer questions about the details of the input image. The dataset used for experiment is VQAv2. This dataset is essentially a big set of pictures and related questions. The pictures come from another collection called Microsoft COCO. In this VQAv2 dataset, there are specific types of questions they ask about the pictures. Some examples of these questions are "how many?" (like how many cats are in the picture) and "what color is?" (like what color is the shirt).  When it comes to answering, there are three main categories of responses the model can give:

1. "Yes" or "No"
2. A number (like "3")
3. Any other kind of answer (like "blue" or "apple").

**Natural Language Inference (NLI)**: Imagine two sentences. NLI is like a game where you decide the relationship between these sentences. For example, if Sentence A is "All dogs bark" and Sentence B is "Some dogs make noise", then Sentence A implies Sentence B, so the relationship is "entailment".

Visual Entailment is similar to the above game, but instead of two sentences, you have one image and one sentence. For instance, if the image shows a red apple and the sentence says "The fruit is red", the relationship is "entailment". But if the sentence says "The fruit is blue", then it's a "contradiction".

Researchers took a well-known dataset called SNLI, which is all about sentence relationships as described in NLI. Then, they modified it to create SNLI-VE by replacing one of the sentences with an image from another dataset called Flickr30k.

The researchers are testing how good CLIP is at a game where it needs to figure out the relationship between an image and a sentence. They're using a modified dataset where one of the sentences is replaced with an image. This is to see how well CLIP understands both images and text together. the researchers are testing the zero shot capability of CLIP i.e. checking if CLIP can play the visual entailment game without being trained for it specifically.

Some previous studies have tried using CLIP directly for tasks that involve understanding both images and text without specific training (that's the "zero-shot" part). But this approach didn't work well. When they tried using CLIP directly on a dataset where the task is to answer questions about pictures, the results were almost as bad as just guessing randomly.

The researchers came up with a new method to help CLIP understand and answer questions about images. They used another model called T5 to assist in this. They use a two step process - 

1. Change the question into a special format (they call it a "masked template").
2. Use a language model (like T5) to help remove answers that don't make sense and come up with a list of possible good answers i.e. generate the list of candidate answers.

The infilled template (filling possible answers from step-2 into the template step-1) makes it easier for CLIP to understand both the question and the possible answers together.

### Zero-shot Cross-modality Transfer

Can the CLIP models understand and relate information across two different types of data, namely text (language) and images (vision)?

### Training Phase

**Step-1:** The premise is given to text encoder to produce $v_1$ and the hypothesis text is given to the text encoder to produce $v_2$

For example, if your premise is "The sky is blue" and the hypothesis is "It is daytime", both of these sentences are individually fed into the CLIP's text encoder. The encoder transforms each of these sentences into corresponding vectors, $v_1$ for premise and $v_2$ for hypothesis.

**Step-2:** The researchers then use fusion which is a process that combines two vectors (representations of the text inputs) in a way that captures their individual features and their interactions.

Given two vectors $v_1$(representing the premise) and $v_2$ (representing the hypothesis):

1. **Concatenation ([*v*1,*v*2]):** This step places one vector right after the other. It ensures that the individual features of both vectors are preserved.
2. **Addition ([*v*1+*v*2]):** This captures common features between the two vectors. If both vectors have high values in specific positions, the addition will accentuate that.
3. **Subtraction([*v*1-*v*2]):**  This helps in identifying differences between the vectors. If a feature is strong in $v_1$ but weak or absent in $v_2$ this operation will highlight it.
4. **Element-wise multiplication([*v*1.*v*2]):** This captures interactions between the two vectors. It will give high values in positions where both vectors have high values.

**Step-3:** We give these fused vectors as an input to the MLP that carries a lot of information — not just about the individual texts represented by v1 and v2, but also about their interactions and differences. The MLP uses all this information to discern the relationship between the premise and hypothesis.

The aim of the fusion and the subsequent training of the MLP is to create a system that deeply understands the relationship between two pieces of information (the premise and hypothesis). During training, both of these are textual. But the ultimate goal is to challenge the system during testing by replacing the textual premise with an image. If the system can still accurately predict the relationship, then it shows that the model has effectively learned to bridge the gap between text and vision, achieving cross-modality understanding.

### Reproduce Results

1. Run the jupyter notebooks ClipShot_AutomaticTemplateGeneration.ipynb & Dependency_Final_Parsing.ipynb to reproduce the results for TAP-C method template generation and answer filtering.
