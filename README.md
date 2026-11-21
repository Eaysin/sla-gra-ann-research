# Multi-Response Optimization and Prediction of SLA 3D Printing Parameters Using GRA–ANN
This repository contains the literature review, experimental design, computational modeling notebook, research paper, and presentation slides for optimizing Stereolithography (SLA) 3D printing parameters.
## Repository Contents
* **Research Paper:** [`paper.md`](paper.md) — Complete research manuscript covering abstract, introduction, methodology, results, discussion, conclusion, and references.
* **Presentation Deck:** [`presentation.pptx`](presentation.pptx) — 6-slide summary covering the problem statement, GRA–ML methodology, results, and key takeaways.
* **Experiment Notebook:** [`experiment.ipynb`](experiment.ipynb) — Executable Jupyter Notebook containing data cleaning, Grey Relational Analysis (GRA), feature engineering, and model evaluation (ANN vs. Random Forest).
* **Methodology:** [`methodology.md`](methodology.md) — Detailed experimental design, feature engineering plan, and evaluation metrics.
* **Literature Review:** [`literature_review.md`](literature_review.md) — Summaries of key peer-reviewed studies on SLA parameter optimization and GRA–ANN modeling.
* **Figures:** [`figures/`](figures/) — High-resolution Matplotlib/Seaborn visualizations used in the paper and presentation.
## Key Results Summary
* **Optimal SLA Parameters:** `0.025 mm` layer thickness, `60 min` UV post-curing time, and `0°` build orientation maximized the composite Grey Relational Grade (`0.8312`).
* **Best Predictive Model:** Random Forest Regressor achieved **$R^2 = 0.9918$** ($\text{RMSE} = 0.0104$, $\text{MAE} = 0.0088$) on the test set, outperforming the feedforward ANN baseline ($R^2 = 0.7173$).
