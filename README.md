# Cross-Domain Target Classifier Training  
### Implementation of US Patent 20180068231 with Embedding-Based Extension

This repository contains an implementation of:

**“Method and System for Training a Target Domain Classifier to Label Text Segments”**  
US Patent No. 20180068231  
Inventors: Raksha Sharma, Sandipan Dandapat, Himanshu Sharad Bhatt  
Filed by Xerox Research Center of India  

The implementation follows the core algorithmic idea proposed in the patent and extends it by replacing the lexicon-based external signal with embedding-based semantic similarity.

---

## Problem Setting

We consider a cross-domain sentiment classification scenario:

- **Source domain (labeled):** Amazon Polarity Dataset  
- **Target domain (unlabeled for training):** Yelp Polarity Dataset  

The goal is to train a target-domain classifier without directly using labeled target data during training.

---

## Method Overview

The pipeline follows the patent flow:

1. Identify statistically significant source-domain keywords.
2. Label target-domain keywords using an external signal.
3. Extract common keywords with consistent polarity across domains.
4. Train an initial classifier using only shared consistent features.
5. Pseudo-label high-confidence target samples.
6. Train a target-domain classifier iteratively.
7. Combine models through weighted ensembling for refinement.

### Extension

Instead of using a predefined sentiment lexicon, this implementation uses:

- Sentence-transformer embeddings  
- Cosine similarity against positive and negative seed vectors  

This provides a semantic external signal for target keyword labeling.

---

## Datasets Used

- Amazon Polarity Dataset (source domain)  
- Yelp Polarity Dataset (target domain)  

Both datasets were obtained from Hugging Face.

---

## Experimental Setup

Two models are compared:

### 1. Traditional Supervised Baseline
- Trained directly on labeled target training data.
- Fully supervised.

### 2. Patent-Style Domain Adaptation
- Trained using labeled source data.
- Target training data used only through pseudo-labeling.
- No direct target supervision during training.

Evaluation is performed on the Yelp test set.

---

## Results

### Quantitative Comparison

| Model                                   | Target Test Accuracy |
|------------------------------------------|----------------------|
| Traditional (Target-Only Supervised)    | **0.8954**           |
| Patent-Style (Source + Target Unlabeled)| **0.8068**           |

**Accuracy Gap:** -0.0886  

The domain-adaptation approach achieves 80.68% accuracy without using labeled target data during training, compared to 89.54% when fully supervised.

---

### Accuracy Comparison

![Accuracy Comparison](figures/accuracy_bar.png)

---

### Confusion Matrix – Target-Only Supervised

![Confusion Matrix Baseline](figures/confusion_baseline.png)

---

### Confusion Matrix – Patent-Style

![Confusion Matrix Patent](figures/confusion_patent.png)

---

### Supervised Training Curve vs Patent Performance

This plot shows how supervised performance improves as more labeled target samples are used.  
The horizontal line indicates the patent-style performance (0.8068).

![Training Curve Comparison](figures/training_curve.png)

---

## Interpretation

While the fully supervised model achieves higher accuracy, the patent-style framework demonstrates that:

- Competitive performance can be obtained  
- Without directly training on labeled target data  
- Using cross-domain consistent features and iterative pseudo-labeling  

This highlights the practical value of domain adaptation when labeled target data is limited or unavailable.

---

## Attribution

This work is an independent implementation of the ideas presented in:

Sharma, R., Dandapat, S., Bhatt, H. S.  
*Method and System for Training a Target Domain Classifier to Label Text Segments*  
US Patent 20180068231  

All credit for the original conceptual framework belongs to the inventors.
