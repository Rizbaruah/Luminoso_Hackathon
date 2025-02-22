# Open Chat: Advanced RAG-based Customer Experience Analyst

An intelligent virtual customer experience analyst powered by Google's Gemini Pro and advanced RAG (Retrieval-Augmented Generation) techniques. This system processes customer data, generates insights, and provides role-specific responses for executives, analysts, and store representatives.

## Table of Contents
- [Overview](#overview)
- [Architecture](#architecture)
- [RAG Implementation](#rag-implementation)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [Example Workflow](#example-workflow)
- [Technical Details](#technical-details)
- [Configuration](#configuration)

## Overview

Open Chat is a sophisticated system that combines modern RAG techniques with Google's Gemini Pro model to provide intelligent analysis of customer experience data. It features:

- Role-based responses (Executive, Analyst, Store Representative)
- Advanced RAG with iterative refinement
- Real-time data processing and analysis
- Interactive web interface
- Actionable insights generation
- Markdown-formatted responses

## Architecture

The system follows a modular architecture with several key components:

1. **Web Interface Layer**
   - FastAPI-based REST API
   - Real-time response streaming
   - Role-based query handling

2. **RAG Engine**
   - Document retrieval using ChromaDB
   - Query refinement with Gemini Pro
   - Iterative reasoning process
   - Context-aware response generation

3. **Analysis Layer**
   - Customer data processing
   - Insight generation
   - Statistical analysis
   - Trend identification

## RAG Implementation

Our RAG implementation is inspired by and builds upon several state-of-the-art approaches:

### Core RAG Components

1. **Document Retrieval**
   ```python
   class DocumentRetriever:
       def retrieve(self, query: str) -> List[Dict[str, Any]]:
           # Semantic search using ChromaDB
           relevant_docs = self.vector_db.similarity_search(query)
           return self._process_results(relevant_docs)
   ```

2. **Query Refinement**
   ```python
   class QueryRefiner:
       def refine_query(self, original_query: str, docs: List[Dict], reflection: str) -> str:
           # Use Gemini Pro to refine the query based on context
           refined_query = self.model.generate_content(self._construct_prompt())
           return refined_query
   ```

3. **Iterative Reasoning**
   ```python
   class ReasoningAgent:
       def reason(self, query: str, role: str) -> Dict[str, Any]:
           # Iterative reasoning process with convergence checking
           while not converged and iterations < max_iterations:
               reflection = self.generate_reflection()
               refined_query = self.refine_query()
               documents = self.retrieve_documents()
   ```

### Innovations and Improvements

Our implementation includes several improvements over traditional RAG:

1. **Iterative Refinement Loop**
   - Query refinement based on retrieved documents
   - Reflection generation for context understanding
   - Convergence detection for optimal results

2. **Role-Based Processing**
   - Custom prompts for different user roles
   - Role-specific insight generation
   - Tailored response formatting

3. **Enhanced Retrieval**
   - Semantic search with ChromaDB
   - Document reranking
   - Context-aware filtering

## Project Structure

```
.
├── open_chat
│   ├── README.md
│   ├── data_loader.py
│   ├── api
│   │   └── server.py
│   ├── agent
│   │   ├── chat_agent.py
│   │   ├── error_handler.py
│   │   └── __init__.py
│   ├── analysis
│   │   └── __init__.py
│   ├── config
│   │   ├── config.yaml
│   │   ├── settings.py
│   │   ├── logger_config.py
│   │   └── __init__.py
│   ├── docs
│   │   └── usage.md
│   ├── generation
│   │   ├── answer_generator.py
│   │   ├── response_formatter.py
│   │   ├── role_based_formatter.py
│   │   └── __init__.py
│   ├── reasoning
│   │   ├── reasoning_agent.py
│   │   ├── loop_controller.py
│   │   ├── reflection_module.py
│   │   ├── insights_module.py
│   │   ├── query_refinement.py
│   │   └── __init__.py
│   ├── retrieval
│   │   ├── retriever.py
│   │   ├── cache_manager.py
│   │   └── __init__.py
│   └── tests
│       └── test_integration.py
├── requirements.txt
├── .env.example
├── data
│   ├── customer_data
│   └── processed_data
└── logs
```

## Example Workflow

Let's walk through how the system processes a query:

1. **User Input**
   ```
   Query: "What is the average customer rating like?"
   Role: "executive"
   ```

2. **Initial Processing**
   - Query is received by FastAPI endpoint
   - Role-specific context is initialized
   - RAG process begins

3. **RAG Pipeline**
   ```python
   # 1. Initial document retrieval
   docs = retriever.retrieve(query)
   
   # 2. Generate reflection
   reflection = reflection_module.generate_reflection(query, docs)
   
   # 3. Refine query
   refined_query = query_refiner.refine_query(query, docs, reflection)
   
   # 4. Retrieve with refined query
   updated_docs = retriever.retrieve(refined_query)
   ```

4. **Response Generation**
   - Insights are extracted from documents
   - Role-specific formatting is applied
   - Actionable insights are generated

5. **Final Output**
   ```json
   {
     "main_response": "Customer satisfaction shows a mixed pattern...",
     "actionable_insights": [
       "Improve online product photography",
       "Address shipping delays to West Coast",
       "Enhance online shopping experience"
     ]
   }
   ```

## Technical Details

### Key Technologies

- **Backend**: Python 3.8+, FastAPI
- **Models**: Google Gemini Pro
- **Vector Store**: ChromaDB
- **Frontend**: HTML5, JavaScript (marked.js for markdown)
- **Data Processing**: pandas, numpy, scikit-learn

### Performance Optimizations

1. **Caching**
   - Document embedding cache
   - Response cache for similar queries
   - Vector store optimization

2. **Async Processing**
   - Asynchronous API endpoints
   - Background task processing
   - Parallel document processing

3. **Resource Management**
   - Connection pooling
   - Rate limiting
   - Error handling and recovery

## Configuration

The system is highly configurable through `config.yaml`:

```yaml
models:
  generation_model:
    model_name: "gemini-pro"
    max_length: 2048
    temperature: 0.7
    
retrieval:
  top_k: 5
  similarity_threshold: 0.7
  cache_size: 1000
  
reasoning:
  max_iterations: 3
  convergence_threshold: 0.95
```

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/open_chat.git
   cd open_chat
   ```

2. Create a virtual environment:
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # Linux/Mac
   # or
   .venv\Scripts\activate  # Windows
   ```

3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

4. Set up environment variables:
   ```bash
   cp .env.example .env
   # Edit .env with your API keys
   ```

5. Run the application:
   ```bash
   uvicorn open_chat.api.server:app --host 0.0.0.0 --port 8000 --reload
   ```

## Usage

Access the web interface at `http://localhost:8000/chat` or use the API endpoints:

```python
import requests

response = requests.post("http://localhost:8000/api/v1/chat", 
    json={
        "query": "What is the average customer rating?",
        "role": "executive"
    }
)
print(response.json())
```

## Contributing

Contributions are welcome! Please read our [Contributing Guidelines](CONTRIBUTING.md) for details on our code of conduct and the process for submitting pull requests.

## References


• [r1-reasoning-rag by deansaco](https://github.com/deansaco/r1-reasoning-rag)

• [RAGLight by deansaco](https://github.com/deansaco/RAGLight)
