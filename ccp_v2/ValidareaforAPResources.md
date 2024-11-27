Valid area for A/P Resources
Based on preliminary experience with ML, we've had the insight that a model trained on a specific area (both spatial and temporal) may not be valid in other places and times. Determining exactly where/when a model is valid seems to still be an issue under ML. But, providing some basic information would be valuable for the A/P Resource metadata.
Hi in the a/p resources metadata there is a 'Input data used' element which must contains a link to the metadata of the data used in training (ML or DL). That's where one should/can find the spatio-temporal information.
In addition, in a/p resources metadata comments of this kind can be written in the field dedicated to the description of the output.
Do you think there is a need to add a specific field?
Perhaps a specific field can be added to highlight the required localisation of the model, or the expected needed retraining to deal with e.g. data drift (after the model was trained)?
good discussion! What information should these fields contain? I'd understood the `Input data` links to describe what data was used to train the model, not for what data the model is valid on. To my view, these are 2 separate concepts:
- Input Data: data utilized in the training of the model
- ???: data on which the model can be utilized what fields do you see required? From what I've picked up, location in space and time is one bit. How would one describe the retraining requirements? 

Also trying to understand what you're describing with data drift - the underlying data changing over time, thus requiring retraining of the model with the updated data?
For ML experts knowing the input data should be sufficient. For non-experts there could be some remarks (in a text field?) about the expected robustness and localisation requirements of the trained model. Mostly to create awareness.

Data drift is indeed as you describe Besides that there is also model (or concept) drift, where the task the model was trained for changes over time. In operational systems both are usually constantly measured/evaluated (easiest for data drift) to avoid model predictions becoming too inaccurate.