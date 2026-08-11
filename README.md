# 📊 Auditing Your Supply Chain with FVA and AVA

> **GitHub edition of the complete notebook narrative.** This README preserves the explanatory methodology, formulas, examples, recommendations, and conclusions from the notebook rather than reducing them to a short project summary.

📓 **Full executable notebook:** [`notebooks/supply_chain_fva_ava_audit.ipynb`](notebooks/supply_chain_fva_ava_audit.ipynb)

---

# Introduction

Today, in modern supply chains, sophisticated systems generate forecasts and allocation proposals. However, human planners often review and adjust these recommendations. The critical question for supply chain leaders is not just "How accurate are our forecasts?" but "Are these manual interventions actually adding value, or are they introducing noise and cost?"

This article introduces two powerful audit tools:

* **Forecast Value Added (FVA)**: Evaluates whether human adjustments improve demand forecasts.

* **Allocation Value Added (AVA)**: Uses the same FVA approach to assess whether human adjustments to inventory allocation proposals lead to better distribution outcomes,regarding stockouts and overstocks.

By using both FVA and AVA, organizations can objectively measure the contribution of each planning and execution step, optimize processes, and ensure resources are focused where they truly add value.

***Important Note:*** I've not found a standard name for this latter methodology. It can be symply addressed as FVA or you can find it called also Inventory Optimization Analysis, Inventory Value Added (acronym too similar to IVA, the Italian value added taxes 😭), Inventory Value Added Analysis. My intention was to keep the two process separated.

---

# What is Forecast Value Added (FVA)?

Forecast Value Added (FVA) is a management technique used to assess the effectiveness and efficiency of a demand planning process. It measures what incremental value each step or functional team adds to the forecast's accuracy. FVA is an internal audit tool that identifies and helps eliminate non-value-added work.

### Why FVA Matters to Supply Chain Professionals?

Many organizations spend time and resources adjusting automated forecasts. If these manual adjustments consistently make the forecast worse, they introduce costs without benefit. FVA provides the following outcomes:

* Quantify Functional Contribution: measure the value added by different teams (Sales, Planning, Marketing).

* Identify Waste: detect non-value-added steps and remove them, freeing up planner time.

### Establishing the Baseline:

The **Baseline Forecast** is the starting point against which all subsequent forecast versions are measured. The most common baseline is the output from a fully Automated Model (Statistical, ML).
It represents the accuracy that can be achieved without any human intervention. Every subsequent forecast adjustment must be able to beat this baseline to be considered valuable. In Sales & Operations Planning (S&OP), for example, the forecast moves sequentially through different hands in order to achieve a consensus plan. We must measure the incremental value added by each function.

---

### The FVA Metric:

FVA is expressed as a percentage representing the reduction in error achieved by the intervention step. In this demostration we'll use Mean Absolute Percent Error (MAPE). The formula for the value added by a specific Step n is:

![fva1.jpg](attachment:84a13fbd-45e4-491a-93b8-c35399546def.jpg)

* **FVA >0%**: Value Added. The step improves accuracy.
* **FVA <0%**: Value Destroyed. The step made the forecast worse.


**Advice**: It is also always good practice to measure bias as an error metric, alongside commonly used metrics like MAPE, Root Mean Square Error (RMSE), or Weighted Mean Absolute Percent Error (WMAPE). Bias provides essential insight into the systematic direction of the error (consistent over- or under-forecasting).

Furthermore, it is useful to evaluate an error concept that is more directly linked to financial results (such as utilizing penalty costs for stockouts or overstocks). We will explore this crucial concept of a financialized error metric, including penalty systems, in the subsequent Allocation Value Added (AVA) section and in future articles.

---

### FVA Data and Scenario Description

We analyze the sequential contributions of the Demand Planner and the Sales team for one product over 12 months, tracking the sequential MAPE and FVA.

**Scenario**: The Automatic Model (F1) shows a high initial error, failing to accurately capture the magnitude of the Q3 spike. The model's features should be enriched in the future to better forecast spikes due to promotions.
The Demand Planner (F2) is aware of the promotions and uses historical data to correct the baseline. The Sales team (F3) then provides an input based on aggressive targets, over-inflating the peak demand. We evaluate the value added by each function.

---

In the code below, we will compute MAPE for the Statistical model, Demand Planner and Sales Team

---

### Calculating Functional FVA

We apply the FVA formula sequentially to evaluate the contribution of each team.

---

### Evaluating Functional FVA Contribution:

The FVA results provide clear evidence regarding the value of each intervention:

