# FUNNEL: A Three-Stage Agentic RAG Architecture for Clinical Documentation

This repository contains the core implementation of the **FUNNEL** pipeline, a specialized Retrieval-Augmented Generation (RAG) architecture designed for intelligent longitudinal reasoning within Electronic Medical Records (EMRs). 

This codebase is provided as supplementary material for reproducibility in support of our MethodsX manuscript.

## Overview
FUNNEL addresses the challenge of retrieving and synthesizing information from dense, years-long clinical timelines through a three-stage process:

1.  **Stage 1: Multi-Modal Retrieval**: Combines vector similarity (Google `embedding-001`), keyword search (BM25), and temporal weighting to retrieve the top 50 candidates.
2.  **Stage 2: LLM-Based Reranking**: Uses `gemini-1.5-flash` to score and refine the candidates down to the top 10 most semantically relevant documents.
3.  **Stage 3: Evidence-Grounded Synthesis**: Leverages `gemini-1.5-pro` to generate a clinical answer with mandatory inline citations and automated conflict detection.

## Repository Structure
- `stage1_retrieval.py`: Implementation of the unified scoring formula and temporal decay logic.
- `stage2_reranking.py`: Relevance scoring logic using Gemini Flash.
- `stage3_synthesis.py`: Cited answer generation and contradiction detection.
- `config.py`: Default weights, decay rates, and threshold parameters.
- `prompts.py`: The exact prompt templates used for reranking and synthesis.
- `demo.py`: End-to-end demonstration script.

## Setup Instructions

### 1. Requirements
- Python 3.9+
- A Google Gemini API Key

### 2. Installation
```bash
pip install -r requirements.txt
```

### 3. Configuration
Export your API key as an environment variable:
```bash
export GOOGLE_API_KEY='your-api-key-here'
```

## Usage
To run the end-to-end reproduction script:
```bash
python -m repro.demo
```

## Input Document Format
The pipeline expects document objects (clinical events) in the following JSON-like format:
```json
{
  "id": "event_uuid",
  "type": "SOAP Notes | Lab Results | Imaging Reports",
  "timestamp": "YYYY-MM-DD",
  "text": "Full document content...",
  "embedding": [0.1, 0.2, ...] // 768-dim vector
}
```

## License
MIT License. See `LICENSE.txt` for details.

