# Project Documentation

**Authors:** Jakub Nitkiewicz, Jakub Sadowski

## Project Topic
Development of a solution for determining semantic similarity (iSTS) between two English sentences on a document level. The solution is inspired by document-level entity and relation extraction techniques. Results are compared to those from existing master's theses and projects.

### Programming Language
Python (or other languages depending on available solutions).

---

## Tasks

1. Familiarize with the provided thesis/project and the prepared solution.
2. Explore solutions for document-level entity and relation extraction and related tasks.
3. Gather available solutions for document-level entity and relation extraction.
4. Adapt these solutions for iSTS purposes.
5. Compute F-measures (F-score, F-type, F-score+type) for the created system using:
   - SemEval 2015 Interpretable STS (single relations)
   - SemEval 2016 Interpretable STS (complex relations)
   Compare these results with previously provided benchmarks.

---

## Approach

### Document-Level Analysis
The project adopts a document-level approach, analyzing text holistically rather than as isolated sentences or phrases. This involves extracting entities and relationships from the entire document.

### Interpretable Semantic Textual Similarity (iSTS)
This task measures the similarity or relationship between pairs of texts in a manner understandable to humans. It emphasizes:

- **Degree of Similarity (Similarity Score):** Ranges from 0 (no connection) to 5 (equivalence).
- **Relation Type:**
  - **EQUI:** Semantically identical.
  - **OPPO:** Opposite meanings.
  - **SPE1/SPE2:** One phrase is more specific than the other.
  - **SIMI:** Similar meaning but not EQUI, OPPO, SPE1, or SPE2.
  - **REL:** Related but not SIMI, EQUI, OPPO, SPE1, or SPE2.

Additional tags for unmatched phrases:
- **ALIC:** Contains matching tokens.
- **NOALI:** No matching tokens.

---

## Solution Structure

### Model
- **Base Model:** `roberta-large` for optimal performance.
- **Classifier:** 2 feed-forward layers with dropout (10%).
- **Loss Function:** Cross-entropy loss for one-hot vectors.
- **Training Data:** Combined datasets:
  - Headlines
  - Images
  - Answer-Students
- **Evaluation:** Separate test sets for Headlines, Images, and Answer-Students.

### Preprocessing
1. Map `y_type` (relation types) to numerical values and apply one-hot encoding.
2. Preprocess `y_score` similarly.
3. Concatenate `y_type` and `y_score` into a single vector.
4. Tokenize `x1` and `x2` (input sentences) separately using the RoBERTa tokenizer and concatenate results.

---

## Usage Instructions

### Environment
Use Google Colaboratory for ease of computation and collaboration. Some commands may be specific to Colab.

### Steps:
1. Load the program into a Colab notebook.
2. Add the provided folder `NLP` to your Google Drive. This folder includes:
   - Test data
   - Training data
   - `.wa` files for evaluations
   - Perl scripts for F1-measure computation.
3. Execute the program in the notebook.

---

## Datasets

### Sources:
- **SemEval 2015 Interpretable STS**:
  - "Images Captions" (970 training pairs, 943 test pairs)
  - "News Headlines" (2188 training pairs, 1172 test pairs)
- **SemEval 2016 Interpretable STS**:
  - "Answers-Students" (919 training pairs, 933 test pairs)

### Format:
```json
{'x1': 'at 91', 'x2': 'aged 91', 'y_type': 'EQUI', 'y_score': 5}
```
Where:
- `x1`, `x2`: Sentence fragments
- `y_type`: Relation type
- `y_score`: Similarity score (0 to 5)

---

## Testing

### Model Comparison
Tested models from Hugging Face include:
- `roberta-large`
- `roberta-base`
- `xlm-roberta-large`
- `bert-base-uncased`

**Best Results:** Achieved using `roberta-large` with training over 10 epochs (model from epoch 9).

### Evaluation Metrics
- **Scripts Used:**
  - `evalF1_penalty.pl`
  - `evalF1_no_penalty.pl`

#### Example Results:
| Dataset            | F1 Type | F1 Score | F1 Type+Score |
|--------------------|---------|----------|---------------|
| **Images**         | 0.8104  | 0.9569   | 0.8417        |
| **Headlines**      | 0.8136  | 0.9499   | 0.8472        |
| **Answers-Students** | 0.8864  | 0.9629   | 0.8877        |

---

## Challenges and Solutions

### Challenges:
1. **Mapping Variables:** Difficulty in using TypeScript variables in Rust. Solved using `serde_wasm_bindgen`.
2. **Rust Integration:** Initial issues with Rust functions in TypeScript. Solved via dynamic imports and initialization in `useEffect`.

---

## References

- Grudzień, E. (2018). "Semantic Similarity Recognition Using Deep Learning Architectures."
- Budzyńska, A. (2021). "Using BERT-Based Solutions for Determining Semantic Similarity."
- Hugging Face Models: [https://huggingface.co/models](https://huggingface.co/models)
- SemEval Data: [Interpretable STS](http://ixa2.si.ehu.eus/stswiki/index.php/Main_Page#Interpretable_STS)

