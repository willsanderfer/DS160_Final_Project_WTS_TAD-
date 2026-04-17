# DS160_Final_Project_WTS_TAD

**DS160-01 Introduction to Data Science, Final Project**
*Bellarmine University*

## Predicting Total Enrollment at U.S. Higher Education Institutions

### Abstract

We used the 2013 IPEDS dataset of 1,534 U.S. higher education institutions to build a multiple linear regression model that predicts total enrollment. The target variable is heavily right-skewed, so we took the log of enrollment before fitting any models. We tested two different feature sets (one with just numeric variables and one that also included two categorical variables) on two train/test splits, for four experiments total. Our best model used six numeric predictors plus Control of Institution and Highest Degree Offered with a 70/30 split. It got an R² of 0.78 on the held-out test data. The Highest Degree Offered variable turned out to matter the most. Taking the log of the target, leaving out any variables that would leak the answer, and adding the two categorical variables were the three things that helped our model the most.

---

## Team

- **Will Sanderfer**, wsanderfer@bellarmine.edu
- **Tad Dunkle**, tdunkle@bellarmine.edu

## Repository Contents

```
DS160_Final_Project_WTS_TAD/
├── README.md                                  This file
├── DS160_Final_Project_Paper.docx             Final paper (Word)
├── DS160_Final_Project_Paper.pdf              Final paper (PDF)
├── DS160_Final_Project.ipynb                  Analysis notebook
├── DS160_Final_Project_Presentation.pptx      Class presentation
├── IPEDS_data.xlsx                            Source dataset
├── figures/                                   Generated figures
│   ├── fig1_target_dist.png
│   ├── fig2_corr_heatmap.png
│   ├── fig3_boxplots.png
│   ├── fig4_scatter.png
│   └── fig5_pred_vs_actual.png
├── results_table.csv                          Experiment results
└── coefficients.csv                           Best-model coefficients
```

## Dataset

The dataset is the 2013 Integrated Postsecondary Education Data System (IPEDS) snapshot from the National Center for Education Statistics. It covers 1,534 U.S. higher education institutions and 145 variables. Columns follow IPEDS naming conventions:

- `IC2013.*` for institutional characteristics and admissions
- `HD2013.*` for header data (location, control, classification)
- `SFA1213.*` for student financial aid
- `EF2013.*` for enrollment

## What We Did

1. **Target transformation.** We applied a log1p transform to the target `Total enrollment`. The raw skewness was 2.49, which dropped to 0.07 after the transform.
2. **Avoided data leakage.** We excluded every enrollment-component variable (undergraduate, graduate, full-time, part-time) from all feature sets so the model could not cheat by using pieces of the target.
3. **Two feature sets:**
   - **Set A:** Applicants, % Admitted, Tuition, 6-year Graduation Rate (4 numeric predictors)
   - **Set B:** Set A + SAT Math 75p + % Pell + Control of institution + Highest degree offered
4. **Four experiments** crossing feature set with 80/20 and 70/30 train/test splits.
5. **Model:** `scikit-learn` `LinearRegression` with `StandardScaler` fit on the training data only.

## Results

| Experiment | Features | Split | R² test | RMSE (log) |
|------------|----------|-------|---------|-----------|
| Exp 1 | Set A (4 numeric) | 80 / 20 | 0.588 | 0.698 |
| Exp 2 | Set A (4 numeric) | 70 / 30 | 0.613 | 0.693 |
| Exp 3 | Set B (6 num + 2 cat) | 80 / 20 | 0.776 | 0.538 |
| **Exp 4 (best)** | **Set B (6 num + 2 cat)** | **70 / 30** | **0.780** | **0.538** |

The best model's biggest standardized coefficients are for `Highest degree offered = Doctoral` (+0.59), `Applicants total` (+0.44), and `Control = Public` (+0.38). Institutional type turned out to matter more than any numeric predictor.

## How to Run

Requirements: Python 3.10+, Jupyter, `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`, `openpyxl`.

```bash
pip install pandas numpy scikit-learn matplotlib seaborn openpyxl jupyter
jupyter notebook DS160_Final_Project.ipynb
```

Run all cells top to bottom. The notebook reads `IPEDS_data.xlsx` from the repository root.

## Tools Used

- **Python 3.11** with pandas 2.1, NumPy 1.26, Matplotlib 3.8, Seaborn 0.13, and Scikit-learn 1.4
- **Jupyter Notebook** for analysis and documentation
- **Microsoft Word / PowerPoint** for the paper and presentation

## References

- National Center for Education Statistics. (2013). *Integrated Postsecondary Education Data System (IPEDS)*. https://nces.ed.gov/ipeds/
- Pedregosa, F., et al. (2011). Scikit-learn: Machine Learning in Python. *JMLR*, 12, 2825 to 2830.
- McKinney, W. (2010). Data Structures for Statistical Computing in Python. *SciPy Proceedings*, 56 to 61.
