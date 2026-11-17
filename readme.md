# AI-Powered RAG-Based Document Question Answering System

An AI-powered document question-answering application built using **Retrieval-Augmented Generation (RAG)**, Python, FastAPI, Streamlit, Google Gemini and ChromaDB.

The application allows users to upload PDF, TXT and DOCX documents, process their content, store document embeddings in a vector database, and ask natural-language questions to receive context-aware answers based on the uploaded knowledge base.

---

## 1. Project Overview

Traditional AI chatbots generate answers using their pre-trained knowledge and may not know the information contained in private documents.

This project addresses that limitation using Retrieval-Augmented Generation (RAG).

The system retrieves relevant information from uploaded documents and provides that information as context to Google's Gemini language model, which generates answers based on the retrieved content.

### Key Objectives

- Upload and process multiple document formats.
- Extract text from PDF, TXT and DOCX files.
- Divide large documents into smaller text chunks.
- Generate numerical embeddings using Google Gemini.
- Store and manage embeddings using ChromaDB.
- Perform semantic similarity searches.
- Generate context-aware responses using Gemini.
- Provide an interactive chat interface using Streamlit.

---

## 2. Features

- **Multi-format Document Upload:** Supports PDF, TXT and DOCX files.
- **Text Extraction:** Extracts readable content from uploaded documents.
- **Text Chunking:** Divides large documents into smaller chunks with overlapping content.
- **Embedding Generation:** Converts text chunks into numerical vector representations.
- **Vector Storage:** Stores embeddings and associated text in ChromaDB.
- **Semantic Search:** Retrieves relevant document chunks based on the meaning of user queries.
- **AI-Powered Responses:** Uses Google Gemini to generate natural-language answers.
- **Context-Based Answering:** Instructs the LLM to answer using retrieved document context.
- **Interactive Interface:** Provides a modern Streamlit-based chat workspace.
- **Persistent Storage:** Maintains vector data using ChromaDB's persistent directory.

---

## 3. Technology Stack

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Streamlit | Frontend development |
| FastAPI | Backend API development |
| LangChain | Vector store and embedding integration |
| Google Gemini Embeddings | Text embedding generation |
| Google Gemini Flash | AI response generation |
| ChromaDB | Vector database |
| Pydantic | API request validation |
| PyPDF | PDF text extraction |
| Python-Dotenv | Environment variable management |
| Requests | Frontend-to-backend communication |

---

## 4. Project Architecture

The application follows a frontend-backend architecture.

### Frontend
- Developed using Streamlit.
- Handles document uploads.
- Extracts document text.
- Performs text chunking.
- Sends chunks and user questions to the backend.
- Displays generated responses.

### Backend
- Developed using FastAPI.
- Provides REST API endpoints.
- Receives document chunks.
- Manages embedding generation and vector storage.
- Performs semantic similarity searches.
- Generates prompts and communicates with Gemini.

### Vector Database
- Uses ChromaDB.
- Stores document embeddings and original text.
- Retrieves relevant chunks based on user queries.

### Architecture Workflow

```text
              USER
                |
                v
        STREAMLIT FRONTEND
                |
        Document Upload
                |
                v
        TEXT EXTRACTION
                |
                v
          TEXT CHUNKING
                |
                v
        FASTAPI BACKEND
                |
                v
       GEMINI EMBEDDINGS
                |
                v
            CHROMADB
          (Vector Storage)
                |
                |
       USER ASKS QUESTION
                |
                v
        SEMANTIC SEARCH
                |
                v
      RETRIEVE TOP 2 CHUNKS
                |
                v
         PROMPT CREATION
                |
                v
          GEMINI LLM
                |
                v
        GENERATED ANSWER
                |
                v
       STREAMLIT CHAT UI
```

---

## 5. Project Directory Structure

```text
Rag Based Question Answering System
    ├── Backend/
    │   │
    │   ├── myenv/
    │   │
    │   ├── .env
    │   ├── main.py
    │   ├── database.py
    │   ├── services.py
    │   ├── requirements.txt
    │   │
    │   └── chroma_db/
    │
    └── frontend/
        │
        ├── frontend.py
        ├── chunking.py
        └── package-lock.json
```