* **Planner Adjustment**: The Demand Planner's deep knowledge of the business and the forecast tool (baseline) was decisive to correct the initial error by over 64%. They received the Automatic model's forecast, with 5% of error, and adjusted the forecast only in the critical months achieving a very low final MAPE.

*  **Sales Adjustment**: The Sales adjustment made the forecast 70% worse. They introduced a strong bias that moved the forecast away from actual demand.

**Key Takeaway**: This analysis demonstrates that the Demand Planning function is the primary driver of value in the process, serving as the necessary check against systematic error. The Sales override failed to add value lowering the accuracy gained by the Planner, ultimately compromising the quality of the final forecast.

---

### Leveraging FVA: Adjusting Influence in the S&OP Process (Consensus Focus)

The FVA results provide the data needed to govern the collaborative S&OP process, ensuring that human intervention is focused and value-driven. While the analysis showed the Demand Planner (F2) is the most reliable forecast contributor, the sales team's qualitative knowledge remains crucial. The strategy is to formalize the flow of information and forecast adjustments.

* The Planner Adjusted Forecast (F2) must be established as the "Recommended Consensus Start" for the S&OP meeting. Since the planner delivered a FVA of +64.07%, any subsequent adjustment must start from this version.

* During the consensus meeting, any further adjustment must take into consideration the FVA weights achieved. In some case the authority to adjust the final plan can be temporarely restricted.

* Qualitative Inputs that comes from all over the organization are always important for the planning process. In this scenario, these input  should be captured and delivered to the Demand Planner before the S&OP meeting. The Planner will then review use them to adjust the automatic model (F1) to create a stronger (F2). This ensures Sales input is considered by the planning expert before consensus.

* The negative Sales FVA must trigger mandatory training on forecast methodology for the Sales team to eliminate behavioral bias and improve the quality of their qualitative input for the next cycle.

* The final consensus meeting should not be a negotiation of numbers, but a risk assessment against the (F2) baseline. All teams (Demand Planning, Sales, Finance, Operations) collectively agree on the single, common, and unconstrained demand plan. The discussion focuses only on:

  -1. Validating the F2 assumptions.

  
  -2. Making minor adjustments where new information has emerged since F2 was finalized.


By implementing this structure, the process retains the benefits of collaborative input while using FVA to ensure that only value-adding steps influence the final decision.

---

### Alternative Approach: Ensemble Method

As an alternative, organizations can use FVA results to implement a numerical weighting system (an ensemble forecast). This approach automates the final forecast by blending the output of all individual forecasts, giving greater weight to the most historically accurate contributor.

In this model, the final Consensus Forecast is a weighted average of all independent forecasts. Weights are calculated inversely proportional to the historical error. in this example:

* F1 (Automatic Model)  = 1/5.01 ≈ 0.20   --> Normalized Weight = 18.5%
* F2 (Planner Adjusted) = 1/1.80 ≈ 0.55   --> Normalized Weight = 51.0%
* F3 (Sales Adjusted)   = 1/3.06 ≈ 0.33   --> Normalized Weight = 30.5%

The resulting ensemble forecast would mathematically assign half of the final influence to the Planner Adjusted Forecast, while significantly reducing the influence of the underperforming Sales Forecast. This method ensures that historical error directly controls future forecast.

---

# What is Allocation Value Added (AVA)?

Allocation Value Added (AVA) is a method that **extends the FVA principle to the execution phase** of the supply chain. AVA measures whether a human planner's adjustments to a system-generated allocation proposal actually improve the final distribution outcome in terms of real costs (e.g., minimizing stockouts and overstocks).

---

### Why AVA Matters to Supply Chain Professionals?

Just as with forecasting, system-recommended allocations are often manually adjusted. These adjustments can be based on feelings, incomplete local information, or even internal politics. the objective of AVA is to:

* Quantify Planner Impact: Measure the financial value added (or destroyed) by allocation planners.

* Optimize Inventory Flow: Ensure stock is placed where it generates the most profit and minimizes penalties.

* Validate System Rules: Confirm if the automated allocation system is performing optimally, or if planner input reveals recurrent system's errors.

### Measuring Error in Distribution:

In distribution, a simple percentage error (like MAPE) is not enough. The cost of a stockout is usually greater than the cost of an equivalent overstock. Therefore, AVA uses a cost-based performance metric to measure the errors.
The Error for AVA is the **Total Allocation Penalty**, which is the sum of quantified financial losses from stockouts and overstock for a given allocation decision.

The AVA formula then becomes:

