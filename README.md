# Customer Record Entity Resolution

Version: 1.0  
Last updated: 2026-01-28

## Overview
This repository contains a scalable entity resolution pipeline that identifies and merges duplicate customer records in noisy datasets. It combines data normalization, blocking for scalability, and fuzzy matching (Levenshtein & Jaro–Winkler via RapidFuzz) with deterministic conflict-resolution rules to produce a cleaned, deduplicated customer dataset.

## Tech Stack
- Python (3.8+)
- pandas
- RapidFuzz
- Jupyter Notebook (for interactive execution)

## Features
- Blocking strategy to drastically reduce pairwise comparisons and improve performance on larger datasets
- Fuzzy string matching using both Levenshtein and Jaro–Winkler similarity measures
- Weighted similarity scoring across multiple attributes (name, email, phone, address)
- Deterministic conflict resolution and record merging rules
- Summary statistics and export of deduplicated output

## Repository Structure
customer-entity-resolution/
- entity_resolution.ipynb        — Main notebook that runs the pipeline  
- data/customers.csv            — Input dataset (noisy customer records)  
- merged_customers.csv          — Output file with merged records (generated)  
- README.md                     — This document  
- USER_MANUAL.md                — Detailed usage & notes  
- requirements.txt              — Python dependencies

## Input / Output
- Input: `data/customers.csv` — CSV of raw customer records (name, email, phone, address, etc.).
- Output: `merged_customers.csv` — Deduplicated, merged customer records produced by the notebook.

## Matching Strategy
- Attributes used: name, email, phone, address.
- Similarity metrics: Levenshtein distance and Jaro–Winkler similarity (via RapidFuzz).
- Scoring: Weighted attribute-level scores are combined into an overall similarity score.
- Default threshold: 85 (0–100 scale). Pairs scoring >= threshold are considered duplicates. Adjust to control precision/recall trade-offs.

## Key Concepts
- Blocking: Records are assigned a `block_key` to restrict comparisons to likely candidates and improve runtime. The `block_key` column is internal — do not use it as a stable identifier.
- Conflict resolution: When multiple records merge, deterministic rules resolve field conflicts (e.g., prefer non-empty values, prefer verified emails, prefer longer/more complete addresses). Modify these rules in the notebook as needed for your business logic.

## Installation
1. (Recommended) Create and activate a virtual environment:
   - macOS / Linux:
     ```bash
     python -m venv venv
     source venv/bin/activate
     ```
   - Windows (PowerShell):
     ```powershell
     python -m venv venv
     .\venv\Scripts\Activate.ps1
     ```
2. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## Running
Interactive (recommended)
1. Start Jupyter:
   ```bash
   jupyter notebook
   ```
2. Open `entity_resolution.ipynb` and run all cells sequentially.

Headless (non-interactive)
```bash
jupyter nbconvert --to notebook --execute entity_resolution.ipynb --output executed_entity_resolution.ipynb
```

After execution, check `merged_customers.csv` for the merged output and the notebook output for summary statistics:
- Total input records
- Duplicate pairs detected
- Final unique customer records

## Configuration & Tuning
Adjustable parameters are defined in the notebook:
- `similarity_threshold` (default: 85) — lower values increase recall; higher values increase precision.
- Blocking strategy / `block_key` generation — tune to balance performance and recall.
- Attribute weights — adjust relative importance for name, email, phone, and address.
- Preprocessing steps — normalize phone numbers, lowercase emails, strip punctuation from addresses to improve matching.

Tip: Evaluate thresholds and weights on a labeled holdout set when possible.

## Performance & Scaling
- Blocking reduces the number of pairwise comparisons and is critical for scalability.
- For very large datasets, consider:
  - More restrictive blocking strategies with multiple passes (multi-pass blocking)
  - Parallelizing block processing
  - Using an approximate nearest-neighbor approach for candidate selection

## Troubleshooting
- Notebook fails or kernel errors: verify Python & dependencies, restart kernel, and run cells from the top.
- Empty or missing `merged_customers.csv`: ensure `data/customers.csv` exists and contains data; inspect notebook output for exceptions.
- Too many false positives/negatives: adjust `similarity_threshold`, change blocking strategy, or re-weight attributes.

## Extending This Project
- Add additional similarity measures or a supervised pair-classifier to improve accuracy.
- Integrate external verification services (e.g., address validation APIs).
- Convert the notebook pipeline into a modular script or package for production use.
- Add unit tests and CI to ensure reproducibility.

## Contributing
Contributions, issues, and feature requests are welcome. Please open an issue or submit a pull request with a clear description and, when appropriate, test data or examples.

## License & Support
See the repository LICENSE file (if present) for license information. For support or questions, open an issue in the repository or contact the project maintainer listed in `README.md`.

---
