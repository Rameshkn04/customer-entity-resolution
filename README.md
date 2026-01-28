## Customer Record Entity Resolution System

### Tech Stack
- Python
- pandas
- RapidFuzz

### Overview
This project implements a scalable customer entity resolution pipeline to identify and merge duplicate customer records from noisy datasets using fuzzy matching techniques.

### Features
- Blocking strategy to reduce pairwise comparisons
- Fuzzy matching using Levenshtein and Jaro-Winkler similarity
- Deterministic conflict resolution and record merging

### Matching Strategy
- Weighted similarity scoring with threshold = 85
- Matching on name, email, phone, and address

### Output
- Clean merged customer dataset
- Summary statistics of duplicate detection and merges
