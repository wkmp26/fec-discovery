# FEC Data Analysis
By Weston Patrick

## Data Utilized

This project and analysis utilize Federal Election Commission (FEC) bulk data found here: [FEC Bulk Data](https://www.fec.gov/data/browse-data/?tab=bulk-data).
Specifically, the `Contributions from Committees to Candidates & Independent Expenditures` which, as outlined by the FEC, contains "The itemized committee contributions file contains each contribution or independent expenditure made by a PAC, party committee, candidate committee, or other federal committee to a candidate during the two-year election cycle." This data set held features such as `CMTE_ID`, `CAND_ID`, `TRANSACTION_AMT`,`TRANSACTION_TP`, and `MEMO_TXT`. Additionally, other datasets from the FEC were used to add metadata for candidates and committees based on ID. 

## Data Cleaning and Preparation

The bulk data was stored in a pipe-delimited text file with a separate header file; the file `DataPreparation.ipynb` contains functions that accept the raw data files (including CSV) and convert them into the much more efficient and workable parquet format.

Some cleaning was also required for the data, such as removing duplicate transactions that were amended. Additionally, the filing information was used to add a support or oppose flag when a transaction was tied to a specific candidate.

Finally, as a lead-in to the analysis of this project, this data was transformed into an interaction matrix of the aggregate supportive contributions between a candidate and a committee, where transposing this matrix will flip which value is the index. Finally, this matrix can be normalized by the sum of the row (i.e., the value as a percentage of the total money a candidate received or the total money a committee contributed in a cycle. 

## Alignment Tool (Nearest Neighbors Tools)

A key purpose of this project was to apply machine learn techniques to this dataset to extract meaningful insights about candidates and political committees. Matrix factorization was applied to the interaction matrices created to reduce the large number of columns to a smaller number of latent features. An issue here arises, however, that these latent features are not interpretable in a meaningful way, making analysis difficult. This is where the alignment tool functions come in (`build_persona`,`find_neighbors_cand`,`find_neighbors_comm`). Using the `build_persona` function, a latent feature vector is constructed from a hypothetical candidate in the interaction matrix, using a dictionary of committee 'contribution' or 'alignment score'. For example, a hypothetical candidate vector could be constructed where they are maximally aligned with Chevron's PAC (CMTE_ID: C00035006) and no other committees. This latent vector is then used in conjunction with the other two functions to produce the nearest neighbors for this vector in the latent space, ultimately producing the candidates or committees most similar to the hypothetical.

### Analysis

The key value of the alignment tool is that it makes the latent feature space constructed from the interaction matrix navigable, where a user is able to use a hypothetical latent vector to find patterns and trends based on campaign contributions by committees to candidates. 

One area where this analysis is especially useful is identifying potential conflicts of interest from contribution patterns. For example, if an elected representative serving on the House committee on education and workforce
The House Committee on Small Business is found to be a neighbor of a hypothetical candidate who is exclusively aligned with a retail store, which is a reason for concern and offers a deeper insight than simply looking at that candidate's donors.

## Classifiers

In addition to the alignment tool above, the latent reduced candidate-committee interaction matrix can be used to train a classifier for different candidates (or committee, though this was not explored) traits. The following three classifiers were built, with their results described below.

* insert table here *

*All the models above were built using a Random Forest Classifer

All three of these classifiers are notably highly performant and warrant potentially additional analysis for their result. For example, examining the misclassification in the Republican classifier or digging into the results to understand what features the model is learning to classify incumbents so effectively. 




