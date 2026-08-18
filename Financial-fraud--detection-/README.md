# Caishen Bank - Fraud Detection MVP

## 🏦 Project Overview

**Organization:** Caishen Bank (NYC-based International Bank)

**Mission:** Develop a machine learning solution to identify fraudulent activity within customer-facing bank accounts over the next 3 years.

**Objective:** Build a Minimum Viable Product (MVP) using ensemble classifiers (Random Forest and Gradient Boosting) to detect fraudulent transactions in real-time.

This project analyzes over 6 million financial transactions to build a robust fraud detection system that can identify suspicious activity while minimizing false alarms.

---

## 📊 Dataset

The project uses a comprehensive financial transaction dataset containing:
- **Total Transactions:** 6,362,620
- **Fraudulent Transactions:** 8,213 (0.129%)
- **Dataset Size:** 471MB

### Features
- `step` - Time step of transaction
- `type` - Transaction type (PAYMENT, TRANSFER, CASH_OUT, DEBIT, CASH_IN)
- `amount` - Transaction amount
- `nameOrig` - Origin account ID
- `oldbalanceOrg` - Origin account balance before transaction
- `newbalanceOrig` - Origin account balance after transaction
- `nameDest` - Destination account ID
- `oldbalanceDest` - Destination account balance before transaction
- `newbalanceDest` - Destination account balance after transaction
- `isFraud` - Fraud indicator (target variable)
- `isFlaggedFraud` - System-flagged fraud indicator

**Note:** The dataset file (`dirty_data.csv`) is not included in this repository due to its size (471MB). See [data/README.md](data/README.md) for instructions on obtaining the dataset.

---

## 🔍 Key Findings

### Class Imbalance Challenge
- Only **0.129%** of transactions are fraudulent
- Severe imbalance ratio: **774:1** (legitimate:fraud)
- Requires specialized handling with `class_weight='balanced'` and careful metric selection

### Fraud Patterns
- **Transaction Types with Fraud:**
  - TRANSFER transactions: Primary fraud vector
  - CASH_OUT transactions: Secondary fraud vector
  - PAYMENT, DEBIT, CASH_IN: Zero fraud instances

- **Ineffective Fraud Detection System:**
  - `isFlaggedFraud` only caught **16 out of 8,213** frauds (0.19% detection rate)
  - Demonstrates need for advanced ML solution

### Balance Analysis
- Fraudulent transactions have **1.98x higher** average origin balance
- 33% of all transactions have zero balance
- Balance inconsistencies are strong fraud indicators

---

## 🛠️ Technical Approach

### 1. Exploratory Data Analysis (EDA)
**Notebook:** [`code/eda.ipynb`](code/eda.ipynb)

- Analyzed transaction distributions across types
- Investigated fraud patterns and correlations
- Examined balance behaviors and anomalies
- Created visualizations comparing fraud vs legitimate transactions

**Key Visualizations:**
- Fraud rate by transaction type
- Transaction amount distributions
- Balance change patterns
- Feature correlation analysis

### 2. Data Preprocessing
**Notebook:** [`code/preprocessing.ipynb`](code/preprocessing.ipynb)

**Feature Engineering (11 new features created):**
- `balance_change_orig` - Origin balance change
- `balance_change_dest` - Destination balance change
- `amount_to_balance_ratio` - Transaction size relative to account balance
- `is_zero_balance_orig` - Zero balance flag (fraud indicator)
- `is_zero_balance_dest` - Destination zero balance flag
- `balance_error_orig` - Origin balance inconsistency detection
- `balance_error_dest` - Destination balance inconsistency detection
- `has_balance_error` - Combined balance error flag
- `account_type_orig_encoded` - Origin account type (Customer/Merchant)
- `account_type_dest_encoded` - Destination account type
- `is_c2c_transaction` - Customer-to-customer transaction flag

**Preprocessing Steps:**
1. ✅ Data quality check (no missing values)
2. ✅ Feature engineering for fraud detection
3. ✅ Categorical encoding (Label Encoding for tree-based models)
4. ✅ Removed irrelevant features (IDs, ineffective flags)
5. ✅ Stratified train-test split (80/20)
6. ✅ Data saved for model training

**Key Decisions:**
- **No feature scaling** - Tree-based ensemble models don't require it
- **Label encoding over one-hot** - More efficient for Random Forest/Gradient Boosting
- **Stratified split** - Maintains fraud rate in both train and test sets

### 3. Model Training & Evaluation
**Notebook:** [`code/model_training.ipynb`](code/model_training.ipynb)

**Ensemble Models Compared:**
- **Random Forest Classifier**
  - 100 trees
  - `class_weight='balanced'` to handle imbalance
  - All CPU cores utilized (`n_jobs=-1`)

- **Gradient Boosting Classifier**
  - 100 boosting stages
  - Sequential tree building
  - Optimized for imbalanced data

**Evaluation Metrics:**
- **Accuracy** - Overall correctness
- **Precision** - Fraud predictions that are actually fraud
- **Recall** - Actual frauds that are detected
- **F1-Score** - Harmonic mean of precision and recall
- **ROC-AUC** - Area under receiver operating characteristic curve

