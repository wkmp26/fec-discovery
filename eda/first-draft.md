# First Draft of Final Project

## Overview of Data Used

As a recap, for this project, I am working with Federal Election Commission data relating to political committee expenditures in relation to candidates (supporting or opposing). In addition to this data, I used the candidate and committee summary datasets to extract metadata like candidate name/party or committee type.

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
    <td>Random Model</td>
    <td>Random Forest</td>
  </tr>
  <tr>
    <td>Classification Report</td>
    <td><img width="200"  alt="image" src="https://github.com/user-attachments/assets/a1c7f4d7-f687-4413-862a-3b685927c0d6" />
</td>
    <td><img width="200" alt="image" src="https://github.com/user-attachments/assets/6f89fb9e-f9de-48cb-b764-97379834e4c4" />
</td>
  </tr>
  <tr>
    <td>Confusion Matrix</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/90334946-3ad7-4fb4-8cab-a9ef47b73549" />

</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/d244afb5-b290-4da3-b182-7089225f6453" />

</td>
  </tr>
  <tr>
    <td>ROC Curve</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/b8a24705-0f3b-4b07-a02d-9f7f59b50507" />

</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/01471d2d-bc4f-4cae-958d-2531e14ef0d5" />

</td>
  </tr>
  <tr>
    <td>PR Curve</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/ad942b15-3b57-4b0d-85b8-0eda56c32056" />

</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/3279cae9-56e8-47dc-853b-983a644f1862" />

</td>
  </tr>
  <tr>
    <td>F1–Threshold</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/7b2efffc-5c6d-4ae6-8074-1024364706f1" />

</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/19c80018-d6c8-4dc2-8440-96b0c0347ed9" />

</td>
  </tr>
</table>

These metrics show that overall party classification is achievable, but there is still some muddiness to the accuracy. An ROC-AUC of 0.95 and the 0.9 F1 score indicate a decent model; however, the model appears to be better at classifying False values over True ones. One area of investigation here would be if there are 'fenceriding' committees that donate to both major parties in a race.

##### Base Line Used

To compare this model, I used a random baseline. This baseline mainly checks to see that some signal is being obtained from the finance data, which it clearly is.


#### Winner Classifer

Increasing complexity and usefulness, I decided to pull FEC data on which candidates actually won their races and attempted to classify this based purely on supportive committee expenditures.

<table>
  <tr>
    <td></td>
    <td>Incumbent Model</td>
    <td>Random Forest Classifer</td>
  </tr>
  <tr>
    <td>Classification Report</td>
    <td><img width="200" alt="image" src="https://github.com/user-attachments/assets/721dfb33-9633-40b5-a7b8-cfae498f2ab7" />
</td>
    <td><img width="200" alt="image" src="https://github.com/user-attachments/assets/05dd1c8c-b90e-426d-b6da-eb41f8cc9ff1" />
</td>
  </tr>
  <tr>
    <td>Confusion Matrix</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/2166cb60-2a5a-4aa3-85f1-41da3eb090c0" />
</td>
    <td> <img width="200" alt="download" src="https://github.com/user-attachments/assets/235449d2-f99c-43d2-a8b2-fd760a62ea97" />
</td>
  </tr>
  <tr>
    <td>ROC Curve</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/a309532d-6298-47d7-a017-959f791b3e97" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/3039c185-cabf-4495-9c02-465a91be08dc" />
</td>
  </tr>
  <tr>
    <td>PR Curve</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/a8e85ca3-d25f-4e2a-96ec-a7a7604a2415" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/a65a58b7-e9f0-4699-a1f7-60381106396e" />
</td>
  </tr>
  <tr>
    <td>F1–Threshold</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/5a5beaf5-8adf-45de-99c4-6be25b46d208" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/1a83f682-6bf9-4274-aced-a70d807b75e1" />
</td>
  </tr>
</table>



##### A (Better) Baseline

Noting that my model was showing pretty decent metrics, I decided to make the baseline more competitive and harder to beat. Incumbents tend to win their races, so I built a model that simply said, "If they are an incumbent, then they will win." Surprisingly, this model performed on par with, if not better than, my campaign finance model. Arguably, this baseline has a bit of an advantage given that it has explicit and helpful candidate data, whereas the finance model simply has contributions. However, I was interested in whether the model was able to pick up on this signal, leading me to build another classifier.   

#### Incumbent Classifier

<table>
  <tr>
    <td></td>
    <td>Random Model</td>
    <td>Random Forest</td>
  </tr>
  <tr>
    <td>Classification Report</td>
    <td> <img width="200" alt="image" src="https://github.com/user-attachments/assets/ec566527-1f2c-4760-873e-8b1cdc361230" />

</td>
    <td><img width="200" alt="image" src="https://github.com/user-attachments/assets/277e8b4d-eea7-4065-9ec0-a6d1fb28a17b" />
</td>
  </tr>
  <tr>
    <td>Confusion Matrix</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/181562c5-0be7-435f-8f2d-5c98c95ae107" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/6a592ea6-63dc-4c24-824e-5e61b4e7aee9" />
</td>
  </tr>
  <tr>
    <td>ROC Curve</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/f17daea6-e9d2-417c-b35b-2048f867df54" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/26383a9a-2fad-47d7-93c9-b5b4a92f0bd3" />
</td>
  </tr>
  <tr>
    <td>PR Curve</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/51b2ae18-3843-49e6-a4ed-dd0eceb6df6a" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/aebaa522-2ce6-4804-b5a4-bc14e2d2ebfb" />
</td>
  </tr>
  <tr>
    <td>F1–Threshold</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/507cb38b-c373-4d3c-a155-e71442ac248d" />
