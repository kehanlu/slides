---
title: 'IR Final: Speech and BERT (21.01.15)'

---

# Speech and BERT <!-- .element: class="title" -->

<div class="title-name">
2021.01.15 <br>
盧克函 游照臨 吳苡瑄
</div>

---

# An Audio-enriched BERT-based Framework for Spoken Multiple-choice Question Answering <!-- .element: class="title" -->
## Chia-Chih Kuo, Shang-Bao Luo, Kuan-Yu Chen <!-- .element: class="subtitle" -->



---

## Outline

- Task(SMCQA)
- Methodology
  - TSAtt
  - aeBERT
- Experimental Setup
- Experimental Results
- Conclusion

---

## Spoken Multiple-choice Question Answering

- Spoken MCQA

  - 1 Passage 🔊
  - 1 Question 🔊
  - Multiple choices 🔊

- Listening comprehension task


----

## Naïve Approaches

<img src="attachments/2021-01-15-13-23-26.png" style="max-width:100%;"/>

----

## Naïve Approaches.

<img src="attachments/2021-01-15-13-23-05.png" style="max-width:100%;"/>


---

# Methodology <!-- .element: class="slide-center" -->

----

## Vanilla BERT

- Passage $p=\\{w_1^p, w_2^p,\cdots,w_{|p|}^p\\}$
- Question $q = \\{w_1^q, w_2^q,\cdots,w_{|q|}^q \\}$
- $n^{th}$ choice $c_n = \\{w_1^{c_n}, w_2^{c_n},\cdots,w_{|{c_n}|}^{c_n} \\}$
- Concatenation of token sequence
  - QCP

$$
\\{[CLS], q, c_n, [SEP], p, [SEP]\\}
$$

![](attachments/2021-01-15-13-18-42.png) <!-- .element: class="img50" -->

----

## Audio-enriched BERT-based Framework
- aeBERT(Proposed)

<img src="attachments/2021-01-14-05-02-18.png" style="max-width:50%"/>

----

## Temporal-spectral Attention Layer

- For each auto-transcribed token $w_i$ and its corresponding acoustic feature vectors(MFCCs)

$$
\mathcal{F}^{w_i} = \\{f_1^{w_i}, \cdots, f_{|w_i|}\\} \in \mathbb{R}^{d_a \times |w_i|}
$$

![](attachments/2021-01-14-04-52-44.png)

----

## MFCCs

- Front-end Signal Processing


<img src="attachments/2021-01-15-00-02-00.png" style='max-width:100%'/>

國立臺灣大學 李琳山教授 【數位語音處理概論】<!-- .element: class="footnote" -->


----

## MFCCs.

<img src="attachments/2021-01-15-04-10-55.png" style="max-width:80%"/>

![](attachments/2021-01-15-04-09-00.png)

國立臺灣大學 李琳山教授 【數位語音處理概論】<!-- .element: class="footnote" -->

----

## MFCCs..

- Pre-emphasis
- Framing & window
- Short time FT
- Mel Filter-bank ➝ **F-bank**
- Inverse FT
- Derivatives
- ➝ **MFCC** (43)

----

## Temporal-spectral Attention Layer.

- Attention map

$$
A^{w_i} = \text{softmax}(\mathcal{W}_a\mathcal{F}^{w_i})
$$

$$
\mathcal{W}_a \in \mathbb{R}^{d_a \times d_a},
A^{w_i} \in \mathbb{R}^{d_a \times |w_i|}
$$

![](attachments/2021-01-14-05-07-20.png)

----

## Temporal-spectral Attention Layer..

- Acoustic level representation

$$
v^{w_i} = \sum^{|w_i|}_{j=1}\[ A^{w_i} \odot  \mathcal{F}^{w_i}\]
$$

![](attachments/2021-01-14-05-07-20.png)

----

## Temporal-spectral Attention Layer...

- Acoustic level representation with size $d_t$ 🤗

$$
\hat{v}^{w_i} = \mathcal{W}_sv^{w_i} + b_s
$$

$$
\hat{v}^{w_i} \in \mathbb{R}^{d_t}
$$

![](attachments/2021-01-14-05-09-59.png)

----

## aeBERT.

<img src="attachments/2021-01-14-05-02-18.png" style="max-width:50%"/>

----

## aeBERT..

$$
r^{c_n} = \mathcal{W}_rh^{[CLS]^{c_n}} + b_r
$$

$$
P(c_n) = \frac{\exp (r^{c_n})}{\sum^N_{n^\prime=1}\exp (r^{c_{n^\prime}})}
$$

<img src="attachments/2021-01-15-02-02-52.png" style="max-width:60%"/>

---

# Experimental Setup <!-- .element: class="slide-center" -->

----
## Experimental Setup

**Dataset**
  - 2018 Fromosa Grand Challenge
    - Dev set, test set, advanced test set
  - A passage, a question and 4 candidate choices
  - Including science, news, medicine, literature, history and so on.

**ASR system**
  - Kaldi
    - trained based on TDNN-F with lattice-free MMI
  - **Error rate 7.79%**

----

## Experimental Setup.

**BERT**

- Huggingface
- 1 epoch MSE warm up
  - Minimize MSE between acoustic-level representation ($v^{w_i}$) and token embeddings.
- Finetune aeBERT

---

# Experimental Results <!-- .element: class="slide-center" -->

----

## Vanilla BERT

w.r.t **manual transcriptions** and **auto-transcribed text**

- Upper bound

<img src="attachments/2021-01-15-02-30-24.png" style="max-width:80%;margin-top:30px"/>


----


## Performance

<img src="attachments/2021-01-15-02-34-31.png" style="max-width:70%;margin-top:30px"/>

----

## Our Experiment

<img src="attachments/2021-01-15-02-36-57.png" style="max-width:100%;margin-top:80px"/>

---

## Conclusion

**Audio-enriched BERT-based framework**
- Proposed framework showed remarkable superiority than other strong baselines, indicating the potential of the framework.
- TSAtt: temporal and spectra attention

**Future work**
- Evaluate the framework on other dataset.
- Extend the proposed aeBERT to other NLP-related tasks


----

## Our contribution from this presentation
*the reason you shoud give us full points! ;-)*

- We introduced the **MFCCs** and **F-bank** representations, which are everywhere in speech research field.
- We shared a novel method to use pretrained BERT for different task.

![](attachments/2021-01-15-04-18-36.png)
