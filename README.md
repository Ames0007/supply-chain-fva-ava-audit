# 📊 Auditing Your Supply Chain with FVA & AVA

A supply-chain decision-audit project focused on **Forecast Value Added (FVA)** and **Assortment Value Added (AVA)**—two practical frameworks for determining whether human interventions and assortment decisions actually improve business outcomes.

> 📓 [Open the complete notebook](notebooks/supply_chain_fva_ava_audit.ipynb)

## 🎯 Business Problem

Supply-chain planning processes often contain multiple layers of human intervention. A forecast may be generated automatically, modified by a demand planner, and then adjusted again by sales. More intervention does not necessarily mean a better forecast.

This project audits those decisions instead of assuming they add value.

## 🔍 Forecast Value Added (FVA)

FVA measures whether a forecasting-process step improves accuracy relative to the forecast entering that step.

The notebook compares:

1. Automatic statistical forecast
2. Demand Planner adjustment
3. Sales Team adjustment

## 📉 Forecast Accuracy Audit

![FVA MAPE Comparison](images/fva_mape_comparison.png)

| Forecast Stage | MAPE |
|---|---:|
| Automatic Forecast | **5.01%** |
| Demand Planner Adjustment | **1.80%** |
| Sales Team Adjustment | **3.06%** |

The Demand Planner substantially improves forecast accuracy. The subsequent Sales adjustment worsens the planner's forecast, although the final forecast remains better than the original automatic baseline.

## 📈 Forecast Value Added

![FVA Contribution](images/fva_contribution.png)

| Intervention | FVA |
|---|---:|
| Demand Planner | **+64.07%** |
| Sales adjustment vs Planner | **−70.0%** |
| Overall vs Automatic Baseline | **+38.92%** |

### Interpretation

**Positive FVA** means an intervention improved forecast accuracy.

**Negative FVA** means the intervention reduced forecast quality and should be challenged, redesigned, or governed more carefully.

The most important result is therefore not simply that humans improve forecasts. It is that **different human interventions have very different value**.

## 🧠 S&OP Governance Insight

The analysis provides a strong S&OP lesson:

```text
Automatic Forecast
        ↓
Demand Planner Adjustment
        ↓
   POSITIVE FVA
        ↓
Sales Adjustment
        ↓
   NEGATIVE FVA
        ↓
Governance / Root-Cause Review
```

Rather than allowing every manual override, organizations can use FVA to identify which interventions consistently improve accuracy and which introduce bias or noise.

## 🤝 Ensemble Forecasting

The notebook also explores combining forecast inputs rather than relying exclusively on one contributor. Ensemble approaches can reduce dependence on individual judgment and provide a more robust planning baseline.

## 🛒 Assortment Value Added (AVA)

The second major concept is **Assortment Value Added**.

AVA extends the same decision-audit philosophy to assortment management: evaluate whether adding, removing, or prioritizing products actually creates incremental value rather than simply increasing complexity.

AVA can support questions such as:

- Which SKUs genuinely contribute value?
- Which assortment decisions increase complexity without enough return?
- Should low-value products be rationalized?
- Where should inventory and planning effort be concentrated?

## 💡 Key Business Insights

- Forecast-process steps should be **measured**, not assumed to add value.
- The Demand Planner intervention produces strong positive FVA.
- The Sales adjustment produces negative incremental FVA relative to the planner forecast.
- The final human-adjusted forecast still improves on the automatic baseline overall.
- FVA can strengthen S&OP governance by identifying harmful overrides.
- AVA applies the same evidence-based thinking to assortment and SKU decisions.
- Decision auditing can reduce unnecessary planning complexity and focus attention on interventions that demonstrably improve outcomes.

## 🛠️ Technologies

- Python
- Pandas
- NumPy
- Matplotlib
- Jupyter Notebook
- Forecast accuracy metrics
- FVA / AVA decision frameworks

## 📂 Repository Structure

```text
supply-chain-fva-ava-audit/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── supply_chain_fva_ava_audit.ipynb
└── images/
    ├── fva_mape_comparison.png
    └── fva_contribution.png
```

## 🚀 Run Locally

```bash
git clone <YOUR-GITHUB-REPOSITORY-URL>
cd supply-chain-fva-ava-audit
python -m venv .venv
source .venv/Scripts/activate
pip install -r requirements.txt
jupyter notebook notebooks/supply_chain_fva_ava_audit.ipynb
```

## 📌 Conclusion

This project demonstrates an important principle in supply-chain analytics: **every intervention should earn its place in the process**.

FVA reveals which forecasting adjustments improve accuracy and which destroy value, while AVA extends the audit mindset to assortment decisions. Together, they provide a practical framework for improving forecast governance, S&OP discipline, and supply-chain decision quality.
