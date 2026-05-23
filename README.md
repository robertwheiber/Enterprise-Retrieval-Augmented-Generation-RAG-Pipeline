# Enterprise-Retrieval-Augmented-Generation-RAG-Pipeline
## The Business Problem
### Large organizations possess vast repositories of unstructured PDF data that are difficult to query, often leading to data silos and operational delays. Traditional search engines fail to understand context, yielding poor or irrelevant results. This project architects a Generative Experience Optimization (GEO) pipeline, automating the ingestion of complex enterprise documents and enabling natural language querying with high data fidelity. This ensures strict, zero-hallucination retrieval for high-stakes decision-making.

## The Tech Stack
 Cloud Infrastructure: Google Cloud Platform (GCP)

 o  Orchestration: LangChain (langchain-google-vertexai, langchain-text-splitters)

 o  Generative Model: Gemini 2.5 Pro (gemini-2.5-pro)

 o  Embeddings: Google text-embedding-004 (768 dimensions)

 o  Vector Database: Vertex AI Vector Search (formerly Matching Engine)

 o  Data Storage: Google Cloud Storage (GCS)

## Architecture & Methodology
1  Automated Data Ingestion: Extracts unstructured PDF documents directly from designated Google Cloud Storage buckets (documents/pdfs/), mirroring enterprise data lakes.

2  Precision Chunking: Employs LangChain text splitters to process and divide documents into contextually optimized chunks, ensuring comprehensive data fits precisely within the LLM's context window.

3  Vectorization & Scalable Indexing: Generates high-fidelity embeddings and deploys a Vertex AI Vector Search Index to a public endpoint, establishing a highly scalable vector datastore.

4  Generative Retrieval: Configures Vertex AI as the LangChain vector store (vsvectordb.as_retriever()) and utilizes a combined document chain to synthesize accurate, context-aware responses powered by Gemini 2.5 Pro.

## Impact & Results
o  Designed an end-to-end cloud architecture capable of maintaining strict data compliance and rapid query resolution for enterprise knowledge bases.

o  Minimized retrieval latency and improved semantic search accuracy through optimized vector search parameters (e.g., leaf_node_embedding_count=500).

o  Established standard operating procedures for cleaning, indexing, and undeploying endpoints to manage cloud computing overhead effectively.

## How to Run It

1  Authenticate your GCP project and region via your environment credentials.

2  Create a dedicated GCS bucket and populate the /documents/pdfs/ directory with target files.

3  Execute the Jupyter Notebook cells sequentially to deploy the endpoint, index the embeddings, and initialize the LangChain QA retriever.

4  To finalize this for your portfolio, create a new repository on GitHub (e.g., Enterprise-VertexAI-RAG), upload the .ipynb file to it, and paste this exact text into the README.md file.
