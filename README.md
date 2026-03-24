# Construction Marketplace RAG Assistant

A Retrieval-Augmented Generation (RAG) pipeline built for a construction marketplace to answer questions based strictly on internal documents.

##  How to Run Locally
1. Clone this repository.
2. Install the dependencies: `pip install -r requirements.txt`
3. Add your OpenRouter API key to the code where indicated.
4. Run the application/notebook. 
5. A Gradio interface will launch in your browser or notebook.

##  Architecture & Design Choices

### 1. Document Chunking & Retrieval
- **Chunking:** Documents are processed using LangChain's `RecursiveCharacterTextSplitter`. I used a `chunk_size` of 150 characters with an `overlap` of 20 to ensure sentences aren't abruptly cut off, preserving the context of the construction policies.
- **Vector Indexing:** I used **FAISS** (`IndexFlatL2`) for local vector indexing. It performs a semantic similarity search to retrieve the top *k* most relevant chunks based on the user's query.

### 2. Models Used
- **Embedding Model:** `all-MiniLM-L6-v2` (via SentenceTransformers). Chosen because it is an open-source, highly efficient model that runs locally and provides excellent semantic representation for short text.
- **LLM:** `openrouter/free` (Auto-routed to the best available free model, such as Llama 3 or Mistral). Chosen to provide high-quality generation without incurring API costs.

### 3. Grounding & Transparency
- **Enforced Grounding:** The LLM is strictly constrained via a System Prompt: *"Answer the user's question using ONLY the provided context... If the answer is not in the context, say 'I don't know'."* This prevents hallucinations.
- **Explainability:** The Gradio interface explicitly displays the exact retrieved document chunks used to generate the answer immediately below the response, ensuring full transparency.
