# Customer Record Entity Resolution — User Manual

Version: 1.0  
Last updated: 2026-01-28

## 1. Overview
This project performs entity resolution on noisy customer records to identify and merge duplicate entries. It applies data normalization, scalable blocking, and fuzzy string matching (Levenshtein, Jaro–Winkler) to detect duplicates, then resolves conflicts and produces a deduplicated customer dataset.

## 2. Prerequisites
- Python 3.8 or later
- pip

Optional but recommended:
- Jupyter Notebook or JupyterLab for interactive execution
- Virtual environment (venv, conda) to isolate dependencies

## 3. Installation
1. Clone or download the repository.
2. Create and activate a virtual environment (recommended):
   - Linux / macOS:
     ```bash
     python -m venv venv
     source venv/bin/activate
     ```
   - Windows (PowerShell):
     ```powershell
     python -m venv venv
     .\venv\Scripts\Activate.ps1
     ```
3. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

## 4. Project Structure
customer-entity-resolution/
│
├── entity_resolution.ipynb        # Main notebook that runs the pipeline
├── data/
│   └── customers.csv              # Input dataset (noisy customer records)
├── merged_customers.csv           # Output: deduplicated and merged records
├── README.md
├── USER_MANUAL.md                 # This document
└── requirements.txt

## 5. Input Dataset
- Location: `data/customers.csv`
- Contents: customer records which may contain variations and inconsistencies in:
  - Names
  - Email addresses
  - Phone numbers
  - Addresses

Ensure the input file exists and is readable before running the notebook.

## 6. Running the Project

Preferred (interactive):
1. Start Jupyter:
   ```bash
   jupyter notebook
   ```
2. Open `entity_resolution.ipynb`.
3. Run cells sequentially from top to bottom.

Headless execution (non-interactive):
```bash
jupyter nbconvert --to notebook --execute entity_resolution.ipynb --output executed_entity_resolution.ipynb
```

The notebook executes the full pipeline:
- Load and normalize data
- Create blocking keys for scalability
- Compute fuzzy similarities (Levenshtein, Jaro–Winkler)
- Detect duplicate pairs
- Resolve conflicts and merge records
- Export final merged dataset

## 7. Algorithmic Details (summary)
- Blocking: Records are grouped using a `block_key` to limit pairwise comparisons and improve performance.
- Similarity metrics:
  - Levenshtein distance (edit distance)
  - Jaro–Winkler similarity
- Duplicate detection: Pairs that exceed a configured similarity threshold are flagged as potential duplicates.
- Conflict resolution: When merging records, the notebook applies deterministic rules to choose the canonical value for each field (e.g., prefer non-empty values, prefer longer/more complete addresses, or prefer verified emails).

## 8. Output
- Primary output file: `merged_customers.csv`
- Notebook prints summary statistics:
  - Total input records
  - Number of duplicate pairs detected
  - Final number of unique customer records

## 9. Configuration
Key parameters (adjust in the notebook):
- `similarity_threshold` (default: 85) — the minimum similarity score (0–100) to consider a pair a duplicate. Lower values increase sensitivity; higher values increase precision.
- `block_key` generation rules — modify blocking strategy to balance recall vs performance.
- Matching weights — if multiple attributes are combined, weights can be adjusted to emphasize certain fields (e.g., email > name > address).

Document any changes to defaults in the notebook so results remain reproducible.

## 10. Notes & Best Practices
- The `block_key` column is internal and used to limit comparisons; do not rely on it as a stable identifier.
- Preprocess and standardize input where possible (normalize phone numbers, lowercase emails, remove punctuation from addresses) to improve matching quality.
- Test different `similarity_threshold` values on a holdout subset to find the best trade-off for your data.
- Keep backups of original data before running merges.

## 11. Troubleshooting
- Notebook fails to run / kernel error:
  - Verify Python version and installed packages in `requirements.txt`.
  - Restart the kernel and re-run cells from the top.
- No output or empty `merged_customers.csv`:
  - Confirm `data/customers.csv` contains records and the correct path is used.
  - Check for exceptions in the notebook output.
- Too many false positives or false negatives:
  - Adjust `similarity_threshold`.
  - Change blocking strategy to include more/less candidates.
  - Tune attribute weights used in combined similarity scoring.

## 12. Extending the Project
- Add additional similarity measures or machine learning models for pair classification.
- Integrate external reference datasets to improve entity resolution (e.g., address validation APIs).
- Parallelize blocking and pair-comparison steps for very large datasets.

## 13. Contact & Support
For questions, issues, or enhancement requests, please open an issue in the project repository or contact the maintainer listed in `README.md`.

---

```
