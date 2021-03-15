---

title: 'Oscar: Object-Semantics Aligned Pre-training
for Vision-Language Tasks'
event: 'Sinica meeting'
date: "2021-03-16"
theme: theme/mytheme.css

---

# Oscar: Object-Semantics Aligned Pre-training for Vision-Language Tasks <!-- .element: class="title" -->
## Li et al., 2020 <!-- .element: class="subtitle" -->

<div class="title-name">
2021.03.16 <br>
Ke-Han Lu
</div>

<span>Oscar http://arxiv.org/abs/2004.06165</span> <!-- .element: class="footnote" -->

---

## Outline

- Oscar: **O**bject-**S**emanti**c**s **A**ligned Pre-t**r**aining
![](attachments/2021-03-07-22-29-33.png) <!-- .element: class="img25" -->
- VinVL: Making Visual Representations Matter in Vision-Language Models (Zhang et al.,2021)
  - Oscar+



<span>VinVL http://arxiv.org/abs/2101.00529</span> <!-- .element: class="footnote" -->

---

## Vision-Language Pre-train(VLP)

- Multi-layer Transformer
- Learn generic representations from massive image-text pairs
  - Object detector
  - Concatenate image region feature and text features as input
- Fine-tuning VLP models on task-specific data

----

## VisualBERT (Li et al., 2019)

- Image-text pairs (e.g. image captions)
- region features from object detector (e.g. Faster-RCNN)
- Objective (e.g. MLM, NSP)

![](attachments/2021-03-07-23-06-40.png) <!-- .element: class="img100" -->

----

## Problems of existing VLP model

- Visual regions are often over-sampled, noisy and ambiguous.
- Lack of explicit **alignment** information between image regions and text 

![](attachments/2021-03-07-23-35-08.png)


---

## Oscar
- Training samples
  - word sequence
  - **object tags** (as anchor points)
  - image region feature
- Motivation
  - Salient objects in an image can be accurately detected.
  - These objects are often mentioned in the paired text.

![](attachments/2021-03-08-00-10-01.png) <!-- .element: class="img100" -->

----

## Oscar

Input: $(w, q, v)$

$v$ and $q$ are generated as follows:
- Given a image with $K$ regions of objects
- Faster R-CNN is used to extract visual semantics of each region. $(v^\prime, z)$
 
$$v^\prime \in \mathbb{R}^{P}, z \in \mathbb{R}^R, P=2048, R=4 \text{or} 6$$

$$v = \text{FFN}(\[v^\prime,z\])$$

$q$ is the sequence of word embeddings of the object tags from Faster R-CNN

----

## Oscar


![](attachments/2021-03-08-00-02-38.png) <!-- .element: class="img100" -->

Multi-layer Transformers is pre-trained BERT models<!-- .element: class="footnote" -->

----

**Dictionary View:Masked Token Loss**
- randomly mask 15% of input token with `[MASK]`, predict from surroundings and image information.

$$
h \overset{\Delta}{=} [w, q]
$$

$$
\mathcal{L}_{\text{MTL}} = -\mathbb{E} _{(v,h)\sim \mathcal{D}} \log p (h_i | h _{\backslash i}, v)
$$

<hr>

**Modality View: Contrastive Loss**
- randomly replace 50% $q$ with different tag sequence sampled from the dataset.
- Use `[CLS]` to predict whether the pair contains original image representations($y=1$)
$$
h^\prime \overset{\Delta}{=} [q, v]
$$

$$
\mathcal{L}_{\text{C}} = -\mathbb{E} _{(h^\prime, w)\sim \mathcal{D}} \log p (y | f(h^\prime, w))
$$

----

## Pre-training Corpus

based on existing V+L datasets

- including COCO, Conceptual Captions, SBU captions, flicker30k, GQA etc.
- 4.1M unique images
- 6.5M text-tag-image triples

---

## Result
**Down stream tasks**
![](attachments/2021-03-08-01-20-11.png) <!-- .element: class="img100" -->

<hr>

**VQA**

![](attachments/2021-03-08-01-37-03.png) <!-- .element: class="img100" -->


<span>* S, B, L indicates performance achieved by small models, VLP of similar size to BERT base and large model<br>
*Blue indicates the best result for a task, and gray background indicates results produced by Oscar<br>
*VQA results are from LXMERT</span> <!-- .element: class="footnote" -->

----

## Qualitative Studies(t-SNE)

**Intra-class**: the distance of the same object between two modalities is reduced. (e.g. person)

**Inter-class**: object classes of related semantics are getting closer but still distinguishable (e.g. animals, furniture, transportation)

![](attachments/2021-03-08-01-51-44.png) <!-- .element: class="img100" -->

----

## Ablation Analysis

**Effect of Object Tags**

![](attachments/2021-03-08-02-04-46.png) <!-- .element: class="img100" -->

With **more accurate object detectors** developed in the future, Oscar can achieve even better performance, closing the gap demonstrated by using the ground-truth tags.