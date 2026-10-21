# CRISPR-Cas9 Guide RNA Analysis — Results Report

## 1. Dataset overview
- Guide-candidate table: **314 candidates**, with 19 columns.
- Off-target table supplied: **65,527 records** across 303 guide IDs.
- CRISPOR metadata identifies the genome as **hg38 (GRCh38)** and PAM as **NGG**. Reported target locus: `chr17:7685686-7687546:-`.
- The candidate table has no duplicate guide IDs.
- Guide IDs do not fully align. The off-target file contains ID(s) not present in the guide table: `['WARNING: cannot write more than 65535 rows to an Excel file. Switch to .tsv format to get all off-targets.']`.

## 2. Candidate score summary
- Mean CFD specificity score: **79.9/100**; median **86.0**.
- Mean MIT specificity score: **68.9/100**; median **73.5**.
- **115 of 314** candidates have CFD specificity ≥90.
- **61 of 314** candidates have MIT specificity ≥90.
- Doench 2016 predicted activity score mean: **49.4**; median: **50.0**.

Scores come from different prediction models and are not interchangeable. High specificity alone does not establish experimental activity or safety.

## 3. Example high-scoring candidates
Candidates with CFD specificity ≥90 and Doench 2016 activity ≥50:

| #guideId   | targetSeq               |   mitSpecScore |   cfdSpecScore |   offtargetCount |   Doench '16-Score |
|:-----------|:------------------------|---------------:|---------------:|-----------------:|-------------------:|
| 433rev     | GTACGCTTTAGGCCGGCTCAAGG |             98 |             99 |                7 |                 57 |
| 823forw    | GAGGACCGTCGCAATCTGAGAGG |             97 |             98 |               23 |                 68 |
| 1389forw   | TGAACGTCGTGAAGCGGAAGGGG |             93 |             98 |               27 |                 53 |
| 1110rev    | TAGGTTGCAGGCGACCCGCGGGG |             97 |             97 |               25 |                 68 |
| 141rev     | AGGGAAGCGTGTCACCGTCGTGG |             96 |             97 |               28 |                 64 |
| 1388forw   | CTGAACGTCGTGAAGCGGAAGGG |             97 |             97 |               19 |                 59 |
| 1387forw   | GCTGAACGTCGTGAAGCGGAAGG |             95 |             97 |               26 |                 58 |
| 798rev     | CAGATTGCGACGGTCCTCCTCGG |             91 |             97 |               26 |                 53 |
| 1135forw   | CGGGTCGCCTGCAACCTAGGCGG |             93 |             96 |               30 |                 69 |
| 254forw    | TCAGCTCGGGAAAATCGCTGGGG |             93 |             96 |               32 |                 64 |

By CFD specificity score alone, the top candidate is **433rev** (CFD **99**, MIT **98**, reported off-target count **7**, Doench 2016 activity **57**). This is a computational shortlist, not a validated recommendation.

## 4. Off-target records
- Mismatch distribution: **1,267 records with 3 mismatches** and **64,259 with 4 mismatches**. One record has missing mismatch count.
- Per-guide observed record counts and score summaries are in `guide_candidate_summary.csv`. They describe only the records present in the uploaded file.

## 5. Critical limitation: off-target file appears truncated
The uploaded off-target file is an old Excel `.xls` file and contains **65,536 spreadsheet rows total**, the legacy `.xls` row limit. After its 8 metadata/header rows, the parsed sheet contains **65,527 data rows**, while guide-table `offtargetCount` values sum to **80,232**. This strongly indicates the detailed off-target export was cut off by the spreadsheet row limit.

Therefore:
- A guide with zero rows in the uploaded off-target table must not be assumed to have zero off-target sites without checking the guide-table count.
- Observed detailed off-target counts are incomplete.
- Re-export/download the results as a genuine tab-delimited `.tsv` or text file (not `.xls`) to preserve all rows, then rerun. The guide-level `offtargetCount` column can still be summarized separately.

## 6. Correlations (descriptive only)
- CFD specificity vs MIT specificity: **0.938**.
- CFD specificity vs Doench 2016 activity: **0.137**.
- CFD specificity vs CRISPR-reported off-target count: **-0.483**.

The specificity and activity scores have a weak linear relationship in this dataset, so evaluate multiple metrics rather than relying on one score. Correlation is descriptive, not causal.

## 7. Output files
The `plots/` folder contains five PNG charts. Other files include guide-level and off-target CSV summaries.

## 8. Suggested project conclusion
This analysis compared **314 CRISPOR-predicted guide candidates** for a TP53-region target using specificity and activity scores, reported off-target counts, and supplied off-target records. Candidate `433rev` had CFD specificity 99, MIT specificity 98, and Doench 2016 activity 57, illustrating a high-scoring candidate across these metrics. These are computational predictions, not experimental validation. A major limitation is that the detailed off-target `.xls` export appears truncated at the legacy Excel row limit. A complete TSV export and confirmation of target sequence/reference coordinates are needed before stronger conclusions.
