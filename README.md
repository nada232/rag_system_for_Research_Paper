#  Project Title
Research Paper RAG System]  
An intelligent question-answering system specialized for analyzing domain-specific academic research papers (e.g., cryptography, medical imaging, NLP, etc.).

##  Overview
This project implements a complete Retrieval-Augmented Generation (RAG) pipeline designed to extract, process, and answer questions from academic PDF documents. The system combines document loading, intelligent chunking, vector embeddings, semantic search, and advanced language models to deliver precise, context-aware answers with source attribution.

##  Features
1-Smart Document Processing: Automatic PDF loading and page-wise segmentation
2-Intelligent Text Splitting: Recursive chunking with overlapping context preservation (300 chars, 50 overlap)
3-Vector Embeddings: OpenAI text-embedding-ada-002 for semantic understanding
4-Similarity Search: FAISS vector database for sub-millisecond document retrieval
5-Dual QA Modes:
Stuff Chain: Single-pass answer generation using all context
Refine Chain: Iterative answer refinement across document chunks
6- Ready RAG Bot: Encapsulated functions (rag_bot(), rag_answer()) for instant deployment
7-Performance Evaluation: DSPy integration with SemanticF1 metric for answer quality assessment

##  Tech Stack

| Component            | Technology                       | Version/Purpose                 |
| -------------------- | -------------------------------- | ------------------------------- |
| **Document Loading** | `LangChain PyPDFLoader`          | PDF parsing and page extraction |
| **Text Splitting**   | `RecursiveCharacterTextSplitter` | Intelligent chunking            |
| **Embeddings**       | `OpenAIEmbeddings`               | `text-embedding-ada-002` model  |
| **Vector DB**        | `FAISS`                          | Local similarity search         |
| **LLM**              | `ChatOpenAI`                     | `gpt-4o-mini` (temperature=0.5) |
| **QA Framework**     | `LangChain QA Chains`            | Stuff & Refine strategies       |
| **Evaluation**       | `DSPy`                           | SemanticF1 metric               |
| **Environment**      | `python-dotenv`                  | Secure API key management       |
| **File Format**      | `PyPDF2`                         | PDF content extraction          |

##  System Architecture
┌─────────────────────────────────────────────────────────────┐
│                     PDF Document Input                      │
│                   │
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
│              Text Splitter (Recursive, 300 chars)           │
│              → Overlapping Chunks + Metadata                │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              Embedding Generator (OpenAI ADA-002)           │
│                 → Vector Representations                    │
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
│              → Retrieve Top-K Relevant Docs                 │
└──────────────────────┬──────────────────────────────────────┘
                       │
        ┌──────────────┴──────────────┐
        ▼                             ▼
┌──────────────┐            ┌──────────────┐
│  Stuff Chain │            │ Refine Chain │
│  (Single-Pass)│            │ (Iterative)  │
└──────────────┘            └──────────────┘
        │                             │
        └──────────────┬──────────────┘
                       ▼
┌─────────────────────────────────────────────────────────────┐
│              LLM Response (GPT-4o-mini)                     │
│              → Answer + Source Documents                    │
└──────────────────────┬──────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────┐
│                 DSPy Evaluation Layer                       │
│              → SemanticF1 Quality Scoring                   │
└─────────────────────────────────────────────────────────────┘

##  Folder Structure
rag-research-system/
├── data/
│   └── papers/                          # Input research PDFs
├── app.env                              
├── basic rag.py                         #  RAG pipeline implementation
├── requirements.txt                     # Dependencies
└── README.md                            

##  How to Run the Project
1-pip install -r requirements.txt
2-Configure Environment run an app.env file in the project root
3-Add PDF Files :Ensure the research paper PDF exists

##  Example Usage
question = "What are the main characteristics of chaotic systems?"
answer = rag_answer(question)
print(answer)
Output: "Chaotic systems include sensitivity to initial conditions, nonlinearity, aperiodicity, fractal structure, and local instability."
 
 ##  Evaluate Custom Questions
 custom_test = {
    "question": "How is chaos applied in cryptography?",
    "response": "Chaos provides pseudo-randomness and sensitivity for secure encryption."
}Re-run DSPy evaluation loop


##  Switch Between QA Modes
stuff_result = stuff_chain({"input_documents": docs, "question": q})
refine_result = refine_chain({"input_documents": docs, "question": q})

##  Important Notes
1- API Costs: gpt-4o-mini and text-embedding-ada-002 incur per-token charges
2- Chunk Size: 300 characters optimized for abstract/summary sections
3- K-Value: Top-5 documents retrieved by default; adjust in similarity_search(k=N)
4- Environment Security: Never commit app.env to version control
5- PDF Naming: Filename must match exactly: Chaos-Based_Image_Encryption_Review.pdf



