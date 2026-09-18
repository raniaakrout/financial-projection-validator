# FallahTech Financial RAG System

A Retrieval-Augmented Generation (RAG) pipeline designed to validate financial projections against historical data for investment due diligence.

## Overview

This system analyzes FallahTech's business plan projections (2025-2029) by cross-referencing them with certified historical financial statements (2023-2025). It provides automated assessment of financial hypotheses as Conservative, Realistic, Optimistic, or Critical Conflict.

## Key Features

- **Dual-source RAG**: Separate vector stores for historical data and projections
- **Multi-format parsing**: Handles PDF financial statements and Excel business plans
- **Intelligent chunking**: Preserves table integrity with context overlap
- **LLM-powered reranking**: Ensures relevant historical context for each query
- **Structured output**: Generates comparison tables with confidence assessments

## Project Structure

```
FallahTech_RAG_Pipline/
├── Rag/
│   ├── FallahTech_RAG_T2_versionFINAL.ipynb  # Main RAG pipeline
│   └── doc_rag.pdf                            # Technical documentation
├── data/
│   ├── Etats_Financiers_Historiques_NCT.pdf   # Historical financials (2023-2025)
│   ├── FallahTech_BusinessPlan_Complet.xlsx   # Projections (2025-2029)
│   └── DataRoom_FallahTech_Professionnelle/   # Supporting documents
│       └── DataRoom_FallahTech_PDF/
│           ├── 1_Juridique/                   # Legal documents
│           ├── 2_Financier/                   # Financial statements
│           ├── 3_Operationnel/                # HR registry
│           └── 4_Commercial/                  # Market research
└── n8n/
    ├── FallahTech T2 .json                    # n8n workflow export
    ├── Livrable Sujet B (T2).pdf              # Workflow documentation
    └── parsing (n8n).ipynb                    # n8n parsing notebook

```

## Technical Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| PDF Parser | PyMuPDF (rawdict mode) | Handles CID-encoded fonts |
| Excel Parser | pandas | Reads structured projections |
| Embeddings | BAAI/bge-m3 (1024-dim) | Multilingual FR/EN/AR support |
| Vector Store | ChromaDB | Dual collections (historical/projections) |
| LLM | Groq LLaMA 3.3 70B | Deterministic generation (temp=0) |
| Orchestration | n8n | Workflow automation (optional) |

## RAG Pipeline Flow

```
User Query
    ↓
Retrieve (k=4 historical + k=4 projections)
    ↓
LLM Reranking (filter irrelevant chunks)
    ↓
Guarantee ≥2 historical chunks
    ↓
Generate structured comparison
    ↓
Output: Markdown table + sources
```

## Usage

Open and run `Rag/FallahTech_RAG_T2_versionFINAL.ipynb` to:
1. Parse financial documents
2. Build vector indices
3. Query the RAG system
4. Generate validation reports

**Example Query:**
```
"What is the revenue growth rate projected for 2026-2027?"
```

**Output Format:**
| Hypothesis | Projected Value | Historical Value | Gap | Assessment |
|------------|----------------|------------------|-----|------------|
| Revenue growth 2026-2027 | +45% | +111.5% (2024-2025) | -66.5pp | Conservative |

## Data Sources

- **Historical Reference**: Certified NCT financial statements (2023-2025)
- **Projections**: FallahTech Business Plan (2025-2029)
- **Supporting**: Legal, HR, and market research documents


## Documentation

See `Rag/doc_rag.pdf` for detailed technical specifications and `n8n/Livrable Sujet B (T2).pdf` for workflow documentation.