### File Responsibilities

| File | Description |
|---|---|
| `main.py` | Defines FastAPI endpoints and handles incoming requests. |
| `database.py` | Configures Gemini embeddings, ChromaDB, document storage and similarity search. |
| `services.py` | Creates prompts, retrieves context and generates Gemini responses. |
| `frontend.py` | Implements the Streamlit interface, document extraction and chat functionality. |
| `chunking.py` | Divides extracted document text into smaller overlapping chunks. |
| `.env` | Stores the Gemini API key securely. |
| `requirements.txt` | Contains backend Python dependencies. |
| `chroma_db/` | Stores persistent ChromaDB data. |

---

## 6. RAG Workflow

The application follows these major processing stages.

### Step 1: Document Upload

Users upload documents through the Streamlit interface.

Supported formats:
- PDF
- TXT
- DOCX

### Step 2: Text Extraction

The frontend extracts readable text from uploaded documents.

- PDF: Uses PyPDF.
- TXT: Uses Python file decoding.
- DOCX: Extracts text from the document XML structure.

### Step 3: Text Chunking

The extracted text is divided into smaller pieces using the custom `split_text()` function.

Current configuration:

| Parameter | Value |
|---|---:|
| Chunk Size | 500 characters |
| Chunk Overlap | 50 characters |

Overlapping chunks help preserve contextual continuity between neighboring text segments.

### Step 4: Embedding Generation

The extracted chunks are sent to the FastAPI backend.

Google Gemini Embeddings converts each text chunk into a numerical vector representation.

Embedding model:

`gemini-embedding-001`

### Step 5: Vector Storage

The generated embeddings and associated text are stored in ChromaDB.

Collection name:

`myCollection`

Persistence directory:

`./chroma_db`

### Step 6: User Query

The user enters a natural-language question through the Streamlit chat interface.

The question is sent to the backend through the `/chat` endpoint.

### Step 7: Semantic Similarity Search

ChromaDB compares the question with stored document embeddings and retrieves the most relevant chunks.

Current retrieval configuration:

`k = 2`

This retrieves the two most relevant chunks.

### Step 8: Context and Prompt Creation

The retrieved chunks are combined into a context.

A prompt is created using LangChain's `ChatPromptTemplate`.

The prompt contains:
- System instructions.
- Retrieved document context.
- User question.
- Answer generation instructions.

### Step 9: Response Generation

The final prompt is sent to the Google Gemini Flash model.

The model generates a natural-language response using the retrieved context.

### Step 10: Display Answer

The generated response is returned through FastAPI and displayed in the Streamlit chat interface.

---

## 7. API Endpoints

The backend provides the following REST API endpoints.

| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | Checks backend availability. |
| POST | `/collection` | Stores document chunks in ChromaDB. |
| GET | `/collection` | Retrieves collection data. |
| POST | `/chat` | Processes user questions and generates answers. |

### API Testing

FastAPI Swagger documentation:

`http://127.0.0.1:8000/docs`

#### A. Check Backend

