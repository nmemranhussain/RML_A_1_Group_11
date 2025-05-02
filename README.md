# Credit Line Increase Model Card

### Basic Information

* **Person or organization developing model**: Patrick Hall, `jphall@gwu.edu` & N M Emran Hussain `nmemran.hussain@gwu.edu`
* **Model date**: May, 2025
* **Model version**: numpy: 2.0.2, pandas: 2.2.2, scikit-learn: 1.6.1, matplotlib: 3.10.0, seaborn: 0.13.2, Python 3.11.12
* **License**: MIT
* **Model implementation code**: [Assignment_1](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_1_template.ipynb), [Assignment_2](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_2_template.ipynb), [Assignment_3](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_3_template_s_309.ipynb), [Assignment_4](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_4_template_final.ipynb), [Assignment_5](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/assign_5_template.ipynb)
 
### Intended Use
* **Primary intended uses**: This model card documents a classification model built and remediated to improve fairness and reduce disparate impact, while maintaining predictive performance. The model is intended for use in decision-making scenarios where fairness is a key consideration, such as admissions, hiring, or lending. It is designed for use by machine learning practitioners and decision-makers seeking both interpretability and accountability in AI systems.
* **Out-of-scope use cases**: The model is not intended for real-time, high-stakes automated decision-making without human oversight. Any use beyond an educational example is out-of-scope.

### Training Data

* Data dictionary: 

| Name | Modeling Role | Measurement Level| Description|
| ---- | ------------- | ---------------- | ---------- |
|**ID**| ID | int | unique row indentifier |
| **LIMIT_BAL** | input | float | amount of previously awarded credit |
| **SEX** | demographic information | int | 1 = male; 2 = female
| **RACE** | demographic information | int | 1 = hispanic; 2 = black; 3 = white; 4 = asian |
| **EDUCATION** | demographic information | int | 1 = graduate school; 2 = university; 3 = high school; 4 = others |
| **MARRIAGE** | demographic information | int | 1 = married; 2 = single; 3 = others |
| **AGE** | demographic information | int | age in years |
| **PAY_0, PAY_2 - PAY_6** | inputs | int | history of past payment; PAY_0 = the repayment status in September, 2005; PAY_2 = the repayment status in August, 2005; ...; PAY_6 = the repayment status in April, 2005. The measurement scale for the repayment status is: -1 = pay duly; 1 = payment delay for one month; 2 = payment delay for two months; ...; 8 = payment delay for eight months; 9 = payment delay for nine months and above |
| **BILL_AMT1 - BILL_AMT6** | inputs | float | amount of bill statement; BILL_AMNT1 = amount of bill statement in September, 2005; BILL_AMT2 = amount of bill statement in August, 2005; ...; BILL_AMT6 = amount of bill statement in April, 2005 |
| **PAY_AMT1 - PAY_AMT6** | inputs | float | amount of previous payment; PAY_AMT1 = amount paid in September, 2005; PAY_AMT2 = amount paid in August, 2005; ...; PAY_AMT6 = amount paid in April, 2005 |
| **DELINQ_NEXT**| target | int | whether a customer's next payment is delinquent (late), 1 = late; 0 = on-time |

* **Source of training data**: [HMDA Trainning Datasets](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/hmda_train_preprocessed.zip)
* **How training data was divided into training and validation data**: 50% training, 25% validation, 25% test
* **Number of rows in training and validation data**:
  * Training Set: 90084 rows, 23 columns
  * Validation Set: 45042 rows, 23 columns

### Test Data
* **Source of test data**: [HMDA Test Datasets](https://github.com/nmemranhussain/RML_A_1_Group_11/blob/main/hmda_test_preprocessed.zip)
* **Test Set**: 45043 rows, 23 columns
* **State any differences in columns between training and test data**: None

### Model details
* **Columns used as inputs in the final model**: 'LIMIT_BAL',
       'PAY_0', 'PAY_2', 'PAY_3', 'PAY_4', 'PAY_5', 'PAY_6', 'BILL_AMT1',
       'BILL_AMT2', 'BILL_AMT3', 'BILL_AMT4', 'BILL_AMT5', 'BILL_AMT6',
       'PAY_AMT1', 'PAY_AMT2', 'PAY_AMT3', 'PAY_AMT4', 'PAY_AMT5', 'PAY_AMT6'
* **Column(s) used as target(s) in the final model**: 'DELINQ_NEXT'
* **Type of model**: Decision Tree 
* **Software used to implement the model**: Python, scikit-learn
* **Version of the modeling software**: 0.22.2.post1
* **Hyperparameters or other settings of your model**: 
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

* Models were assessed primarily with AUC and AIR. See details below:

| Train AUC | Validation AUC | Test AUC |
| ------ | ------- | -------- |
| 0.3456 | 0.7891  | 0.7687* |

Table 1. AUC values across data partitions. 

| Group | Validation AIR |
|-------|-----|
| Black vs. White | 0.8345 |
| Hispanic vs. White | 0.8765 |
| Asian vs. White | 1.098 |
| Female vs. Male | 1.245 |

Table 2. Validation AIR values for race and sex groups. 

(**HINT**: Test AUC taken from https://github.com/jphall663/GWU_rml/blob/master/assignments/model_eval_2023_06_21_12_52_47.csv)

#### Correlation Heatmap

![Correlation Heatmap](download.png)

Figure 1. Correlation heatmap for input features. 

