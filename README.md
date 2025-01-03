# Master Project: Temporal Relation Extraction from EHRs

## **Overview**

In the modern healthcare system, overburdened workloads for healthcare providers have become a significant concern. This challenge is evident not only during crises like the COVID-19 pandemic but also in routine operations. Accurate diagnosis and treatment often require a meticulous review of electronic health records (EHRs), including the timing and dosage of treatments. However, this process is time-consuming and can strain resources.

This project leverages **Natural Language Processing (NLP)** to automate the extraction and analysis of temporal information in EHRs. By identifying treatment start and end times and classifying temporal relationships, this approach aims to:
- Alleviate healthcare providers' workload.
- Improve patient care efficiency by reducing waiting times.

---

## **Approaches**
Two distinct methods are employed in this project:

### **1. Sequence Labelling Approach**
- **Objective**: Perform full temporal relation extraction in a single task.
- **Implementation**: Developed in the [`model.ipynb`](model.ipynb) notebook.
- **Performance**: Achieved a macro F1 score of **0.443**.

### **2. Natural Language Inference (NLI) Approach**
- **Objective**: Focus solely on classifying temporal relations between pre-identified entities.
- **Implementation**: Developed in the [`NLI.ipynb`](NLI.ipynb) notebook.
- **Performance**: Achieved a macro F1 score of **0.865**, demonstrating the effectiveness of this approach.

---

## **Key Features**
- **Preprocessing Pipelines**: Automated preprocessing of raw EHR data, generating training and test datasets tailored for each approach.
- **Novel Application**: To the best of our knowledge, this project is the first to apply these methodologies to the temporal relation extraction task in EHRs.

---

## **Runing Code**
**run with jupyter notebook**
### Run the respective notebook for the desired approach:
- Sequence Labelling: model.ipynb
- NLI-based Classification: NLI.ipynb

## **References**
- Cui, Yang, Han, Lifeng, & Nenadic, Goran. (2024). **MedTem: Temporal Relation Extraction of Clinical Events from Free Text Data**. [Link to paper](https://aclanthology.org/2023.acl-srw.27/) *(Replace `#` with actual link)*

