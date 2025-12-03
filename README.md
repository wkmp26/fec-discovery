# FEC Data Analysis
By Weston Patrick

## Data Utilized

This project and analysis utilize Federal Election Commission (FEC) bulk data found here: [FEC Bulk Data](https://www.fec.gov/data/browse-data/?tab=bulk-data).
Specifically, the `Contributions from Committees to Candidates & Independent Expenditures` which, as outlined by the FEC, contains "The itemized committee contributions file contains each contribution or independent expenditure made by a PAC, party committee, candidate committee, or other federal committee to a candidate during the two-year election cycle." This data set held features such as `CMTE_ID`, `CAND_ID`, `TRANSACTION_AMT`,`TRANSACTION_TP`, and `MEMO_TXT`. Additionally, other datasets from the FEC were used to add metadata for candidates and committees based on ID. 

## Data Cleaning and Preparation

The bulk data was stored in a pipe-delimited text file with a separate header file; the file `DataPreparation.ipynb` contains functions that accept the raw data files (including CSV) and convert them into the much more efficient and workable parquet format.

Some cleaning was also required for the data, such as removing duplicate transactions that were amended. Additionally, the filing information was used to add a support or oppose flag when a transaction was tied to a specific candidate.

Finally, as a lead-in to the analysis of this project, this data was transformed into an interaction matrix of the aggregate supportive contributions between a candidate and a committee, where transposing this matrix will flip which value is the index. Finally, this matrix can be normalized by the sum of the row (i.e., the value as a percentage of the total money a candidate received or the total money a committee contributed in a cycle. 

## Alignment (Nearest Neighbors Tools)

## Classifiers
