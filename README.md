# 🧠 Medical QA with Knowledge-Augmented Prompts

This project presents a pipeline for improving multiple-choice medical question answering using structured medical knowledge. By integrating UMLS-derived knowledge triplets and PubMed abstracts into prompt augmentation, the system enhances LLM performance in clinical reasoning tasks.

---

## 📁 Project Structure

### 📘 `KB_Extration_and_Creation.ipynb` – Knowledge Base Construction

This notebook builds a concept-level knowledge base for each answer option in the MedQA dataset:

- **UMLS Concept Triplets**  
  Attempts to extract subject–predicate–object (SPO) triplets related to each answer option from the UMLS knowledge graph.

- **Fallback: PubMed Sentence Retrieval**  
  If fewer than 60 triplets are available, additional context is retrieved from PubMed abstracts using keyword-based search.

- **Masking Strategy**  
  The answer option is masked with `[MASK]` in each sentence to prepare for contextual embedding (as in mention vector modeling).

- **Embedding with Bio_ClinicalBERT**  
  Masked sentences are embedded using [`emilyalsentzer/Bio_ClinicalBERT`](https://arxiv.org/abs/1904.03323), a clinical domain-specific transformer model.

- **Averaging**  
  The sentence embeddings for each answer option are averaged to produce a single concept vector.

- **Storage**  
  Each answer option is mapped to its final averaged embedding and saved in a JSON file:  
  `optionll_embeddings_and_sentences.json`

---

### 📘 `Prompt_Augmentation.ipynb` – Knowledge-Enhanced Inference

This notebook performs medical question answering using the knowledge vectors:

- **Load Questions**  
  Reads MedQA-style multiple-choice questions (with five options).

- **Embed Questions**  
  The stem of each question is embedded using the same Bio_ClinicalBERT model for consistency with the option embeddings.

- **Semantic Similarity Scoring**  
  Cosine similarity is computed between the question vector and each answer option’s vector from the knowledge base.

- **Prompt Augmentation**
  Add context saved + scores into our prompt for each answer option

- **Generate Answer**
---

##Requirements
  1. HuggingFace Token
  2. UMLS License: A valid license is required to access and download the Unified Medical Language System (UMLS) data
  3. Access to the PubMed API: Access to PubMed’s E-utilities (Entrez Programming Utilities) for retrieving biomedical abstracts
  4. Medqa Dataset

## 🧪 Models Used
  emilyalsentzer/Bio_ClinicalBERT
  meta-llama/Llama-3.1-8B-Instruct
##📚 Dataset
  MedQA (USMLE-style questions)
  
**Knowledge Source**:  
  - Unified Medical Language System (UMLS)  
  - PubMed (via Entrez API)
