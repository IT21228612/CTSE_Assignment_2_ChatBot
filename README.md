# CTSE Chatbot using RAG in Jupyter Notebook

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

## 4. How to Run

Follow these steps to run the chatbot locally:

1. **Download all the project files** from this repository to your local machine.

2. **Open the `CTSE_chatbot_IT21228612.ipynb` file** using Jupyter Notebook or JupyterLab.

3. **Run the notebook cell by cell** from top to bottom.

4. You will likely encounter errors indicating that **some libraries or frameworks are missing**.

5. **Copy each error message** and **paste it into ChatGPT or any AI assistant** to identify which Python package is missing or needs to be installed.

6. **Install the required packages** using `pip` inside the notebook or your terminal. For example:

   ```bash
   pip install pymupdf langchain sentence-transformers faiss-cpu transformers jupyter

   ```

7. After installing all the required packages, restart the notebook kernel and run the cells again.

8. Once all cells run without errors, you can interact with the chatbot and ask questions based on the CTSE lecture notes.
