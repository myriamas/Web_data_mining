Here's the link the notebook with all the outputs and the visualizations of our results : https://colab.research.google.com/drive/1VVp6Z3eqZk7gNX48_mYzD3DDC06M9cdV

# Web Semantics Project - Knowledge Graph Construction & Embedding

## Overview
This project builds an end-to-end pipeline for constructing and analyzing a knowledge graph from English news articles. It covers every step from web scraping and NLP-based information extraction to RDF graph generation, enrichment with DBpedia, and knowledge graph embedding with PyKEEN.

The goal was to develop a complete, reproducible workflow for transforming unstructured text into structured, queryable knowledge and evaluating it through link prediction models.

---

## Part 1 - Knowledge Graph Construction

### **Text Preprocessing**
A full preprocessing pipeline was implemented:
- Lowercasing  
- Stop-word removal  
- Lemmatization  
- Punctuation filtering  
- Batch processing with *nlp.pipe()* for improved performance  

### **Named Entity Recognition**
Two approaches were compared:
- **CRF model** with handcrafted token-level and contextual features  
- **spaCy transformer model (en_core_web_trf)**  

CRF achieved the best scores due to BIO alignment and strong token dependency modeling.

### **Relation Extraction**
A hybrid strategy combining:
- Dependency-based patterns  
- Regex rules mapped to DBpedia predicates  

This enabled extraction of relations such as *worksAt*, *hasAward*, *hasChild*, etc.

### **RDF Graph Construction**
Using **RDFlib**, we:
- Defined namespaces  
- Created triples with URIs and Literals  
- Serialized the graph to RDF/XML  
- Validated structure using SPARQL queries  

### **Web Scraping**
- Initial pipeline (Politico) blocked by anti-bot mechanisms  
- Final scraping source: **NPR News**  
- Extracted titles, URLs, and cleaned article text into structured JSON  

This provided 24 high-quality articles for knowledge extraction.

---

## Part 2 - Knowledge Graph Embedding with PyKEEN

### **Entity Linking & Enrichment**
DBpedia enrichment was performed via exact label matching:
- Queried related facts for each entity  
- Added new triples without modifying URIs  
- Expanded the graph from **78 → 4233 triples**  

### **Embedding Pipeline**
Triples were converted into PyKEEN format and used to train three models:
- **TransE**
- **DistMult**
- **RotatE**

Models were evaluated with:
- Mean Rank (MR)
- Mean Reciprocal Rank (MRR)
- Hits@k metrics

**DistMult** achieved the strongest performance across all metrics.

### **Visualizations**
PCA and t-SNE projections were generated for:
- Entity embeddings  
- Relation embeddings  
- Top-k predictions for sample triples  

These visualizations confirmed structural patterns and cluster formation.

---

## Key Insights
- CRF NER outperformed spaCy for entity extraction on this dataset.  
- Rule-based and dependency-based relation extraction produced coherent triples.  
- DBpedia enrichment significantly increased graph density and semantic richness.  
- DistMult delivered the most consistent link prediction performance.  

---

## Technologies Used
- Python, BeautifulSoup, Selenium (scraping)  
- spaCy, sklearn-crfsuite (NER)  
- RDFlib (RDF graph construction)  
- PyKEEN (embeddings & link prediction)  
- scikit-learn (t-SNE, PCA)