**Model Selection Criteria:**
- Primary metric: **F1-Score** (balances precision and recall)
- Business focus: Maximize fraud detection while minimizing false alarms

---

## 📈 Results

### Model Performance
*(Results will vary based on the specific dataset and final model selection)*

**Expected Performance Metrics:**
- High ROC-AUC score (>0.95) indicating strong discrimination ability
- Balanced precision-recall trade-off
- Significantly outperforms baseline flagging system (16 frauds caught vs. 8,213 actual)

### Feature Importance
Top fraud indicators identified by the model:
1. Balance change features
2. Transaction amount
3. Balance error detection
4. Account type combinations
5. Zero balance flags

### Business Impact
- **Fraud Detection Rate:** Significantly higher than current 0.19% system
- **False Alarm Management:** Optimized to reduce customer friction
- **Scalability:** Real-time prediction capability for production deployment

---

## 📁 Project Structure

```
Financial-fraud--detection-/
├── README.md                          # Project documentation (this file)
├── code/
│   ├── eda.ipynb                      # Exploratory Data Analysis
│   ├── preprocessing.ipynb            # Data preprocessing & feature engineering
│   └── model_training.ipynb           # Model training & evaluation
├── data/
│   ├── README.md                      # Dataset instructions
│   └── dirty_data.csv                 # Raw dataset (not in repo - 471MB)
└── models/
    ├── fraud_detection_model_*.pkl    # Trained model (generated after training)
    └── model_metadata.pkl             # Model performance metrics
```

---

## 🚀 Getting Started

### Prerequisites
- Python 3.9+
- Jupyter Notebook or JupyterLab
- Required libraries:
  ```bash
  pip install pandas numpy matplotlib seaborn scikit-learn
  ```

### Installation

1. **Clone the repository:**
   ```bash
   git clone https://github.com/snalepa11/Financial-fraud--detection-.git
   cd Financial-fraud--detection-
   ```

2. **Obtain the dataset:**
   - See [data/README.md](data/README.md) for instructions
   - Place `dirty_data.csv` in the `data/` directory

3. **Run the notebooks in order:**

   **Step 1: Exploratory Data Analysis**
   ```bash
   jupyter notebook code/eda.ipynb
   ```
   - Understand the data
   - Visualize fraud patterns
   - Identify key insights

   **Step 2: Data Preprocessing**
   ```bash
   jupyter notebook code/preprocessing.ipynb
   ```
   - Engineer fraud detection features
   - Prepare training/test sets
   - Save preprocessed data

   **Step 3: Model Training**
   ```bash
   jupyter notebook code/model_training.ipynb
   ```
   - Train ensemble classifiers
   - Compare model performance
   - Save production model

---

## 🎯 Key Features

### Fraud Detection Capabilities
- ✅ **Real-time prediction** - Fast ensemble inference
- ✅ **Imbalanced data handling** - Class weighting and appropriate metrics
- ✅ **Interpretable features** - Business-understandable fraud indicators
- ✅ **Production-ready** - Serialized model for deployment

### Technical Highlights
- **Ensemble Learning** - Combines multiple decision trees for robust predictions
- **Feature Engineering** - Custom fraud-specific features from domain knowledge
- **Cross-validation** - Ensures model generalization
- **Comprehensive Evaluation** - Multiple metrics for complete performance picture

---

## 📊 Visualizations

The project includes comprehensive visualizations:
- Fraud distribution by transaction type
- ROC and Precision-Recall curves
- Confusion matrices
- Feature importance rankings
- Transaction amount comparisons
- Balance error analysis

---

## 🔮 Future Improvements

1. **Advanced Models**
   - XGBoost, LightGBM, CatBoost for improved performance
   - Deep learning approaches for complex pattern detection
   - Ensemble stacking combining multiple model types

2. **Feature Engineering**
   - Temporal patterns (time-based fraud indicators)
   - Network analysis (transaction graphs)
   - Customer behavior profiling

3. **Production Deployment**
   - REST API for real-time predictions
   - Model monitoring and drift detection
   - A/B testing framework
   - Automated retraining pipeline

4. **Business Intelligence**
   - Dashboard for fraud analytics
   - Alert system for high-risk transactions
   - Customer impact analysis

---

## 👥 Author

**Sarah Nalepa**
- GitHub: [@snalepa11](https://github.com/snalepa11)

---

## 📝 License

This project was developed as part of the TKH Data Analytics program.

---

## 🙏 Acknowledgments

- **Caishen Bank** (Fictional Organization) - Project context
- **TKH (The Knowledge House)** - Educational program
- **Cybersecurity Team** - Dataset provision (fictional scenario)

---

## 📞 Contact

For questions or collaboration opportunities, please reach out through GitHub.

---

**⚠️ Important Notes:**
- This is an educational project and should not be used for actual fraud detection without proper validation
- The dataset is synthetically generated for learning purposes
- Model performance may vary based on data characteristics and hyperparameter tuning
