# Princeton-CLSJ-PBM-Project

## Model 1 - Unaltered Model

For our baseline model we will use historical prescendent in cases as the primary driver of our prediction. The data used to track historical treatment is available at the ["Cook County District Attorney Site"](https://datacatalog.cookcountyil.gov/browse?tags=state%27s+attorney+case-level&sortBy=most_accessed). To make a prediction we build an XGBoost model using only cases where the crime charged was in the Retail Theft category.

## Model 2 - Convert race of individuals to White

In our second model, we alter race and covariates of race in order to examine how these may alter the treatment of specific cases.

## Model 2 - Suggestive Model v2

This is additional code that can be used to experiment with the suggestive model. Download "revised model 3.Rmd" and "blackandwhite.csv"

## Model 3 - Race Agnostic Model

For the race agnostic model we utilize the method developed by Yahav Bechavod and Katrina Ligett in ["Penalizing Unfairness in Binary Classification"](https://arxiv.org/abs/1707.00044). Predictions with fitted model can be run immediately in `race_agnostic_model.Rmd`. To retrain model follow the instructions below.

### Training

The code to train this model and find hyperparameters is automated in the following github repository. Dependencies are automatically downloaded following the instructions in their README.

    `$ git clone https://github.com/jjgold012/lab-project-fairness.git`

To train a new model with our Chicago dataset we provide code to create the neccesary inputs to run their method. These include a reformated training data set and a json file which outlines options for their method to use, such as target variable, features, and protected group variable.

    `$ python model3_preprocess.py train.csv test.csv`

Three files will be created, `train_processed.csv`, `test_processed.csv`, and `chicago.json`. Two of the files must be moved to run the fairness method. Model predictions will be made using `test_processed.csv` in the Princeton-CLSJ-PBM-Project directory.

    `$ cp train_processed.csv ~/lab-project-fairness/fairness_project/datasets/`
    `$ cp chicago.json ~/lab-project-fairness/fairness_project/options/`

Run the training proceedure.

    `$ python ~/lab-project-fairness/fairness_project/start.py ~/lab-project-fairness/fairness_project/options/chicago.json > chicago.result`

The method will split the training set via 5-fold cross validation to suggest the best hyperparameters to be used, and returns many outputs which includes the best weight vector used in the trained logistic regression function. Note, test_processed.csv is not being used in the 5-fold cross validation.


### Testing

Fitted weights can be found near the end of the result file under the header "Best Values for Objective squared relaxation" with key "w". Unfortuneately, the authors do not provide an automated way to retrieve or parse results. Copy weights from `chicago.result` to `race_agnost_modelweights.csv`

To predict on `train_processed.csv` run code in `race_agnostic_model.Rmd`.
