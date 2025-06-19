# Customer Lifetime Value Prediction (RFM + Random Forest)

Predict the *future revenue potential* of each customer using classic RFM metrics and a Random Forest Regressor.  
This project was built as part of an internship phase, with a strict 2‑week timeline and a 1–2 page report requirement.

---

## Table of Contents
1. [Project Overview](#project-overview)
2. [Dataset](#dataset)
3. [Quick Start](#quick-start)
4. [Usage](#usage)
5. [Results](#results)
6. [Contributing](#contributing)

---

## Project Overview
- **Goal :** Estimate Customer Lifetime Value (CLV) from historical sales data.  
- **Method :**  
  1. Create **Recency‑Frequency‑Monetary (RFM)** features.  
  2. Train a **Random Forest Regressor** to predict monetary value per customer.  
  3. Segment customers into **Low / Medium / High** LTV tiers.  
- **Deliverables :**  
  - `Customer_LTV.ipynb` notebook (fully reproducible)  
  - `customer_ltv_segments.csv` with predicted LTV scores  
  - Report (`Customer Lifetime Value Prediction Model.docx`) summarising approach & findings  

---

## Dataset
| Column          | Description                              |
|-----------------|------------------------------------------|
| **CustomerID**  | Unique customer identifier               |
| **InvoiceNo**   | Transaction/invoice number               |
| **InvoiceDate** | Purchase timestamp (dd‑mm‑yyyy HH:MM)    |
| **Quantity**    | Number of units bought                   |
| **UnitPrice**   | Price per unit                           |
| **Country**     | Customer location                        |

> **Note:** The raw CSV (`customer_segmentation.csv`) is taken from `https://www.kaggle.com/datasets/sergeymedvedev/customer_segmentation`. Remove or anonymise it if your org’s policy requires.

---

## Quick Start
```bash
# 1️ Clone the repo
git clone https://github.com/your‑username/customer‑lifetime‑value‑prediction.git
cd customer‑lifetime‑value‑prediction

# 2️ Create a virtual environment (optional but recommended)
python -m venv .venv
source .venv/bin/activate        # On Windows: .venv\Scripts\activate

# 3️ Install dependencies
pip install -r requirements.txt

# 4️ Run the notebook or the script
jupyter notebook Customer_LTV.ipynb
# ‑‑ or ‑‑
python src/ltv_model.py
```

## Usage
The notebook walks through:

- Data Loading & Cleaning
- Handles ISO‑8859‑1 encoding
- Drops credit‑note invoices and negative quantities
- Feature Engineering
- Computes Recency, Frequency, Monetary
- Generates Amount = Quantity × UnitPrice
- Model Training

```python
model = RandomForestRegressor(
    n_estimators=200,
    random_state=42
)
model.fit(X_train, y_train)
```

## Results
### Evaluation

```python
y_pred = model.predict(X_test)
mse  = mean_squared_error(y_test, y_pred)
rmse = np.sqrt(mse)
mae  = mean_absolute_error(y_test, y_pred)
```

### Segmentation

```python
rfm["Segment"] = pd.qcut(
    rfm["Predicted_LTV"], q=3,
    labels=["Low", "Medium", "High"]
)
```

### Export
```bash
Saves customer_ltv_segments.csv
```
High‑LTV customers (top 33 %) account for ~70 % of projected revenue—prioritise retention campaigns here.

## Contributing
Pull requests are welcome! Please open an issue first to discuss your ideas or bug fixes.
