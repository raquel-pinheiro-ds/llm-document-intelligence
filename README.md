# LLM Document Intelligence

Three tasks over financial documents, each evaluated against public ground truth. Every task compares an API model (Claude) with an open-weight model served locally (Ollama) on quality, cost and latency.

## Tasks

| Task | Data | Ground truth | Metrics |
|------|------|--------------|---------|
| Sentiment classification | Financial PhraseBank | Annotator labels | Macro-F1, cost per 1k items, latency |
| Structured extraction | SEC 10-K filings | SEC Financial Statement Data Sets (XBRL) | Per-field exact match within relative tolerance |
| Retrieval-augmented QA | FinanceBench (150 questions, 10-K PDFs) | Reference answers and evidence passages | Evidence recall@k, answer correctness, faithfulness |

### Sentiment classification

Zero-shot and few-shot prompting of both LLMs, compared with a small encoder fine-tuned on the training split. Stratified train/test split with a fixed seed.

### Structured extraction

Headline figures (e.g. revenue, net income, total assets, diluted EPS) extracted from 10-K text into a JSON schema. Reference values come from the XBRL `num` table, matched on filing accession number, tag, period end date, duration and the absence of segment dimensions.

### Retrieval-augmented QA

PDF parsing, chunking and embedding-based retrieval over FinanceBench filings. Retrieval is scored against the annotated evidence; answers are scored by an LLM judge, with a manually reviewed subsample to measure judge agreement.

## Serving

FastAPI endpoints for each task, packaged with Docker. GitHub Actions runs lint and a small evaluation smoke test.

## Data and licenses

| Dataset | License |
|---------|---------|
| Financial PhraseBank | CC BY-NC-SA 3.0 |
| SEC Financial Statement Data Sets | Public SEC data |
| FinanceBench (open sample) | CC BY-NC 4.0 |

Data is not versioned; `scripts/` contains the download steps.

## Results

Pending.

## Reproduce

```bash
cp .env.example .env
uv sync
```

## Project structure

```
configs/          model, prompt and evaluation settings
notebooks/        exploration and figures
reports/figures/  generated figures
scripts/          data download and evaluation runs
src/llm_document_intelligence/  package code
```
