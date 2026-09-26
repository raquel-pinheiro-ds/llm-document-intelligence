# LLM Document Intelligence

Three tasks over financial documents, each evaluated against public ground truth with open-weight LLMs. The evaluation covers statistical inference with confidence intervals, calibration, bias and hallucination checks, and validation of LLM judges against human labels. Methods, formulas and references are in [docs/methods.md](docs/methods.md).

## Tasks

| Task | Data | Ground truth | Primary metrics |
|------|------|--------------|-----------------|
| Sentiment classification | Financial PhraseBank | Annotator majority labels with agreement levels | Macro-F1, calibration (ECE), label bias |
| Structured extraction | SEC 10-K filings | SEC Financial Statement Data Sets (XBRL) | Per-field accuracy within rounding tolerance, grounding and fabrication rates |
| Retrieval-augmented QA | FinanceBench open sample (150 questions) | Reference answers and evidence pages | Evidence recall@k, answer correctness, faithfulness |

### Sentiment classification

Zero-shot and few-shot prompting of each LLM, compared with a DeBERTa-v3 encoder fine-tuned on the training split. Stratified train, validation and test splits with a fixed seed; prompt and hyperparameter choices are made on validation.

### Structured extraction

Headline figures (revenue, net income, total assets, stockholders' equity, operating cash flow, diluted EPS) extracted from 10-K text into a JSON schema. Reference values come from the XBRL `num` table, matched on accession number, tag, period end date, duration, unit and the absence of segment dimensions.

### Retrieval-augmented QA

PDF parsing, chunking and BM25, dense and hybrid retrieval over FinanceBench filings. Retrieval is scored against the annotated evidence pages. Answers are scored by an LLM judge and Ragas; a human-labeled sample measures judge agreement and corrects judge-based estimates with prediction-powered inference.

## Models and inference

LLMs run on a Google Colab T4 GPU through llama.cpp `llama-server`, with GGUF Q4_K_M weights for every model, temperature 0 and JSON-schema constrained decoding. The generators and the judge come from different model families. Each run writes a manifest with the model revision, llama.cpp commit, server flags and a hash of the request file.

Requests are prepared locally and exchanged with Colab as JSONL files; scoring and figures run locally.

## Serving

A FastAPI service exposes `/health` and `/sentiment` (fine-tuned encoder on CPU), packaged with Docker. GitHub Actions runs lint, an evaluation smoke test and the Docker build.

## Data and licenses

| Dataset | License |
|---------|---------|
| Financial PhraseBank | CC BY-NC-SA 3.0 |
| SEC Financial Statement Data Sets, EDGAR filings | Public SEC data |
| FinanceBench (open sample) | CC BY-NC 4.0 (dataset card); CC BY-NC-ND 4.0 (paper) |

Data and fine-tuned weights are not versioned; `scripts/` contains the download steps.

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
docs/             methods, formulas and references
notebooks/        Colab generation notebook
reports/figures/  generated figures
scripts/          data preparation, generation, evaluation
src/llm_document_intelligence/  package code
```
