# CTSE_Assignment_2_ChatBot

## 1. Introduction

Large Language Models (LLMs) are advanced AI systems trained on vast datasets to understand and generate human-like text. Despite their capabilities, LLMs are not inherently aware of private or domain-specific content, such as lecture materials. To overcome this limitation, the **Retrieval-Augmented Generation (RAG)** technique is used. This method augments the LLM with an external document retrieval mechanism, allowing it to respond with higher accuracy and contextual relevance.

In this assignment, the RAG technique is used to create a chatbot that can answer questions strictly based on the **Current Trends in Software Engineering (CTSE)** module lecture notes. The chatbot is implemented in a **Jupyter Notebook**, where each code cell corresponds to a key processing stage in the pipeline. This introduction walks through the actual steps taken in the notebook to build the chatbot.

## 2. Pipeline Overview

1. **Lecture Note Extraction**  
   The process begins by reading a combined PDF of CTSE lecture slides and extracting the full text content from each page. The extracted text is saved as a `.txt` file, forming the raw dataset that powers the chatbot.

2. **Text Cleaning**  
   The raw extracted text is cleaned to remove unnecessary artifacts like page numbers or whitespace clutter. This ensures that the content is in a consistent format, suitable for downstream processing.

3. **Document Chunking and Semantic Indexing**  
   The cleaned text is loaded and split into smaller overlapping chunks to preserve context. Each chunk is then converted into a numerical embedding using a high-performance sentence embedding model (**intfloat/e5-large-v2**). These embeddings are stored and indexed using **FAISS**—a vector search library that enables rapid similarity-based retrieval.

4. **Retriever Initialization**  
   The precomputed FAISS index and associated document chunks are reloaded, and a custom retriever is defined. This retriever accepts a user’s query, computes its semantic embedding, and finds the top-matching lecture chunks based on similarity.

5. **Language Model Setup**  
   A text-to-text generation model (**Flan-T5**) is initialized using the Hugging Face `transformers` library. It serves as the LLM component that produces the final response by conditioning on both the user’s question and the retrieved lecture content.

6. **Prompt Design and Chat Loop**  
   A structured prompt template is defined to guide the LLM in generating grounded answers strictly from the provided lecture material. A chat interface is then set up to allow users to ask questions interactively. The system maintains a short history of previous turns and ensures that responses are based only on lecture-relevant content. If the answer is not found in the notes, the bot is designed to respond with:

   > “I don’t know based on the lecture notes.”

## 3. Technology Stack

This structured pipeline allows for an interpretable and accurate chatbot experience, ensuring that all answers are grounded in the actual CTSE lecture material. The full technology stack includes:

- **PyMuPDF** – For PDF text extraction
- **LangChain** – For document loading, chunking, and prompt handling
- **SentenceTransformers (`intfloat/e5-large-v2`)** – For semantic vectorization
- **FAISS** – For efficient vector similarity search
- **Transformers (`Flan-T5`)** – For natural language generation
- **Jupyter Notebook** – As the interactive development and deployment environment

---

Through these carefully defined steps, the chatbot provides helpful, focused answers, supporting students in revisiting and reinforcing lecture content with clarity and confidence.
