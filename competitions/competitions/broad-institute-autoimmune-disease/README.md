---
description: >-
  Crunch Foundation, The Eric and Wendy Schmidt Center, and The Klarman Cell
  Observatory invite you to join the Autoimmune Disease ML Challenge to design
  algorithms to help millions of people.
cover: ../../../.gitbook/assets/banner (2).webp
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Broad Institute Autoimmune Disease

## Challenge Overview

### Introduction

Autoimmune diseases arise when the immune system mistakenly targets healthy cells. Affecting 50M people in the U.S., with rising global cases, Inflammatory Bowel Disease (IBD) is one of the most prevalent forms. IBD occurs when the barrier between our gut and the microbes living there breaks down, leading to the activation of the immune system and persistent inflammation. This cycle of flares and remission increases the risk of colorectal cancer (up to two-fold). Although modern treatments have improved survival, IBD remains challenging to diagnose and treat due to its complex pathogenic pathways and multifactorial nature.

Pathologists rely on **gut tissue images** to diagnose and treat IBD, guiding decisions on the most suitable drug treatments and predicting cancer risk. These tissue images, combined with recent advances in **genomics**, offer a valuable dataset for machine learning models to revolutionize IBD diagnosis and treatment.

{% hint style="info" %}
[Read the full competition specifications here.](full-specifications.md)
{% endhint %}

This challenge is meant for everyone! We have created a three-lecture crash course that provides background on the biology, technology, and data in the three crunches. You do not need a background in biology or medicine to participate.

{% hint style="info" %}
[Find the lecture crash course here.](https://www.youtube.com/watch?v=9OTvuvr81R0\&list=PLlMMtlgw6qNhqMxU8C2V_zsuhlqIgpW6y)
{% endhint %}

## Phases

The challenge is broken down into three Crunches, ordered by increasing complexity.

### **Crunch 1 –** Oct 28 to Feb 9 – **P**redict gene expression in spatial transcriptomics data from matched pathology images

Crunchers will build a model to predict the expression of 460 genes in held-out patches of colon tissue using **H\&E pathology images** and **Xenium spatial transcriptomics** training data. Hematoxylin and Eosin (H\&E) images provide insight into cell organization, while Xenium data add information on gene expression and cellular pathways of disease.

### **Crunch 2 – Nov 18 to Mar 21 – Predicting Unseen Genes**

In this phase, participants will predict the expression of all protein-coding genes, including those that were not measured in the spatial training data, using **single-cell RNA-seq** data as support. This Crunch focuses on leveraging cell transcriptional profiles to enhance the predictive model’s ability to infer the expression of unknown genes in spatial contexts.

### **Crunch 3 – Dec 9 to Apr 30 (submission deadline) / May 15 (peer review deadline) – Identifying Gene Markers for Pre-cancerous Regions**

Participants will rank genes by their ability to distinguish between dysplasia (pre-cancerous) regions and noncancerous tissue in IBD patients, increasing our ability to detect cancer early. The final gene panel will be chosen based on participant performance in Crunch 2 and on peer review of participants' methods taking place after the submission deadline. The gene panel will be experimentally validated in a new colon tissue with dysplasia, and all participants' ranked gene lists will be scored.

{% hint style="info" %}
The best-performing models will be experimentally tested to validate their ability to predict cancer risk, which could lead to early detection and improved treatment options. The best models will be publish in an official publication from Broad Institute.
{% endhint %}

<figure><img src="../../../.gitbook/assets/Screenshot 2024-10-24 at 14.35.49.png" alt=""><figcaption></figcaption></figure>

### **Participant Output Requirements**

For each Crunch, participants must submit predictions in CSV format. Each submission must adhere to the provided **log1p-normalization** standards.

Outputs will be evaluated using:

* **Mean Squared Error** (Crunch 1). To avoid overfitting Crunch will score all submitted models on a private dataset during two Checkpoints.
* **Spearman’s Correlation** (Crunch 2)
* **Accuracy** and **Diversity Metrics** (Crunch 3)

### **Evaluation Criteria**

Performance will be evaluated through:

* **Accuracy in gene expression prediction** (Crunch 1 & 2)
* **Gene panel design** for distinguishing between noncancerous and dysplasia regions (Crunch 3)
* **Diversity of selected gene programs** in Crunch 3, with extra emphasis on identifying unique biological pathways
* **Peer review** of methods to select dysplasia gene panel in Crunch 3

## Prizes

### Crunch 1

| Winner’s rank | Prize value (in USD) |
| ------------- | -------------------- |
| 1st Place     | 3,500                |
| 2nd Place     | 2,500                |
| 3rd Place     | 1,750                |
| 4th Place     | 900                  |
| 5th Place     | 750                  |
| 6th Place     | 700                  |
| 7th Place     | 600                  |
| 8th Place     | 500                  |
| 9th Place     | 450                  |
| 10th Place    | 350                  |
| Total         | 12,000               |

### Crunch 2

| Winner’s rank | Prize value (in USD) |
| ------------- | -------------------- |
| 1st Place     | 3,500                |
| 2nd Place     | 2,500                |
| 3rd Place     | 1,750                |
| 4th Place     | 900                  |
| 5th Place     | 750                  |
| 6th Place     | 700                  |
| 7th Place     | 600                  |
| 8th Place     | 500                  |
| 9th Place     | 450                  |
| 10th Place    | 350                  |
| Total         | 12,000               |

### Crunch 3

| Winner’s rank | Prize value (in USD) |
| ------------- | -------------------- |
| 1st Place     | 7,000                |
| 2nd Place     | 6,500                |
| 3rd Place     | 5,000                |
| 4th Place     | 3,000                |
| 5th Place     | 1,000                |
| 6th Place     | 900                  |
| 7th Place     | 800                  |
| 8th Place     | 700                  |
| 9th Place     | 600                  |
| 10th Place    | 500                  |
| Total         | 26,000               |

## External Resources

Crunchers are encouraged to use publicly available external resources, including gene expression datasets and pre-trained models, as long as they are properly credited.

A list of potential resources and references is provided in the [full challenge specifications.](full-specifications.md)

**Foundry Institute** offers a computing environment with $10 USD equivalent to around 10h of GPU time.

{% hint style="info" %}
[Find ML Foundry documentation here.](https://docs.mlfoundry.com/foundry-documentation)
{% endhint %}