**GET /**

Example response:

```json
{
  "Welcome": "RAG Application"
}
```

#### B. Store Documents

**POST /collection**

Request body:

```json
{
  "documents": [
    "Python is a programming language.",
    "FastAPI is a Python framework for building APIs.",
    "RAG stands for Retrieval Augmented Generation."
  ]
}
```

Example response:

```json
{
  "message": "Documents stored successfully!"
}
```

#### C. Retrieve Collection

**GET /collection**

Returns the stored collection data.

#### D. Ask a Question

**POST /chat**

Request body:

```json
{
  "prompt": "What is FastAPI?"
}
```

Example response:

```json
{
  "response": "FastAPI is a Python framework used for building APIs."
}
```

---

## 8. Installation and Setup

### Prerequisites

Make sure the following are installed:

- Python 3.11
- pip
- Visual Studio Code
- Google Gemini API Key

### Step 1: Clone the Repository

```bash
git clone YOUR_REPOSITORY_URL
```

Navigate to the project directory:

```bash
cd ragproject/rag
```

### Step 2: Create a Virtual Environment

Navigate to the Backend directory:

```bash
cd Backend
```

Create the virtual environment:

```bash
python -m venv myenv
```

Activate it on Windows:

```powershell
.\myenv\Scripts\activate
```

### Step 3: Install Dependencies

```bash
pip install -r requirements.txt
```

### Step 4: Configure Environment Variables

Create a `.env` file inside the Backend directory.

Add your Gemini API key:

```env
GEMINI_API_KEY=your_actual_gemini_api_key
```

**Important:** Never upload your actual API key to GitHub.

### Step 5: Run the Backend

From the Backend directory:

```bash
python -m uvicorn main:app --reload
```

Backend URL:

`http://127.0.0.1:8000`

### Step 6: Run the Frontend

Open another terminal.

Navigate to the frontend directory:

```bash
cd frontend
```

Activate the Backend virtual environment:

```powershell
..\Backend\myenv\Scripts\activate
```

Install frontend dependencies if they are not already installed:

```bash
python -m pip install streamlit requests pypdf
```

Run Streamlit:

```bash
streamlit run frontend.py
```

Frontend URL:

`http://localhost:8501`

---

## 9. Configuration

The following configurations are currently used in the application.

| Configuration | Value |
|---|---|
| Embedding Model | `gemini-embedding-001` |
| LLM | `gemini-3.8-flash` |
| Vector Database | ChromaDB |
| Collection Name | `myCollection` |
| Chunk Size | 500 |
| Chunk Overlap | 50 |
| Retrieval Count | 2 |
| Backend Framework | FastAPI |
| Frontend Framework | Streamlit |

---

## 10. Error Handling and Limitations

### Current Limitations

- Answer quality depends on the quality of uploaded documents.
- Retrieval currently returns only two chunks.
- The current chunking approach uses character-based splitting.
- The application relies on an active internet connection for Gemini API access.
- API usage may be subject to Google Gemini rate limits and quotas.
- Scanned PDFs may require OCR because ordinary text extraction cannot reliably read images.
- Document metadata and source citations are not currently included in the chat response.

### Security Considerations

- Store API credentials in `.env`.
- Add `.env`, `myenv/` and `chroma_db/` to `.gitignore`.
- Never commit API keys or private documents to a public repository.
- Configure appropriate authentication before exposing the API publicly.

---

## 11. Future Enhancements

Potential improvements include:

- Implement recursive and document-structure-based chunking.
- Add metadata-based document retrieval.
- Implement source citations in generated answers.
- Add OCR support for scanned PDFs.
- Support more document formats.
- Implement conversation memory.
- Add document deletion and collection management.
- Introduce hybrid search combining keyword and semantic retrieval.
- Add response evaluation and retrieval accuracy testing.
- Deploy the application to a cloud platform.

---

## 12. Learning Outcomes

Through this project, the following concepts and technologies are explored:

- Retrieval-Augmented Generation (RAG).
- Natural Language Processing (NLP).
- Text preprocessing and chunking.
- Vector embeddings.
- Vector databases.
- Semantic similarity search.
- Prompt engineering.
- Large Language Models (LLMs).
- REST API development.
- Frontend-backend integration.
- Environment and dependency management.

---

## 13. Conclusion

This project demonstrates the practical implementation of a Retrieval-Augmented Generation system for document-based question answering.

By integrating Streamlit, FastAPI, Google Gemini and ChromaDB, the application provides a simple way to interact with private document collections through natural-language questions.

The project combines document processing, vector search and generative AI into a single application, demonstrating an end-to-end RAG workflow.

---

**Project Type:** AI / Machine Learning / Generative AI

**Core Concept:** Retrieval-Augmented Generation (RAG)

**Primary Language:** Python

**Status:** Functional Prototype
