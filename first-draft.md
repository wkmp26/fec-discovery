# First Draft of Final Project

## Overview of Data Used

As a recap, for this project, I am working with Federal Election Commission data relating to political committee expenditures in relation to candidates (supporting or opposing). In addition to this data, I used the candidate and committee summary dataset to extract metadata like candidate name/party or committee type.

## Feature Engineering

The main feature I am interested in for this dataset is `TRANSACTION_AMT`, which is the amount of the contribution made in support of or to oppose a certain candidate. From this data, I built an interaction matrix with the aggregate total of contributions from a committee to a candidate during an election cycle. From here, there is a subtle yet significant feature engineering question: how best to normalize these features. In essence, there are two approaches: the amount contributed to a candidate as a percentage of the total committee contributions or the amount a committee contributed to a candidate as a percentage of the total amount the candidate received. Seeing distinct value in both of these, I built two interaction matrices with each approach. Additionally, when I converted these matrices into latent features, I used the standard scaler.  

## Classifiers

One model application I was interested in trying was building increasingly complex classifiers using purely supportive committee contributions.  

### A Tale of Three Models

#### Party Classifer

An interesting, simple classifier I wanted to explore was predicting a candidate political party. I turned this into a binary problem by classifying `Is the candidate a Republican?`

<table>
  <tr>
    <td></td>
    <td>Random</td>
    <td>Random Forest</td>
  </tr>
  <tr>
    <td>Classification Report</td>
    <td><img width="654" height="294" alt="image" src="https://github.com/user-attachments/assets/a1c7f4d7-f687-4413-862a-3b685927c0d6" />
</td>
    <td><img width="660" height="308" alt="image" src="https://github.com/user-attachments/assets/6f89fb9e-f9de-48cb-b764-97379834e4c4" />
</td>
  </tr>
  <tr>
    <td>Confuson Matrix</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/303d3742-dad6-4ba8-937d-e3f8e279a956" />
</td>
    <td><img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/4fbd8771-3360-43d7-9cac-918e64f5890b" />
</td>
  </tr>
  <tr>
    <td>ROC Curve</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/445320f2-8296-429b-b2c0-038dea6122cd" />
</td>
    <td><img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/66578b63-79e0-4422-8ffc-56f1eba91973" />
</td>
  </tr>
  <tr>
    <td>PR Curve</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/7bb75831-fa58-4f31-8ba6-5fab1cee7120" />
</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/7b4c529f-2d77-45ae-b937-893870a1c710" />
</td>
  </tr>
  <tr>
    <td>F1–Threshold</td>
    <td><img width="576" height="453" alt="download" src="https://github.com/user-attachments/assets/ce2d06c6-98c5-481e-b536-fc8f34a4aee3" />
</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/24a32052-ea83-4b07-828b-4e05a0b6991d" />
</td>
  </tr>
</table>


##### Base Line Used

To compare this model, I used a random baseline. This baseline mainly checks to see that some signal is being obtained from the finance data, which it clearly is.


#### Winner Classifer

Increasing complexity and usefulness, I decided to pull FEC data on which candidates actually won their races and attempted to classify this based purely on supportive committee expenditures.

<table>
  <tr>
    <td></td>
    <td>Incumbent Model </td>
    <td>Random Forest Classifer</td>
  </tr>
  <tr>
    <td>Classification Report</td>
    <td><img width="646" height="292" alt="image" src="https://github.com/user-attachments/assets/721dfb33-9633-40b5-a7b8-cfae498f2ab7" />
</td>
    <td><img width="654" height="300" alt="image" src="https://github.com/user-attachments/assets/05dd1c8c-b90e-426d-b6da-eb41f8cc9ff1" />
</td>
  </tr>
  <tr>
    <td>Confusion Matrix</td>
    <td><img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/2166cb60-2a5a-4aa3-85f1-41da3eb090c0" />
</td>
    <td> <img width="507" height="453" alt="download" src="https://github.com/user-attachments/assets/235449d2-f99c-43d2-a8b2-fd760a62ea97" />
</td>
  </tr>
  <tr>
    <td>ROC Curve</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/a309532d-6298-47d7-a017-959f791b3e97" />
</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/3039c185-cabf-4495-9c02-465a91be08dc" />
</td>
  </tr>
  <tr>
    <td>PR Curve</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/a8e85ca3-d25f-4e2a-96ec-a7a7604a2415" />
</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/a65a58b7-e9f0-4699-a1f7-60381106396e" />
</td>
  </tr>
  <tr>
    <td>F1–Threshold</td>
    <td><img width="576" height="453" alt="download" src="https://github.com/user-attachments/assets/5a5beaf5-8adf-45de-99c4-6be25b46d208" />
</td>
    <td><img width="567" height="453" alt="download" src="https://github.com/user-attachments/assets/1a83f682-6bf9-4274-aced-a70d807b75e1" />
</td>
  </tr>
</table>



##### A (Better) Baseline

Noting that my model was showing pretty decent metrics, I decided to make the baseline more competitive and harder to beat. Incumbents tend to win their races, so I built a model that simply said, "If they are an incumbent, then they will win." Surprisingly, this model performed on par with, if not better than, my campaign finance model. Arguably, this baseline has a bit of an advantage given that it has explicit and helpful candidate data, whereas the finance model simply has contribution. However, I was interested in whether the model was able to pick up on this signal, leading me to build another classifier.   

#### Incumbent Classifier

## Clustering

## A Hybrid Approach