</td>
    <td><img width="200" alt="download" src="https://github.com/user-attachments/assets/f385d266-edbd-46e5-8142-3dcd2946215f" />
</td>
  </tr>
</table>

##### Random Binomial Baseline 

For my baseline, given the uneven distribution of incumbents, I randomly sampled the predictions using a Bernoulli distribution, making this baseline slightly more robust.

##### Incumbent Conclusion

As can be seen, the incumbent classifier based purely on committee expenditures performs impressively well, with a 0.98 ROC-AUC, an f1 of 0.94, and an overall test accuracy of 93%. This is significantly better than the results of a random binomial baseline. From here, we can see that the winner model was likely just figuring out that incumbents win and then building a form of this model. The interesting finding here is that this is purely from contributions, so diving into these models and making them more interpretable to find the indicators of incubancy could reveal some important insights for campaign finance data.

## Error and Bias

While much of my EDA and data cleaning prepped the data for errors, there could be other nuances in filing strategies or other missing pieces of context. Another source of error could occur as I work more with latent features; the models become less interpretable, and back-tracing decisions can be difficult. For example, there was a moment where I realized that I forgot to filter by support, mixing a candidate's best supporters with their biggest opponents, confusing my results. One foreseeable area of bias is that candidates who receive more and committees that spend more are more represented in this transaction-based dataset.

## Clustering

An initial area I wanted to explore with this data is clustering; however, this is proving difficult. When I initially latent reduced the dimensions and plotted by various features, some clusters seemed present; however, after multiple attempts, I have still been unable to form remotely distributed clusters. Findings from this area lead me to the hybrid model described below.

### Clustering by Committee 

<table>
  <tr>
    <td>Reduction to n=6</td>
    <td>Coloration by Commitee Type</td>
    <td>Clusteriny By HDBScan (Best Resut to Date</td>
  </tr>
  <tr>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/caa9c0ca-410f-4958-a150-265a8d4d4b5c" />
</td>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/b594ca35-7485-4941-baf0-f805f70a24d1" />
</td>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/c731e2c5-6f06-4cf7-8b37-4423c2a7b4ce" />
</td>
  </tr>
</table>


### Clustering by Candidate

<table>
  <tr>
    <td>Reduction to n=20</td>
    <td>Coloration by Candidate Type (i.e, Incumbent, Challenger, etc.)</td>
    <td>Coloration by Candidate Party</td>
    <td>Coloration by Candidate Region</td>
    <td> Clusterining By KMeans (k=8) [Similar result for most approaches] </td>
  </tr>
  <tr>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/b22a5c66-b9a9-4836-bead-aea1d1d8daf6" />

</td>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/6ac359e7-3a4d-4da9-9a60-fa1967f24384" />

</td>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/c38f187d-63de-4ba5-af93-f1be1d497b1a" />

</td>
    <td><img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/e7fd17fe-b5f7-4603-84c5-64b2fc833a6b" />

</td>
<td>
    <img width="689" height="525" alt="download" src="https://github.com/user-attachments/assets/ba7b694b-8946-4d0b-8fc0-e42f24a7fcd8" />
</td>
  </tr>
</table>


## A Hybrid Approach

These efforts have allowed me to get to know this dataset and ultimately understand what it is good for. Ultimately, I was pursuing the goal of using this data to create a semi-novel and interesting insight. Given everything I learned and the questions I still have about money in politics, I decided on the approach below.

### Back to Neighbors

As mentioned above, I created two interaction matrices, one where candidates were the index and the other where committees were the index. The values correspond to either the amount of total contribution that the committee made up for a candidate, or what percentage of its total expenditures were spent on that one candidate. I want to produce an interpretable way to relate candidates and committees to each other based on this data. Thus, I plan to reduce these sparse matrices to latent features using truncated SVD and, from there, use K-nearest neighbors to find what candidates/committees are related to each other. Where I take this a step further is that I plan to develop a function where 'fake' committees and candidates can be made based on the values that would be found in their hypothetical interaction matrix row. For example, a candidate 100% bankrolled by Walmart or a committee that only supports Representative Alexandria Ocasio-Cortez. With these 'dummy' neighbors, I can see what actual candidates/committees are close to them in my feature space, communicating a very interesting and unique insight about those candidates/committees. 

*Note: This approach could also be used to partially identify the meaning of latent features

### Planned Approach

-- Build Committee/Candidate Interaction Matrices [Normalized By Percent]
-- Use Truncated SVD to reduce matrices down to latent features, then scale
-- Fit a KNN model to both matrices
-- Develop a function that accepts as input a funding distribution (i.e., 25% from BP Oil, 75% from the NRA) and returns a Committee/Candidate latent vector
-- Use this vector to produce the candidate/committees' nearest neighbors
-- *The insight you then gain is about the neighbors returned


### Initial Approach

Here are some initial results from my first attempt. For now, I am using KNN on real candidates (in this case, I used Nancy Mace) and comparing their top committee donors. As I develop my 'dummy' neighbor function, I will continue to test like this.

#### Neighbors

<img width="300" alt="image" src="https://github.com/user-attachments/assets/8b7094a7-29b6-4933-afa3-ad4ab2d2dbe0" />

#### Top Donors of Neighbors


<img width="300" alt="image" src="https://github.com/user-attachments/assets/9ef8552d-67d5-416a-9405-21ba2efe6d0a" />


## Next Steps and Ideas For Final Report

I plan to continue on this approach and ultimately produce simple, readable, and usable code for a user to gather these insights. I will then **concisely** document my approach and the use of my code in my final README report.
