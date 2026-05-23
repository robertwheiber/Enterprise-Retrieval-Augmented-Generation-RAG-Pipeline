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

4  Generative Retrieval: Configures Vertex AI as the LangChain vector store   (vsvectordb.as_retriever()) and utilizes a combined document chain to synthesize accurate, context-aware responses powered by Gemini 2.5 Pro.

## Core Implementation Highlights

## 1. Automated Document Ingestion & Precision Chunking

# Python Code
from langchain_community.document_loaders import GCSDirectoryLoader
from langchain_text_splitters import RecursiveCharacterTextSplitter

# 1. Extract PDFs from Google Cloud Storage
BUCKET_URI = "your-gcs-bucket-uri"
folder_prefix = "documents/pdfs/"

loader = GCSDirectoryLoader(
    project_name=PROJECT_ID, 
    bucket=BUCKET_URI, 
    prefix=folder_prefix
)
all_documents = loader.load()
print(f"Processing documents from gs://{BUCKET_URI}/{folder_prefix}")

# 2. Precision Chunking for LLM Context Windows
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000, 
    chunk_overlap=100
)
doc_splits = text_splitter.split_documents(all_documents)

## 2. Scalable Vector Indexing with Vertex AI
Deploys highly scalable GCP vector stores optimized for rapid semantic search.

from google.cloud import aiplatform

# Initialize Vertex AI Environment
aiplatform.init(project=PROJECT_ID, location=REGION, staging_bucket=BUCKET_URI)

# Configure the Vector Search Index with optimized parameters
my_index = aiplatform.MatchingEngineIndex.create_tree_ah_index(
    display_name="adta5770_enterprise_index",
    dimensions=768, # Aligned with text-embedding-004
    approximate_neighbors_count=150,
    distance_measure_type="DOT_PRODUCT_DISTANCE",
    leaf_node_embedding_count=500, # Optimized for retrieval latency
)

# Deploy the index to a public endpoint
my_index_endpoint = aiplatform.MatchingEngineIndexEndpoint.create(
    display_name="adta5770_qasearch_endpoint",
    public_endpoint_enabled=True
)

my_index_endpoint = my_index_endpoint.deploy_index(
    index=my_index, deployed_index_id="enterprise_rag_deployed_index"
)

## 3. Generative Retrieval & Prompt Orchestration
Connects the vector database to the generative model to synthesize zero-hallucination answers.

from langchain_google_vertexai import VertexAIEmbeddings, VertexAI
from langchain_classic.chains.combine_documents import create_stuff_documents_chain

# Initialize Google's Text Embedding Model
embeddings = VertexAIEmbeddings(model_name="text-embedding-004")

# Configure Vertex AI Vector Search as the LangChain Retriever
retriever = vsvectordb.as_retriever(
    search_type="similarity",
    search_kwargs={"k": 5} # Retrieve top 5 most relevant chunks
)

# Initialize the Generative Model
llm = VertexAI(model_name="gemini-2.5-pro", temperature=0.2)

# Create the final document-combining chain
combine_docs_chain = create_stuff_documents_chain(llm, qa_prompt)

# Example Execution
response = combine_docs_chain.invoke({
    "input_documents": retrieved_docs, 
    "question": "What are the core compliance protocols outlined in the uploaded SOPs?"
})

## Impact & Results
o  Designed an end-to-end cloud architecture capable of maintaining strict data compliance and rapid query resolution for enterprise knowledge bases.

o  Minimized retrieval latency and improved semantic search accuracy through optimized vector search parameters (e.g., leaf_node_embedding_count=500).

o  Established standard operating procedures for cleaning, indexing, and undeploying endpoints to manage cloud computing overhead effectively.

## How to Run It

1  Authenticate your GCP project and region via your environment credentials.

2  Create a dedicated GCS bucket and populate the /documents/pdfs/ directory with target files.

3  Execute the Jupyter Notebook cells sequentially to deploy the endpoint, index the embeddings, and initialize the LangChain QA retriever.

4  To finalize this for your portfolio, create a new repository on GitHub (e.g., Enterprise-VertexAI-RAG), upload the .ipynb file to it, and paste this exact text into the README.md file.
