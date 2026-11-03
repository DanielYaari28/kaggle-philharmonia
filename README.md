Predicting which past patrons of the Philharmonia Baroque Orchestra will buy a subscription to the 2014–15 concert season (Duke AIPI Kaggle competition, practice run).

Target: binary — purchased a 2014–15 subscription of any level (1) or not (0)
Output: probability of subscribing for each account in test.csv
Metric: AUROC
Results
Model	Features	CV AUROC (5-fold)	Public leaderboard
Logistic regression	8	0.961	0.978
Repository structure
kaggle-philharmonia/
├── data/
│   ├── raw/            # Kaggle CSVs go here (not committed)
│   └── processed/      # generated tables (not committed)
├── notebooks/
│   ├── 01_eda.ipynb        # data exploration, missing-value and leakage checks
│   └── 02_features.ipynb   # feature build, cross-validation, submission
├── submissions/        # prediction files uploaded to Kaggle
├── requirements.txt
└── README.md
Data

The competition data is not included in this repository. Download it from the Kaggle competition page and place these files in data/raw/:

train.csv, test.csv, account.csv, subscriptions.csv, tickets_all.csv, concerts.csv, concerts_2014-15.csv, zipcodes.csv, sample_submission.csv

Loading notes:

File	Setting needed
account.csv	encoding='latin-1'
concerts.csv, concerts_2014-15.csv	encoding='utf-8-sig'
zipcodes.csv	lineterminator='\r'
How to run

The notebooks are written for Google Colab with the data stored in Google Drive.

Upload the CSVs to MyDrive/kaggle-philharmonia/data/raw/ in Google Drive.
Open notebooks/01_eda.ipynb in Colab and choose Runtime → Run all (approve the Drive mount when asked).
Open notebooks/02_features.ipynb and run all cells. It prints the cross-validated AUROC and writes the submission to MyDrive/kaggle-philharmonia/submissions/.

To run locally instead, install the requirements and change the RAW path at the top of each notebook to your local data/raw/ folder:

bash
pip install -r requirements.txt
Approach (summary)
Exploration: profiled each file for missing values and format changes; checked donation fields for leakage.
Features: one row per account built from subscription history, 2013 donations and ticket purchases.
Validation: stratified 5-fold cross-validation, with scaling inside a pipeline.
Model: logistic regression, refit on all training data to predict the test set.