![image.png](attachment:641be8b3-6dc0-496a-aec8-011dbd6dc15b.png)

---

### Scenario:

We examine **three weekly allocation decisions for a single product**. This setup demonstrates the critical trade-off: the significant gains from a correct intervention (Run 1) versus the destruction from unnecessary or biased interventions (Runs 2 & 3).

The automated system proposes an allocation. The planner, possessing local knowledge adjusts the allocation. We will determine if this adjustment reduced the total financial penalty.

**Assumptions**:

* Lost Profit per Stockout Unit = €10

* Holding Cost per Excess Unit = €2

---

The code below computes the total run penalties and the global penalties for both System and Planner. Using this metrics we will be able to calculate the AVA for the single run but also the global AVA.

---

### Analysis of Individual and Total AVA:

* **Run1:** Actual demand was 140 units, while the system proposed 127 units. The Planner, correctly identifying an un-tracked market event, allocated 135 units, saving 80€ in lost profit. The system's total penalty was 130€ (due to 13 units of stockout). The Planner's allocation generated only a 50€ profit loss.

* **Run2:** Planner costs the company 30€ by creating unnecessary overstock. Actual demand was 100 units, system proposal was 105 units, planner's adjustment was 120 units.

* **Run3:** Planner increases the penalty by turning a minor overstock into an expensive stockout. Actual Demand was 105 units, System Proposed 110 (10€ overstock cost), Planner's allocation was 100 units, resulting in a 50€ profit loss due to stockout.

### Total Evaluation: The Risk of Unjustified Overwrite

**The overall AVA is positive by only +7%**, Total System Penalty is 150€ while Total Planner's penalty is 140€, but this value is entirely driven by the single intervention in Run 1. This gain barely compensates for the significant losses incurred by the poor and unjustified interventions in Runs 2 and 3.

**Conclusion**: The analysis clearly shows that the Planner must overwrite the system only when they are certain of a local event not captured by the system (like the promotion in Run 1). The risk of generating a much bigger financial error when intervening without concrete evidence is high and unnecessary.

The Planner's override authority should therefore be restricted to only those situations where they possess **concrete and documented knowledge**. Any planner adjustment that creates a stockout where the system predicted a surplus (as seen in Run 3) should trigger a high-risk review before execution. As this example demonstrates, frequent adjustments made purely by "gut feeling" can lead to much bigger errors in the long run. However, the overall planning process must continue integrating the Planner's well-documented knowledge about local events for continuous improvement.

---

# Conclusion: Continuous Improvement Across the Supply Chain

**Forecast Value Added (FVA)** and **Allocation Value Added (AVA)** are indispensable tools for modern supply chain professionals,providing financial quantification of human intervention.

* FVA guides improvements in the planning process, ensuring that S&OP discussions are data-driven and that forecasting efforts are concentrated where they truly reduce demand error.

* AVA provides crucial insights into the execution process, validating planner decisions and highlighting areas where automated allocation rules might need enhancement or where planners possess unique information.

By integrating both FVA and AVA into regular performance reviews and continuous improvement initiatives, organizations can systematically identify waste, empower high-value contributors, and achieve financial benefits throughout their supply chain. **These metrics transform subjective judgments into objective insights, fostering a culture of data-driven decision-making**.

---


# Visual Audit Summary

The charts below summarize the principal FVA results calculated in the notebook.

## Forecast Accuracy Audit (MAPE)

![Forecast Accuracy Audit](images/fva_mape_comparison.png)

The automatic forecast has a MAPE of **5.01%**. The Demand Planner reduces the error to **1.80%**, while the subsequent Sales Team adjustment increases it to **3.06%**.

## Forecast Value Added by Intervention

![Forecast Value Added](images/fva_contribution.png)

- **Demand Planner FVA:** +64.1%
- **Sales Adjustment FVA:** -70.0%
- **Overall FVA vs baseline:** +38.9%

These visuals complement the detailed calculations and interpretation in the notebook sections above.

---

# Repository Structure

```text
supply_chain_fva_ava_audit_project/
├── README.md
├── requirements.txt
├── .gitignore
├── notebooks/
│   └── supply_chain_fva_ava_audit.ipynb
└── images/
    ├── fva_mape_comparison.png
    └── fva_contribution.png
```

# Run Locally

```bash
git clone https://github.com/Ames0007/supply-chain-fva-ava-audit.git
cd supply-chain-fva-ava-audit
python -m venv .venv
source .venv/Scripts/activate
pip install -r requirements.txt
jupyter notebook notebooks/supply_chain_fva_ava_audit.ipynb
```
