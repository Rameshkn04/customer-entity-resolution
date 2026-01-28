# User Manual – Customer Record Entity Resolution System

## 1. Prerequisites
Ensure you have the following installed:
- Python 3.8 or above
- pip

Install dependencies using:
```bash
pip install -r requirements.txt

2. Project Structure
customer-entity-resolution/
│
├── entity_resolution.ipynb
├── data/
│   └── customers.csv
├── merged_customers.csv
├── README.md
├── USER_MANUAL.md
└── requirements.txt

3. Input Dataset

The input customer dataset is located at:

data/customers.csv


The dataset contains noisy customer records with variations in names, emails, phone numbers, and addresses.

4. Running the Project

Open the notebook:

entity_resolution.ipynb


Run all cells sequentially from top to bottom.

The notebook performs:

Data loading and normalization

Blocking for scalability

Fuzzy matching using Levenshtein and Jaro-Winkler similarity

Duplicate detection

Conflict resolution and record merging

5. Output

The final merged dataset is generated as:

merged_customers.csv


The notebook also prints summary statistics, including:

Total input records

Duplicate pairs detected

Final unique customer records

6. Notes

The block_key column is used internally for blocking and scalability.

The similarity threshold is set to 85 and can be adjusted if required.

