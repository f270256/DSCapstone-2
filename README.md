# Kickstarter Campaign Success Prediction

## Project Overview
This project is about analyzing Kickstarter campaigns and making a binary classification model to predict if a project will be **successful** or **unsuccessful** by using information available at the launch time.

The full workflow includes data cleaning, exploratory data analysis, feature engineering, preprocessing, and predictive modeling.

## Business Problem
Creators and platform teams need a practical way to estimate campaign success before the campaign starts. A prediction model before launch can help to find risky campaigns, set more realistic expectation, and support better decisions like changing funding goal or improving launch timing.

## Dataset
- Source file: `ks-projects-201801.csv`
- Final cleaned dataset size: **372,066 projects**
- Outcome distribution: **36.0% successful** and **64.0% unsuccessful**

## Data Preparation
Main preparation steps were:
- Removed records with non-final states such as `live` and `undefined`
- Removed malformed country values
- Created `duration` from launch and deadline timestamps
- Created `launch_year`, `launch_month`, and `launch_weekday`
- Removed post-launch leakage variables such as `pledged`, `usd_pledged_real`, and `backers`
- Applied one-hot encoding for categorical features and scaling for numeric features

## Exploratory Findings
- Successful projects usually had **lower funding goals**. The median goal for successful campaigns was around **$3,840**, while unsuccessful campaigns had around **$7,702**.
- Success rate changed a lot by category. The strongest categories in this dataset were **Dance (62.4%)**, **Theater (60.1%)**, and **Comics (54.4%)**.
- Timing also showed some effect, but it was less strong than goal size and category. **March** had the highest observed success rate (**38.2%**), and **July** had the lowest (**32.4%**).

## Modeling Approach
The models tested in this project were:
- Logistic Regression
- Decision Tree (Gini)
- Decision Tree (Entropy)
- Random Forest with GridSearchCV tuning

The evaluation was done on a stratified 80/20 train-test split. **F1 score** was used as the main metric because the classes are imbalanced and accuracy only is not enough.

## Model Performance

| Model | Accuracy | Precision | Recall | F1 Score |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.689 | 0.607 | 0.388 | 0.473 |
| Decision Tree (Gini) | 0.687 | 0.602 | 0.382 | 0.467 |
| Decision Tree (Entropy) | 0.686 | 0.607 | 0.365 | 0.456 |
| **Random Forest (Tuned)** | **0.696** | **0.601** | **0.457** | **0.520** |

## Final Model
The final selected model was a **tuned Random Forest** with these parameters:
- `n_estimators = 200`
- `max_depth = None`
- `min_samples_split = 5`
- `min_samples_leaf = 1`

The most important features from the final model were:
- `usd_goal_real`
- `duration`
- `launch_month`
- `launch_weekday`
- `launch_year`

## Recommendations
1. **Set more realistic funding goals.** Goal size was the strongest predictor, and successful projects usually asked for much lower targets.
2. **Keep campaign duration in a reasonable range.** In this dataset, successful campaigns were often shorter, and many of them were close to 30 days.
3. **Use category and timing when planning launch.** Some categories performed much better than others, and some launch periods also showed better results.

## Project Files
- `data_wrangling.ipynb`
- `exploratory_data_analysis.ipynb`
- `pre_processing.ipynb`
- `modeling.ipynb`
- `model_metrics.txt`
- `Capstone_Final_Report.pdf`
- `Capstone2_Kickstarter_Fahad_Memon.pptx`

## Next Steps
Possible future improvements are:
- Add NLP features from project titles and descriptions
- Test boosted tree models such as XGBoost or LightGBM
- Tune classification threshold for better recall and precision balance
- Check calibration and year-based validation split
