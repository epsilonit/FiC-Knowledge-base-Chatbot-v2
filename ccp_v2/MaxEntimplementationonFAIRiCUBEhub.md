MaxEnt implementation on FAIRiCUBE hub
We aim to find out what is the best possible solution to run MaxEnt model (https://biodiversityinformatics.amnh.org/open_source/maxent/) on the FAIRiCUBE hub. We check the functionality of different python and R packages, to find alternative to GUI based software with working code. Overview and new inputs (please add any known if not included) are in the table summary at the following link: https://nilu365.sharepoint.com/:x:/r/sites/Horizon2021_CUBE/Shared%20Documents/WP2%20-%20use/common/models/maxent/MaxEnt%20packages%20comparison.xlsx?d=w6eca6bd3de2c42d1a62012dce60b00d9&csf=1&web=1&e=Wdspbu
This is already an Issue:  `Add Java JVM and MaxEnt model application to EOXHub`  https://github.com/FAIRiCUBE/FAIRiCUBE-Hub-issue-tracker/issues/36

I thought about more code/packed focused issue here but I don't mind if both issues are merged. In case it would be needed could an R kernel be added to the Jupyter environment? Yes, that should not  be a problem. Jupyther-Lab works quite well with R
Let me know when you will need it

HI thanks for providing the overview. We are going to test the Python libraries next week and provide feedback
Dear all,

I have now tested a very basic example using the python library **Elapid** for Maxent modeling.
A detailed tutorial can be found [here](https://earth-chris.github.io/elapid/sdm/maxent/).
I have also implemented and tested a simple example which you can find [here](https://github.com/FAIRiCUBE/uc1-urban-climate/blob/master/processing/Hello_Maxent_Elapid.py). 

Here is a brief overview of the code:
```python
import numpy as np
import pandas as pd
import elapid
from sklearn.metrics import roc_auc_score

# Step 1: Generating synthetic data
presence_data = pd.DataFrame({
    'longitude': [10.0, 12.1, 13.5, 11.2, 14.3],
    'latitude': [60.1, 61.0, 59.9, 60.5, 61.2],
    'temperature': [15, 18, 16, 14, 19],
    'precipitation': [300, 400, 350, 450, 500]
})

background_data = pd.DataFrame({
    'longitude': np.random.uniform(10, 15, 100),
    'latitude': np.random.uniform(59, 62, 100),
    'temperature': np.random.uniform(10, 20, 100),
    'precipitation': np.random.uniform(200, 600, 100)
})

# Step 2: Combine data and create labels
presence_labels = np.ones(len(presence_data))
background_labels = np.zeros(len(background_data))
combined_data = pd.concat([presence_data, background_data], ignore_index=True)
labels = np.concatenate([presence_labels, background_labels])

# Features (environmental variables)
features = combined_data[['temperature', 'precipitation']]

# Step 3: Initialize and train MaxEnt model
model = elapid.MaxentModel()
model.fit(features, labels)

# Step 4: Make predictions for new environmental data
new_data = pd.DataFrame({
    'temperature': [17, 15, 12],
    'precipitation': [350, 420, 480]
})

predictions = model.predict(new_data)
print("Predicted probabilities:", predictions)

# Step 5: Evaluate the model (optional)
predicted_probabilities = model.predict(features)
auc_score = roc_auc_score(labels, predicted_probabilities)
print(f"Model AUC: {auc_score}")

```
## Summary:
Running the model looked straightforward. 
You should provide the **features** and **labels** dataframe as input (Step 1), create and train the mode (Step 3), and can then predict the probability of species presence for unseen data (Step 4).

In MaxEnt, **presence data** is labeled as 1 and **background data** (not necessarily absence) is labeled as 0. Background data represents locations where species presence is unknown.

The elapid library is very close to the R implementation. It is slightly different from the Java implementation version, but mainly in terms of default parameters setting and not the core implementation of the model.
 
Note that you can also use a **feature transformation** step that is optional but can be useful:
```python
model = elapid.MaxentModel(feature_types=["linear"])
featuresTransform = elapid.MaxentFeatureTransformer()
z = featuresTransform.fit_transform(features)
model.fit(z, labels)
```

Finally, the following are the **parameters** that you can customize when creating the model:

```python
model = elapid.MaxentModel(
    feature_types = ['linear', 'hinge', 'product'], # the feature transformations
    tau = 0.5, # prevalence scaler
    clamp = True, # set covariate min/max based on range of training data
    scorer = 'roc_auc', # metric to optimize (from sklearn.metrics.SCORERS)
    beta_multiplier = 1.5, # regularization scaler (high values drop more features)
    beta_lqp = 1.0, # linear, quadratic, product regularization scaler
    beta_hinge = 1.0, # hinge regularization scaler
    beta_threshold = 1.0, # threshold regularization scaler
    beta_categorical = 1.0, # categorical regularization scaler
    n_hinge_features = 10, # number of hinge features to compute
    n_threshold_features = 10, # number of threshold features to compute
    convergence_tolerance = 1e-07, # model fit convergence threshold
    use_lambdas = 'best', # set to 'best' (least overfit), 'last' (highest score)
    n_cpus = 4, # number of cpu cores to use
)
```
The detailed description of each parameter is the following:
- **`feature_types = ['linear', 'hinge', 'product']`**:
  - Specifies the types of transformations applied to the environmental data.
  - **Linear**: Direct relationships between variables (e.g., temperature).
  - **Hinge**: A function that lets the model handle more flexible, threshold-based responses.
  - **Product**: Interactions between different variables (e.g., temperature × precipitation).

- **`tau = 0.5`**:
  - The prevalence scaler: Determines how common the species is assumed to be in the study area. A value of 0.5 means the model assumes the species is equally likely to be present or absent.

- **`clamp = True`**:
  - Clamping restricts the predicted values of environmental variables to the range found in the training data. If a new input falls outside that range, it is "clamped" to the minimum or maximum value from the training data.

- **`scorer = 'roc_auc'`**:
  - The metric used to evaluate model performance. In this case, it’s ROC AUC, a common measure that indicates how well the model can distinguish between presence and background data.

- **`beta_multiplier = 1.5`**:
  - A regularization scaler that controls how much to penalize complex models (higher values reduce overfitting by dropping less important features).

- **`beta_lqp = 1.0`**:
  - Controls regularization specifically for linear, quadratic, and product features. A value of 1.0 means regularization is applied at the default level.

- **`beta_hinge = 1.0`**:
  - Regularization scaler for hinge features. Adjusts how much the hinge features are penalized. Higher values will smooth out the predictions.

- **`beta_threshold = 1.0`**:
  - Regularization scaler for threshold features, which create stepwise functions in the model. Controls how strictly these features are penalized.

- **`beta_categorical = 1.0`**:
  - Regularization scaler for categorical variables (features that are discrete categories, like habitat type). Higher values would drop less important categories.

- **`n_hinge_features = 10`**:
  - The number of hinge features the model will compute. More hinge features allow for more flexible decision boundaries but can lead to overfitting.

- **`n_threshold_features = 10`**:
  - The number of threshold features (stepwise functions) to compute. Similar to hinge features, they can introduce flexibility but too many may lead to overfitting.

- **`convergence_tolerance = 1e-07`**:
  - The convergence threshold: A very small value that tells the model when to stop training. The model will keep training until the improvement in predictions becomes smaller than this value.

- **`use_lambdas = 'best'`**:
  - Chooses the lambda (regularization strength) to use. 'best' selects the value that gives the best model performance without overfitting, while 'last' uses the highest-scoring lambda (which may overfit).

- **`n_cpus = 4`**:
  - The number of CPU cores to use for parallel computation. Using more cores speeds up model training.


Best,

-Bachir.

Dear all,

FYI, I have also tested [intros-MaxEnt](https://pypi.org/project/intros-MaxEnt/#files) and I would not recommend using it.
The documentation is lacking or difficult to find.
I had to manually edit a python file within the library to make it work.

Moreover, there is little flexibility in how inputs are passed to the model. The library seems to require CSV files in a specific structure, but it's not clearly documented. For example, it's unclear whether the presence CSV should only include presence data or if it should also contain background data. Even when I used a simple randomly generated data sample (see the code below), the model remained in an "empty" state at the end.

``````python
from introsmaxent.model import MaxEntModel
# Initialize the MaxEntModel
model = MaxEntModel()

# Set input files (replace with actual paths to your data)
model.setinputfiles('env_features.csv', 'env_data.csv', 'presence_data.csv')

# Optionally set model parameters
model.setprobmodel(beta=0.5, tau=0.5)
model.setexecmodel(iters=1000, cnvg=1e-5)

# Consume the data
model.consume()
# Compute the model
model.compute()
# Save the results to a CSV (optional)
model.representcsv()
``````
Given this humble experience, I recommend using Elapid, which is simple and straightforward (see comment above). 
However, we still need to validate it on real datasets to assess its performance.

Best regards,

-Bachir.