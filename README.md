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

## Earlier work
[MedTem2.0](https://github.com/yang-C23/Third_year_project)

## Dataset Overview

This dataset contains annotated medical text with temporal relations between treatment events and dates. It's designed for training NLP models to identify when medical treatments occurred relative to specific dates (before, after, or overlapping).

Two versions of the dataset are provided:
- **Chunked Data**: Text chunks centered around sentences containing target entities
- **Full Data**: Complete text with all annotations

The data uses the BIO (Beginning-Inside-Outside) tagging scheme to mark entities and their temporal relationships.

## Data Structure

Each entry in the dataset contains:

```python
{
    'tokens': ['token1', 'token2', ...],             # BERT tokenized text
    'bio_time_aligned': ['O', 'B-BEFORE', ...],      # Temporal relation tags
    'bio_treatment_aligned': ['B-TREATMENT', ...]    # Treatment entity tags
}
```

### Label Mapping

```
{
    "O": 0,              # Outside any entity
    "B-BEFORE": 1,       # Beginning of date that comes BEFORE the treatment
    "I-BEFORE": 2,       # Inside of date that comes BEFORE the treatment
    "B-AFTER": 3,        # Beginning of date that comes AFTER the treatment
    "I-AFTER": 4,        # Inside of date that comes AFTER the treatment
    "B-OVERLAP": 5,      # Beginning of date that OVERLAPS with treatment
    "I-OVERLAP": 6       # Inside of date that OVERLAPS with treatment
}
```

## Getting Started

### Installation Requirements

```bash
pip install torch transformers sklearn numpy pandas matplotlib spacy
python -m spacy download en_core_web_sm
```

### Loading the Dataset

```python
import pickle

# Load the chunked dataset
with open('temporal_relation_dataset/chunked_data.pkl', 'rb') as f:
    chunked_data = pickle.load(f)

# Load the full dataset
with open('temporal_relation_dataset/full_data.pkl', 'rb') as f:
    full_data = pickle.load(f)

# Load train/val/test splits
with open('temporal_relation_dataset/splits/train_data.pkl', 'rb') as f:
    train_data = pickle.load(f)

with open('temporal_relation_dataset/splits/val_data.pkl', 'rb') as f:
    val_data = pickle.load(f)

with open('temporal_relation_dataset/splits/test_data.pkl', 'rb') as f:
    test_data = pickle.load(f)

print(f"Loaded {len(chunked_data)} chunked examples")
print(f"Loaded {len(full_data)} full examples")
print(f"Train: {len(train_data)}, Validation: {len(val_data)}, Test: {len(test_data)}")
```

### Exploring a Single Example

```python
def explore_example(example):
    """Display a single example with its annotations."""
    print("TOKENS:")
    print(example['tokens'])
    print("\nTEMPORAL TAGS:")
    print(example['bio_time_aligned'])
    print("\nTREATMENT TAGS:")
    print(example['bio_treatment_aligned'])
    
    # Print the text with highlighted entities
    highlighted_text = []
    for token, time_tag, treatment_tag in zip(example['tokens'], 
                                             example['bio_time_aligned'], 
                                             example['bio_treatment_aligned']):
        if time_tag.startswith('B-') or time_tag.startswith('I-'):
            relation_type = time_tag.split('-')[1]
            highlighted_text.append(f"[TIME:{relation_type}]{token}")
        elif treatment_tag.startswith('B-') or treatment_tag.startswith('I-'):
            highlighted_text.append(f"[TREATMENT]{token}")
        else:
            highlighted_text.append(token)
    
    print("\nHIGHLIGHTED TEXT:")
    print(" ".join(highlighted_text))

# Explore the first example
explore_example(train_data[0])
```


## **References**
- Cui, Yang, Han, Lifeng, & Nenadic, Goran. (2024). **MedTem: Temporal Relation Extraction of Clinical Events from Free Text Data**. [Link to paper](https://www.researchgate.net/profile/Lifeng-Han-3/publication/384463764_MedTem_Temporal_Relation_Extraction_of_Clinical_Events_from_Free_Text_Data/links/66fae52cb753fa724d549fbf/MedTem-Temporal-Relation-Extraction-of-Clinical-Events-from-Free-Text-Data.pdf)
- Yang Cui, Lifeng Han, and Goran Nenadic. 2023. MedTem2.0: Prompt-based Temporal Classification of Treatment Events from Discharge Summaries. In Proceedings of the 61st Annual Meeting of the Association for Computational Linguistics (Volume 4: Student Research Workshop), pages 160–183, Toronto, Canada. Association for Computational Linguistics. [download](https://aclanthology.org/2023.acl-srw.27/)



