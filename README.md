# Predicting Individual Level Voter Turnout From North Carolina Vote File

### Problem Statement

Using the North Carolina voter file (voter history and demographics), can we use data from 2010 - 2014 to predict whether one million randomly selected voters voted in the 2016 general election?

---

### Project Notes

This repo does NOT include all of the datasets needed to run the process outlined in the Jupyter notebooks.  Due to the large size of the data files, anyone reading this will need to read the notes in 1_load_data.ipynb to locate data and create an environment that is capable of running the processes.  It is not advised to run these scripts on a local machine.

---

### Executive Summary

Like many states, North Carolina puts their voter file online, publicly available.  This voter file can be used by political campaigns to understand the electorate, reach voters, and understand their performance.  Campaigns, political parties, and other groups (PACs, committees, etc.) use this data to create models and plan their campaign strategy.

The voter file contains the turnout history (which elections) each active voter participated in from 2010 - 2018.  Because it only contains active voters, it does not include all of the voters that participated in each election, only those that are still registered in NC.  This is a potential source of bias within our data.

Using the voter file, we ran a variety of classification models to compare their performance.  Additionally, we experimented with adding in the congressional district level donation data to the model as a feature.  However, the donations and number of donors, by party, did not end up being a strong predictor of whether or not individuals would vote.

Below is the performance for the models that were run.  The last two are the ones that we added in the donations to.  To evaluate our model, we are looking to maximize sensitivity.  We are interested in minimizing false negatives, because we would rather predict that someone was going to vote and have them not turn-out, than the opposite.  Because this type of model would be used by campaigns to identify likely voters, we would rather have some voters who are not going to vote in our "voting" class, then have some voters who are going to vote marked as "not voters".  If we mark registered individuals as non-voters, than they likely will be ignored by the campaign.  The model with the highest sensitivity score among the test (2016) dataset was the Naive Bayes model.  It has a sensitivity score of .58.  As you can see, the models have a higher specificity score for all of the models, indicating that they are at optimizing for false positives than false negatives.

---
| Model               | set   | pred pos   | pred neg   | tn              | fp                | fn                | tp              | spec     | sens     | acc      | total records |
|---------------------|-------|------------|------------|-----------------|-------------------|-------------------|-----------------|----------|----------|----------|---------------|
| Logistic Regression | train |   361,782  |   638,218  |        527,024  |           82,677  |         111,194   |        279,105  | 0.864397 | 0.715106 | 0.806129 | 1000000       |
|                     | test  |   389,649  |   610,351  |        334,690  |           31,702  |         275,661   |        357,947  | 0.913475 | 0.564934 | 0.692637 | 1000000       |
| Naïve Bayes         | train |   459,978  |   540,022  |        438,833  |         170,868   |         101,189   |        289,110  | 0.719751 | 0.74074  | 0.727943 | 1000000       |
|                     | test  |   467,991  |   532,009  |        268,944  |           97,448  |         263,065   |        370,543  | 0.734033 | 0.584814 | 0.639487 | 1000000       |
| Random Forest       | train |   385,375  |   614,625  |        596,021  |           13,680  |           18,604  |        371,695  | 0.977563 | 0.952334 | 0.967716 | 1000000       |
|                     | test  |   373,875  |   626,125  |        335,566  |           30,826  |         290,559   |        343,049  | 0.915866 | 0.541422 | 0.678615 | 1000000       |
| AdaBoost            | train |   382,091  |   617,909  |        468,945  |         140,756   |         148,964   |        241,335  | 0.769139 | 0.618334 | 0.71028  | 1000000       |
|                     | test  |   386,341  |   613,659  |        276,723  |           89,669  |         336,936   |        296,672  | 0.755265 | 0.468226 | 0.573395 | 1000000       |
| LR With Donation    | train |   362,418  |   637,582  |        526,711  |           82,990  |         110,871   |        279,428  | 0.863884 | 0.715933 | 0.806139 | 1000000       |
|                     | test  |   394,765  |   605,235  |        333,337  |           33,055  |         271,898   |        361,710  | 0.909782 | 0.570873 | 0.695047 | 1000000       |
| Naïve with Donation | train |   411,035  |   588,965  |        390,753  |         218,948   |         198,212   |        192,087  | 0.640893 | 0.492153 | 0.58284  | 1000000       |
|                     | test  |   381,313  |   618,687  |        268,972  |           97,420  |         349,715   |        283,893  | 0.73411  | 0.448058 | 0.552865 | 1000000       |


### Conclusion and Next Steps

In conclusion, the models that we ran using the donation data in addition to the voter file did not yield significantly better results than the voter file alone.  This does not mean that we should not look at additional datasets.  ACS (census), polling data, and other sources could be used to enhance the model.  

The models showed higher specificity than sensitivity.  The next steps of this project are to investigate why that is, and optimize the models for the metric we care about (sensitivity).


---

### Source Documentation

- [North Carolina Board Of Elections](https://www.ncsbe.gov/index.html)
- [Follow The Money](https://www.followthemoney.org/)
- [Huff Po Pollster](https://elections.huffingtonpost.com/pollster)
