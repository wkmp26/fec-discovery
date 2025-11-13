# Federal Election Commission Exploratory Data Analysis

## FEC Data

https://www.fec.gov/data/browse-data/?tab=bulk-data

For my final project, I decided to use Federal Election Commission (FEC) data, which tracks election contributions and campaign spending. After exploring how candidates spend and receive money, I decided to focus my project on the bulk data set called `Contributions from Committees to Candidates and Independent Expenditures`, which essentially tracks the spending of different political committees, such as campaign committees or political action committees (PACs). This data seems like a good measure of what larger entities were contributing money to support or oppose a candidate.

In addition to this dataset, I used the Candidate and Committee summary datasets to connect IDs to their metadata.

## Campaign Contributions and Individual Expenditures Structure

Each row in this dataset is in reference to an individual transaction made by a political committee during an election cycle (i.e. 21-22). The row holds useful information such as `CMTE_ID`, `TRANSACTION_TP` (type), `MEMO_TEXT`, and date. Usefully, the rows also held a `CAND_ID` column which noted the candidate the transaction was made in regard to. Additionally, the dataset notes an `OTHER_ID` column, which most of the time lists a candidate's ID or their campaign committee's ID. Something convenient is that these IDs are universal across the FEC data set, making connecting metadata much easier.

This data is relevant for multiple reasons, mainly that examining the spending done toward different candidates can unveil signals both about candidates and the election. Additionally, this data is relevant because it is robust, as the FEC is the legal entity with jurisdiction over campaign finance law, meaning that the political committees in this data set were required to file these expenditures. Finally, this dataset has complete data from the most recent election (2024) but also has 'live' data for the current 2025-26 election cycle.


## Preparing Data

Because the primary data set was from the FEC's 'bulk data' section, it came as a pipe-delimited text document, with a header file included. The candidate and committee summary dataset came as csv. Once parsed, both of these datasets required some preparation, such as adding datatypes. After this preparation was complete, the new datasets were saved as parquet files. 


## Initial Discoveries and Adjusments

With my data set up, I started my exploration by getting to know the features and finding possible early interventions. My first note was that, while most data was associated with a candidate or candidate committee, sometimes this was not the case; thus, I dropped these expenditures. Next, I examined the type of transaction, which was made up of one of 50+ FEC categories; however, most expenditures fell under one of six types, with the vast majority being 24K: 'Contribution made to nonaffiliated committee' with 'nonaffiliated' being in the legal sense, not the political sense. Using the filing type information found [here](https://www.fec.gov/campaign-finance-data/transaction-type-code-descriptions/), I was able to map the expenditure type to a flag of support or opposition for a candidate.

<img width="400" alt="download" src="https://github.com/user-attachments/assets/617e7fa3-0c79-40d7-8572-06e32c673519" />

My next major concern was the `AMDT_IND` (amendment indicator) column, as the FEC notes on their site that an amendment to reports creates the possibility for duplicate columns. I examined these rows and found that the `TRAN_ID`, which is described as "ONLY VALID FOR ELECTRONIC FILINGS. A unique identifier associated with each itemization or transaction appearing in an FEC electronic file. A transaction ID is unique for a specific committee for a specific report. In other words, if committee, C1, files a Q3 New with transaction SA123 and then files three amendments to the Q3 transaction SA123 will be identified by transaction ID SA123 in all four filings." However, some 'A' rows do not have a corresponding 'N'. It is not immediately clear why this is, but it can be assumed based on the FEC quote above that this issue can be mostly remedied by just taking the most recent TRAN_ID in the table.

<img width="400" alt="download" src="https://github.com/user-attachments/assets/d4e3e44c-c5f8-4a39-847e-556baaf3b609" />

## Data Exploration (Visualizations)

Here are some initial visualizations that both describe the data but also start to hint at the types of election insights that can be obtained by this data. Note: all of this data is from the 2021-22 election cycle, though the bulk data contains data from cycles dating back to 1980.

<img width="400" alt="download" src="https://github.com/user-attachments/assets/b0d93f64-7af4-4832-88b5-cc2727fe4681" />
<img width="400" alt="download" src="https://github.com/user-attachments/assets/82de03bb-35ba-480a-8c9b-12d619cba76d" />
<img width="400" alt="download" src="https://github.com/user-attachments/assets/c5b86c05-7c74-4eac-86f8-25cd1e23d5f7" />
<img width="400" alt="download" src="https://github.com/user-attachments/assets/9728bd9e-b0ee-4274-8352-41f3211e2da7" />


## Clustering First Attempt

One application for this data that I am interested in is clustering candidates based on contributions. To start this process, I created an interaction matrix between candidates and the committee (with amounts unnormalized for this initial attempt). I then used truncated SVD to construct latent columns and then graphed these using UMAP. I am still working out which clustering algorithm to use, but for now, there is a clear connection between location on the plot and the type of candidate (Incumbent, Challenger, Open Seat) and political party, but not region (yet).

<img width="400" alt="download" src="https://github.com/user-attachments/assets/130ee006-ed6a-4069-ba42-6f3dfe153952" />
<img width="400" alt="download" src="https://github.com/user-attachments/assets/e31c2489-7d35-442e-9e21-1a6608827995" />
<img width="400" alt="download" src="https://github.com/user-attachments/assets/9851fc0d-6e95-4b0f-a32e-824eed381b23" />


## Initial Ideas and Anticipated Challenges

My initial plans for this data are to cluster based on the interaction matrix and observe what insight can be gained from that process. Expanding on this, I hope to build a classifier for candidate parties based on latent contribution features and see how accurate it is. Something that excites me is that his data exists in discrete cycles, making a 'field' test of a model (i.e., using a model purely trained on previous elections on a more recent election) possible.

There are a few challenges I plan to tackle next. Mainly, handing 'negative' contributions, which are not wholly uncommon in the dataset and represent refunds or cancelled contributions. Further, I need to devise a way to effectively normalize this data. One plan for now is to divide the total amount given to a candidate by a committee by that committee's total election contribution; however, I would take a dual approach to this, as I think there is value to the actual amount each committee contributed to different candidates. Finally, I think that the biggest hurdle to overcome will be understanding the data context to prevent model preparation errors, as the amendment and report type issues are certainly not the last of errors like these.
