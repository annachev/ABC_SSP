# Philips Supplier Sustainability Analytics - Data Package

## Quick Start

### 1. Install Required Packages
```bash
pip install -r requirements.txt
```

### 2. Verify Installation
```python
import pandas as pd
import sklearn
import xgboost
print("✓ Ready to start")
```

### 3. Load Data
```python
df = pd.read_excel('data/SSP_Data.xlsx')
```

---

## Assignment Requirements

**Main Deliverable:** 4-6 page brief (slide deck or written report)

**Core Tasks:**
1. Build GI-only models (observable supplier characteristics)
2. Build GI+SAQ models (with detailed questionnaires)
3. Compare both approaches
4. Create risk segmentation framework
5. Check fairness across regions

**Expected Timeline:** 10-14 hours

---

## Data Files

### SSP_Data.xlsx
- **1,236 assessments** from **463 suppliers** (2016-2025)
- **Target variable:** `Val_Score` (Overall Sustainability score, 0-1 scale)
- **Topic scores:** Val_Environment, Val_Health and Safety, Val_Business Ethics, Val_Labor and Human Rights
- **GI features (31):** Activities, Facilities, Number of workers, Country
- **SAQ features (~450):** Q-code columns (filter out columns with >50% missing)

### column_explanation.csv
- Maps Q-codes to topics and chapters
- Example: Q1272 → Environment - Corrective Action Approach
- Use this to interpret SAQ feature importance

---

## Critical Requirements

### ⚠️ Train/Test Split
**You MUST split by supplier ID, not randomly.**

Random split causes data leakage (same supplier in train and test).

Example approaches:
```python
# Option 1: GroupShuffleSplit
from sklearn.model_selection import GroupShuffleSplit
splitter = GroupShuffleSplit(test_size=0.3, random_state=42)
train_idx, test_idx = next(splitter.split(X, y, groups=df['ID']))

# Option 2: Manual split
unique_suppliers = df['ID'].unique()
train_suppliers, test_suppliers = train_test_split(
    unique_suppliers, test_size=0.3, random_state=42
)
train_mask = df['ID'].isin(train_suppliers)
```

### ⚠️ Two-Stage Approach
The assignment requires you to:
1. **First:** Build models with GI features only
2. **Then:** Add SAQ features and compare

Don't skip the GI-only stage. Understanding the trade-off is a core learning objective.

---

## Data Notes

### Q-Codes (SAQ Features)
- Question text is confidential (Philips proprietary)
- You have: Q-code → Topic → Chapter mapping
- This is sufficient for interpretation
- Example: "Q1272 ranks #1" → "Environment Corrective Action matters most"

### Missing Values
- Many SAQ columns have >50% missing (suppliers didn't answer)
- Consider filtering out high-missing columns
- Document your approach

### Geographic Distribution
- ~93% of suppliers are in China
- Remaining in India, Germany, Indonesia, Italy, Taiwan, Thailand, Türkiye, Yemen
- Check for fairness across regions in your analysis

---

## Expected Performance Ranges

These are guidelines, not requirements. Your results may vary.

| Metric                    | GI-Only | GI+SAQ | 
|---------------------------|---------|--------|
| Improvement over baseline | 3-10%   | 15-30% |

If your results differ but methodology is sound, that's acceptable.

---

## Deliverable Checklist

Your brief should include:

- [ ] Two-stage results comparison (GI-only vs GI+SAQ)
- [ ] Feature importance for both stages with business interpretation
- [ ] Risk segmentation framework (3-5 tiers with recommended actions)
- [ ] Fairness analysis (check performance across regions)
- [ ] Two visualizations: feature importance chart + calibration/comparison plot
- [ ] Business recommendations

---

## Common Mistakes to Avoid

❌ Random train/test split (data leakage)  
❌ Skipping GI-only analysis  
❌ Listing Q-codes without interpreting topics  
❌ No fairness check  
❌ Notebook-focused instead of brief-focused  

---

## Resources

**Technical Issues:** [TA Email]  
**Conceptual Questions:** Office hours [Times]  
**Grade Questions:** [Instructor Email]

**Reference:** Tan et al. (2024) "Stop Auditing and Start to CARE" - INFORMS Journal on Applied Analytics

---

## Academic Integrity

- Use dataset for coursework only
- Work independently
- Cite any external resources used
- Do not share code with classmates

---

**Good luck! Focus on the business insights, not just the models.**
