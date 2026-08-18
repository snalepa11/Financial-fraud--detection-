# Data Directory

## Dataset: dirty_data.csv

This directory should contain the `dirty_data.csv` file for the Caishen Bank fraud detection project.

### File Size
The dataset is approximately **471MB** and is not included in this repository due to GitHub's file size limitations.

### How to Obtain the Data

**Option 1: If you're a TKH student**
- Download the dataset from your course materials or instructor

**Option 2: Alternative sources**
- The dataset appears to be based on the PaySim fraud detection dataset
- You can find similar datasets at:
  - [Kaggle - Synthetic Financial Datasets](https://www.kaggle.com/datasets)
  - Contact your instructor for the specific dataset

### Expected File Location
```
Financial-fraud--detection-/
└── data/
    └── dirty_data.csv  <- Place your downloaded file here
```

### Dataset Structure
The CSV file should contain the following columns:
- `step` - Time step
- `type` - Transaction type (PAYMENT, TRANSFER, CASH_OUT, DEBIT, CASH_IN)
- `amount` - Transaction amount
- `nameOrig` - Origin account ID
- `oldbalanceOrg` - Origin account balance before transaction
- `newbalanceOrig` - Origin account balance after transaction
- `nameDest` - Destination account ID
- `oldbalanceDest` - Destination account balance before transaction
- `newbalanceDest` - Destination account balance after transaction
- `isFraud` - Fraud indicator (0 = legitimate, 1 = fraud)
- `isFlaggedFraud` - Flagged fraud indicator

### Next Steps
Once you have the `dirty_data.csv` file:
1. Place it in this directory
2. Run the notebooks in this order:
   - `../code/eda.ipynb` - Exploratory Data Analysis
   - `../code/preprocessing.ipynb` - Data Preprocessing
   - `../code/model_training.ipynb` - Model Training
