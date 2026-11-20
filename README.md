# Antidepressant Adverse Event Analysis

Analyse antidepressant-related adverse events (AEs) from Reddit posts and compare them with FAERS reference data.

## Notebooks

| Notebook | Description |
|----------|-------------|
| [`LLM_processingV2.ipynb`](LLM_processingV2.ipynb) | Clean Reddit JSON, normalise drug names, filter antidepressant posts, and extract structured fields with Gemini |
| [`Antidepressants_analysis_v2.ipynb`](Antidepressants_analysis_v2.ipynb) | Dictionary-based AE detection, co-occurrence analysis, FAERS validation, and longitudinal timelines |

## Pipeline

```text
Raw Reddit JSON
        │
        ▼
LLM_processingV2.ipynb  →  cleaned JSONL + optional LLM extractions
        │
        ▼
Antidepressants_analysis_v2.ipynb  →  AE stats, heatmaps, FAERS comparison, timelines
```

## Setup

```bash
pip install pandas numpy matplotlib seaborn scipy
pip install pydantic emoji tqdm tenacity google-genai   # for LLM notebook
```

Open the notebooks in Jupyter, JupyterLab, or VS Code.

### Gemini API key

Set the key in your environment — never commit it:

```bash
export GEMINI_API_KEY="your_key_here"
```

## Expected data

Data files are **not** included in this repo. Configure local paths as below.

### `LLM_processingV2.ipynb`

```text
./data/
  raw/                              # input Reddit JSON
  json_clean/                       # cleaned outputs
  1.Antidepressants_drug_list.csv
  combined_all.jsonl
  LLM_results/                      # Gemini batch CSVs
```

### `Antidepressants_analysis_v2.ipynb`

Place these CSVs in the notebook working directory (or update the load paths):

- `1.Antidepressants_drug_list.csv`
- `2.adverse_reactions_to_organ_system.csv`
- `3.combined_preprocessed.csv`
- `4.FAERS_data.csv`

## Usage

1. Run `LLM_processingV2.ipynb` top to bottom to clean and (optionally) extract with Gemini.
2. Set `BATCH_TO_RUN` in the extraction cell; re-runs resume from existing batch CSVs.
3. Run `Antidepressants_analysis_v2.ipynb` with the prepared CSVs for analysis and FAERS comparison.

## Notes

- Notebooks are cleared of outputs, absolute paths, and credentials for public sharing.
- LLM extraction uses **Gemini 2.5 Flash Lite** with a structured schema (drugs, AEs, indication, outcome, demographics, and related fields).
- Handle Reddit/FAERS data according to platform terms and your institution’s ethics policies. Do not publish raw posts that contain personal information.
