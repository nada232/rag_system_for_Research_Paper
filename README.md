#  Project Title  
**Research Paper RAG System**  
An intelligent question-answering system specialized for analyzing domain-specific academic research papers (e.g., cryptography, medical imaging, NLP, etc.).

---

##  Overview  
This project implements a complete **Retrieval-Augmented Generation (RAG)** pipeline designed to extract, process, and answer questions from academic PDF documents.

The system combines:
- Document loading  
- Intelligent chunking  
- Vector embeddings  
- Semantic search  
- Advanced language models  

to deliver precise, context-aware answers with **source attribution**.

---

##  Features  
1. **Smart Document Processing** – Automatic PDF loading and page-wise segmentation  
2. **Intelligent Text Splitting** – Recursive chunking with overlapping context (300 chars, 50 overlap)  
3. **Vector Embeddings** – OpenAI `text-embedding-ada-002`  
4. **Similarity Search** – FAISS vector database  
5. **Dual QA Modes**  
   - Stuff Chain (single-pass)  
   - Refine Chain (iterative refinement)  
6. **Ready RAG Bot** – Functions: `rag_bot()`, `rag_answer()`  
7. **Evaluation** – DSPy with SemanticF1 metric  

---

##  Tech Stack  

| Component            | Technology                       | Purpose                         |
|----------------------|----------------------------------|---------------------------------|
| **Document Loading** | LangChain `PyPDFLoader`          | PDF parsing                     |
| **Text Splitting**   | `RecursiveCharacterTextSplitter` | Chunking                        |
| **Embeddings**       | `OpenAIEmbeddings`               | `ada-002` vector model          |
| **Vector DB**        | `FAISS`                          | Fast similarity search          |
| **LLM**              | `ChatOpenAI`                     | `gpt-4o-mini`                   |
| **QA Framework**     | LangChain QA Chains              | Stuff & Refine                  |
| **Evaluation**       | DSPy                             | SemanticF1                      |
| **Environment**      | python-dotenv                    | API key loading                 |
| **PDF Parsing**      | PyPDF2                           | PDF extraction                  |

---

## � System Architecture

```text
┌─────────────────────────────────────────────────────────────┐
│                     PDF Document Input                      │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                  Document Loader (PyPDF)                    │
│                    → Pages Extraction                       │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│          Text Splitter (Recursive, 300 chars)               │
│          → Overlapping Chunks + Metadata                    │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│       Embedding Generator (OpenAI ADA-002)                  │
│          → Vector Representations                           │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                  FAISS Vector Database                      │
│              → Semantic Search Index (k=5)                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│               Similarity Search Engine                      │
│             → Retrieve Top-K Relevant Docs                  │
└──────────────────────┬──────────────────────────────────────┘
                       │
        ┌──────────────┴──────────────┐
        ▼                             ▼
┌──────────────┐            ┌──────────────┐
│  Stuff Chain │            │ Refine Chain │
│ (Single-Pass)│            │ (Iterative)  │
└──────────────┘            └──────────────┘
        │                             │
        └──────────────┬──────────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              LLM Response (GPT-4o-mini)                     │
│            → Answer + Source Documents                      │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                 DSPy Evaluation Layer                       │
│             → SemanticF1 Quality Scoring                    │
└─────────────────────────────────────────────────────────────┘

##  Folder Structure

rag-research-system/
├── data/
│   └── papers/                  # Input research PDFs
├── app.env                      
├── basic rag.py                 # RAG pipeline implementation
├── requirements.txt             
└── README.md                    

## How to Run the Project

1. **Install dependencies**: `pip install -r requirements.txt`
2. **Configure environment**: Create an `app.env` file in the project root directory
3. **Add PDF files**: Ensure the research paper PDF exists in the appropriate location
4. **Run RAG function**

## Example Usage

```python
question = "What are the main characteristics of chaotic systems?"
answer = rag_answer(question)
print(answer)
# Output: "Chaotic systems include sensitivity to initial conditions, nonlinearity, aperiodicity, fractal structure, and local instability."

## Switch Between QA Modes

```python
stuff_result = stuff_chain({"input_documents": docs, "question": q})
refine_result = refine_chain({"input_documents": docs, "question": q})

## Evaluate Custom Questions

```python
custom_test = {
    "question": "How is chaos applied in cryptography?",
    "response": "Chaos provides pseudo-randomness and sensitivity for secure encryption."
}
##  Important Notes
1- API Costs: gpt-4o-mini and text-embedding-ada-002 incur per-token charges
2- Chunk Size: 300 characters optimized for abstract/summary sections
3- K-Value: Top-5 documents retrieved by default; adjust in similarity_search(k=N)
4- Environment Security: Never commit app.env to version control
5- PDF Naming: Filename must match exactly: Chaos-Based_Image_Encryption_Review.pdf


