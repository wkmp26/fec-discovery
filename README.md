# FEC Data Analysis
By Weston Patrick

## Data Utilized

This project and analysis utilize Federal Election Commission (FEC) bulk data found here: [FEC Bulk Data](https://www.fec.gov/data/browse-data/?tab=bulk-data).
Specifically, the `Contributions from Committees to Candidates & Independent Expenditures` which, as outlined by the FEC, contains "The itemized committee contributions file contains each contribution or independent expenditure made by a PAC, party committee, candidate committee, or other federal committee to a candidate during the two-year election cycle." This dataset includes features such as `CMTE_ID`, `CAND_ID`, `TRANSACTION_AMT`,`TRANSACTION_TP`, and `MEMO_TXT`. Additionally, other datasets from the FEC were used to add metadata for candidates and committees based on ID. 

## Data Cleaning and Preparation

The bulk data was stored in a pipe-delimited text file with a separate header file; the file `data-preparation.ipynb` contains functions that accept the raw data files (including CSV) and convert them into the much more efficient and workable parquet format.

Some cleaning was also required for the data, such as removing duplicate transactions that were amended. Additionally, the filing information was used to add a support or oppose flag when a transaction was tied to a specific candidate.

Finally, as a lead-in to the analysis of this project, this data was transformed into an interaction matrix of the aggregate supportive contributions between a candidate and a committee, where transposing this matrix will flip which value is the index. Finally, this matrix can be normalized by the sum of the row (i.e., the value as a percentage of the total money a candidate received or the total money a committee contributed in a cycle). 

## Alignment Tool (Nearest Neighbors Tools)

A key purpose of this project was to apply machine learning techniques to this dataset to extract meaningful insights about candidates and political committees. Matrix factorization was applied to the interaction matrices created to reduce the large number of columns to a smaller number of latent features. An issue here arises, however: these latent features are not interpretable in a meaningful way, making analysis difficult. This is where the alignment tool functions come in (`build_persona`,`find_neighbors_cand`,`find_neighbors_comm`), which are housed in the `alignment-tool.ipynb` file. Using the `build_persona` function, a latent feature vector is constructed from a hypothetical candidate in the interaction matrix, using a dictionary of committee 'contributions' or 'alignment scores'. For example, a hypothetical candidate vector could be constructed where they are maximally aligned with Chevron's PAC (CMTE_ID: C00035006) and no other committees. This latent vector is then used in conjunction with the other two `find_neighbors` functions to produce the nearest neighbors for this vector in the latent space, ultimately producing the candidates or committees most similar to the hypothetical.

### Analysis

The key value of the alignment tool is that it makes the latent feature space constructed from the interaction matrix navigable, where a user is able to use a hypothetical latent vector to find patterns and trends based on campaign contributions by committees to candidates. 

One area where this analysis is especially useful is identifying potential conflicts of interest from contribution patterns. For example, if an elected representative serving on the House committee on education and workforce or
the House committee on small business is found to be a neighbor of a hypothetical candidate who is exclusively aligned with large retail store PACs, this would raise concerns and offers a deeper insight than simply looking at that candidate's donors.

## Classifiers

In addition to the alignment tool above, the latent reduced candidate-committee interaction matrix can be used to train a classifier for different candidate (or committee, though this was not explored) traits. The following three classifiers were built, with their results described below.

<table>
  <thead>
    <tr>
      <th></th>
      <th>Is Republican?</th>
      <th>Won Election?</th>
      <th>Is Incumbent?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Classification Report</th>
      <td><img width="304" height="157" alt="image" src="https://github.com/user-attachments/assets/306be5fc-c998-4ac7-a3da-3e8ae25be1f5" />
</td>
      <td><img width="299" height="158" alt="image" src="https://github.com/user-attachments/assets/d242ffbe-d82e-4154-9abf-2161b0846cb5" />
</td>
      <td><img width="301" height="159" alt="image" src="https://github.com/user-attachments/assets/27e5aacc-26f0-4d84-ba09-f4305f519382" />
</td>
    </tr>
    <tr>
      <th>Confusion Matrix</th>
      <td><img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/e49aeffd-c478-4887-91ce-87c0d9bff0a2" />
</td>
      <td><img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/5ce00e1b-d8ec-47db-9880-cbbe0368bc67" />
</td>
      <td><img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/958883cc-b7f3-4f79-b214-98b8616c6225" />
</td>
    </tr>
    <tr>
      <th>ROC Curve</th>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/f3d22005-1561-471e-9be3-46ae0bf15f25" />
</td>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/e68b126d-cd96-43d6-9652-77ed6e503135" />
</td>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/0b86faf8-7574-4c5d-9675-bb98a3270062" />
</td>
    </tr>
    <tr>
      <th>Precision-Recall Curve</th>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/e5ac1e51-7039-4941-b1eb-28435c1e9e64" />
</td>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/49a57a5f-b5aa-41e0-88b0-6a9ca0164b15" />
</td>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/af867398-1084-49df-8030-9259508f145e" />
</td>
    </tr>
    <tr>
      <th>F1 Vs. Threshold</th>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/ca4df088-fa7d-4fac-bf5a-551227289920" />
</td>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/54b8289a-821b-4e10-b55a-39c3c9718f6d" />
</td>
      <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/3429c5f5-82a6-45df-b531-a8fa53dc4f56" />
</td>
    </tr>
  </tbody>
</table>


*All the models above were built using a Random Forest Classifier. The code for these models is in the `eda-init-models.ipynb` file.

All three of these classifiers are notably highly performant and warrant further analysis of their results. For example, examining misclassifications in the Republican classifier or digging into the results to understand which features the model is learning to classify incumbents so effectively. Further, additional examination could reveal if the classifier is learning incumbency to predict the election result or if it is detecting signal elsewhere. 




