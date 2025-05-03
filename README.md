# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Patrick Hall, `jphall@gwu.edu` & N M Emran Hussain `nmemran.hussain@gwu.edu`
* **Model date**: May, 2025
* **Model version**: 1.0 
* **License**: [Apache License 2.0](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/LICENSE)
* **Model implementation code**: [Assignment_1](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_1_template.ipynb), [Assignment_2](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_2_template.ipynb), [Assignment_3](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_3_template_s_309.ipynb), [Assignment_4](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_4_template_final.ipynb), [Assignment_5](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_5_template.ipynb)
 
### Intended Use
* **Primary intended uses**: This model card documents a classification model built and remediated to improve fairness and reduce disparate impact, while maintaining predictive performance. The model is intended for use in decision-making scenarios where fairness is a key consideration, such as admissions, hiring, or lending. It is designed for use by machine learning practitioners and decision-makers seeking both interpretability and accountability in AI systems.
* **Out-of-scope use cases**: The model is not intended for real-time, high-stakes automated decision-making without human oversight. Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
| **term_360** |	input	| int (binary)	| 1 if loan term is 360 months (30 years), 0 otherwise |
| **conforming** |	input |	int (binary) |	1 if loan is conforming to GSE standards, 0 otherwise |
| **debt_to_income_ratio_missing** |	input	| int (binary) |	1 if debt-to-income ratio is missing, 0 otherwise |
| **loan_amount_std** |	input |	float	| Standardized loan amount |
| **loan_to_value_ratio_std** |	input	| float	| Standardized loan-to-value (LTV) ratio |
| **no_intro_rate_period_std** |	input	| float	| Standardized indicator for absence of introductory interest rate period |
| **intro_rate_period_std** |	input	| float	| Standardized duration of introductory interest rate period |
| **property_value_std** |	input	| float	| Standardized appraised property value |
| **income_std** |	input |	float	| Standardized applicant income
| **debt_to_income_ratio_std** |	input	| float	| Standardized applicant debt-to-income ratio |
| **high_priced** |	target	| int (binary) |	1 if loan is classified as high-priced under federal rules; 0 otherwise |

* **Source of training data**: [HMDA Trainning Datasets](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/hmda_train_preprocessed.zip)
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
* **Train data**: rows = 112253, columns = 23
* **Validation data**: rows = 48085, columns = 23
  
### Test Data
* **Source of test data**: [HMDA Test Datasets](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/hmda_test_preprocessed.zip)
* **Test Set**: 45043 rows, 23 columns
* **Any differences in columns between training and test data**: Test data dosn't have the 'y variable' and 'hih_priced'

### Model details
* **Columns used as inputs in the final model**: **'term_360'**, **'conforming'**, **'debt_to_income_ratio_missing'**, **'loan_amount_std'**, **'loan_to_value_ratio_std'**, **'no_intro_rate_period_std'**, **'intro_rate_period_std'**, **'property_value_std'**, **'income_std'**, **'debt_to_income_ratio_std'**, 
* **Column(s) used as target(s) in the final model**: **'high_priced'**
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: numpy: 2.0.2, pandas: 2.2.2, scikit-learn: 1.6.1, matplotlib: 3.10.0, seaborn: 0.13.2, Python 3.11.12
* **Hyperparameters or other settings of the model**: 
```
rem_params = {'max_bins': 512,
              'max_interaction_bins': 16,
              'interactions': 10,
              'outer_bags': 4,
              'inner_bags': 4,
              'learning_rate': 0.05,
              'validation_size': 0.5,
              'min_samples_leaf': 1,
              'max_leaves': 3,
              'n_jobs': 4,
              'early_stopping_rounds': 100,
              'random_state': 309}

rem_x_names = ['property_value_std',
               'conforming',
               'debt_to_income_ratio_missing',
               'income_std',
               'term_360',
               'no_intro_rate_period_std',
               'debt_to_income_ratio_std',
               'intro_rate_period_std']
```
### Quantitative Analysis

