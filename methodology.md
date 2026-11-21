# Research Methodology
## 1. Refined Research Question
How can Grey Relational Analysis (GRA) and supervised machine learning models—specifically Artificial Neural Networks (ANN)—be integrated to predict and optimize the combined tensile, compressive, flexural, and impact strengths of Stereolithography (SLA) 3D-printed components across varying layer thicknesses, post-curing times, and build orientations?
## 2. Dataset Description
* **Source & Design:** Structured around a full-factorial / Taguchi-inspired experimental matrix for SLA photopolymer printing (3 factors × 3 levels with 3 replicates per condition, totaling **81 experimental specimens**).
* **Input Features (Process Parameters):**
  1. `layer_thickness_mm`: Layer height during photopolymerization (`0.025`, `0.050`, `0.100` mm).
  2. `post_curing_time_min`: UV post-curing duration after cleaning (`15`, `30`, `60` minutes).
  3. `build_orientation_deg`: Part inclination angle on the build platform (`0°` flat, `45°` inclined, `90°` upright).
* **Raw Mechanical Responses:**
  1. `tensile_strength_mpa`: Ultimate tensile strength (MPa, higher-is-better).
  2. `compressive_strength_mpa`: Compressive strength (MPa, higher-is-better).
  3. `flexural_strength_mpa`: Flexural/bending strength (MPa, higher-is-better).
  4. `impact_strength_kj_m2`: Izod/Charpy impact strength ($\text{kJ/m}^2$, higher-is-better).
* **Target Variable:**
  * `grey_relational_grade` (`GRG`, continuous scalar in $$): A unified multi-response performance index computed via Grey Relational Analysis (GRA) from the four normalized mechanical strength responses.
* **Limitations:** Because physical laboratory fabrication and destructive mechanical testing are scheduled for subsequent phases of the thesis, this initial computational experiment uses a physics-informed, literature-calibrated dataset simulating experimental variance and measurement noise.
## 3. Data Cleaning Plan
1. **Missing Value & Type Verification:** Inspect all columns for null entries, corrupted sensor readings, or non-numeric strings, enforcing strict floating-point data types.
2. **Physical Range & Outlier Filtering:** Verify that all process inputs match valid printer bounds and flag/handle extreme experimental measurement outliers using the Interquartile Range (IQR) method ($1.5 \times \text{IQR}$) per parameter group.
3. **Deduplication:** Check for accidental duplicate specimen IDs while preserving valid experimental replicates.
## 4. Feature Engineering Plan
1. **Grey Relational Analysis (GRA) Target Construction:**
   * Apply *larger-is-better* min-max normalization to each of the four mechanical responses: $x_i^*(k) = \frac{x_i(k) - \min x(k)}{\max x(k) - \min x(k)}$.
   * Compute Grey Relational Coefficients ($\xi_i(k)$) with distinguishing coefficient $\zeta = 0.5$, and average them across all four responses to obtain the unified target `grey_relational_grade`.
2. **Trigonometric Orientation Encoding:** Convert `build_orientation_deg` ($\theta$) into $\sin(\theta)$ and $\cos(\theta)$ components to capture directional layer-adhesion anisotropy.
3. **Domain Interaction Terms:** Create `curing_per_layer_ratio` ($\frac{\text{post\_curing\_time}}{\text{layer\_thickness}}$) and `curing_x_orientation` to model how UV penetration interacts with layer thickness and build angle.
4. **Feature Standardization:** Apply `StandardScaler` (zero mean, unit variance) fitted strictly on the training split to prevent data leakage and stabilize neural network gradient descent.
## 5. Models Selected and Rationale
1. **Artificial Neural Network (`MLPRegressor`):** As proposed in the study, a feedforward Multi-Layer Perceptron with ReLU activation and L2 regularization is used to capture the complex, nonlinear mappings between SLA curing parameters and multi-response mechanical performance.
2. **Random Forest Regressor (`RandomForestRegressor`):** Selected as a robust, tree-based ensemble benchmark that handles nonlinear interactions well on small-to-medium tabular experimental datasets without sensitivity to feature scaling.
## 6. Evaluation Metrics and Rationale
To align directly with Section 3.5 of the research proposal, models are evaluated on a held-out test set (20% split) using:
* **Coefficient of Determination ($R^2$):** Measures the proportion of variance in the Grey Relational Grade explained by the SLA process parameters.
* **Root Mean Squared Error (RMSE):** Penalizes larger prediction errors more heavily, ensuring reliability when identifying optimal printing parameters.
* **Mean Absolute Error (MAE):** Provides an intuitive average magnitude of prediction error in the same unit scale ($0\text{–}1$) as the Grey Relational Grade.