* Models were assessed primarily with AUC:

| Train AUC | Validation AUC | Test AUC |
| ------ | ------- | -------- |
| 0.7823 | 0.7836  | 0.7832* |

* Plots related with data & model

![basic data exploration](basic_data_exploration.jpg) 
Figure 1. Basic Data Exploration

![correlation map](correlation_map.jpg) 
Figure 2. Correlation Heatmap

![compare global features across models](Compare_global_feature_across_models.jpg) 
Figure 3. Global Featuress Across Models

![compare local features across models](Compare_local_feature_across_models.jpg) 
Figure 4. Local Features Across Models

![Plot Partial Difference for all features and models](plot_pd_1.jpg)
Figure 5. Plot partial dependence for all features and models

![Plot Partial Difference for all features and models](plot_pd_2.jpg)
Figure 5. Plot partial dependence for all features and models

![Plot Partial Difference for all features and models](plot_pd_3.jpg)
Figure 5. Plot partial dependence for all features and models

![Plot Partial Difference for all features and models](plot_pd_4.jpg)
Figure 5. Plot partial dependence for all features and models

![air_v_auc](AIR_v_AUC.jpg)
Figure 6. Model selection (AIR vs. AUC)

![model extration attack](visualization_simulated_data_for_extraction_attack.jpg)
Figure 7. Model Extraction Attack

![stolen model](example_of_stolen_model.jpg)
Figure 8. Stolen Model

![variable importancel](variable_importance.jpg)
Figure 9. Variable Importance

![sensitivity analysis](sensitivity_analysis.jpg)
Figure 10. Sensitivity Analysis

![residual analysis](global_logloss_residuals.jpg)
Figure 10. Residual Analysis

### Ethical Consideration
The best remediated model shows improved fairness and accuracy, but 'potential negative impacts' still remain. From a mathematical or software perspective, issues like underfitting due to excessive remediation (e.g., aggressive fairness constraints or data balancing) could degrade generalizability when deployed on unseen data, particularly if future data distributions shift (data drift). Furthermore, if preprocessing (e.g., one-hot encoding or imputation) introduces bias or is inconsistently applied across training and deployment pipelines, the model could misclassify sensitive applicants. 

In terms of real-world risks, the model’s outputs—likely related to credit or loan decisioning based on HMDA data—can affect individuals' financial opportunities. If the model inadvertently disadvantages subpopulations (e.g., low-income or minority groups) due to unaddressed indirect bias, borrowers could face unjust rejections or higher scrutiny, especially during high-stakes financial reviews (e.g., during home loan applications). These risks are more pronounced at the point of deployment by financial institutions and can erode trust or violate legal standards like the Fair Housing Act. Continuous monitoring and human-in-the-loop oversight are essential to ensure the model remains fair, accurate, and ethically sound over time.  

The impacts of using our group’s best remediated model are subject to several 'uncertainties' that must be acknowledged. From a mathematical and software standpoint, uncertainties arise from hyperparameter tuning variability, choice of fairness metrics, and the reliability of training data distributions. For instance, while the model may perform well on current data, its effectiveness could change if input features shift over time or if the underlying population changes (concept drift). Additionally, small bugs in preprocessing scripts or inconsistencies in feature scaling between training and deployment pipelines could subtly affect predictions without being immediately noticeable.  

On the real-world side, uncertainties are amplified by how and where the model is deployed. If adopted by financial institutions for loan approval decisions, the consequences of incorrect predictions could significantly impact individuals’ financial well-being. Uncertainty lies in who might be affected—applicants from historically marginalized groups may still face unintentional bias if proxy variables sneak in. The "when" and "how" depend on the institution's oversight: if model monitoring is irregular or remediation isn’t updated as new data flows in, it can lead to unfair or outdated decision-making. Moreover, regulatory changes or shifts in legal interpretations around fairness in AI could suddenly render a previously compliant model risky or non-compliant. These uncertainties highlight the need for ongoing validation, auditing, and transparent documentation in production use.

