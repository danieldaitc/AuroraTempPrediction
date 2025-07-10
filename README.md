# AuroraTempPrediction

7/10 insights

# Setup
We used the pretrained Aurora model to forecast daily maximum temperatures for major cities from June 15 to June 30. The model takes input in the form of a batch that includes:

Surface-level variables: 2t, u10, v10, msl
Atmospheric variables: t, u, v, q, z
Static variables: z, slt, lsm (elevation, slope, land-sea mask)

Since Aurora forecasts globally, we cropped the inputs to a 4°×4° tile (±2°) around each city. This localizes the forecast but may reduce accuracy; better regional adaptation is likely needed.

For prediction of a certain day, we used only data from the previous day.
“surf_tile["t2m"].values[:2][None]  # Only first 2 time steps”

We set the number of steps as six, so this is able to predict temperature up to 36 hours.
“preds = list(rollout(model, batch, steps=steps))  # Default steps=6 here”

From these, we extracted the maximum temperature per day and compared it to actual ERA5 values.

# Results

Observation:
The forecast and actual temperatures follow the same overall trends.
There’s a consistent gap between predicted and observed values.
The overall RMSE is 4.62 °C.

Notes:
Aurora’s prediction is generated every 6 hours. For more precise daily max estimates, we may need higher-resolution output (e.g., hourly).
Fine-tuning Aurora on regional data may be necessary to improve performance.


# Next Steps & Questions:
What training window should we use — past 2 weeks or full month?
Should we optimize the model for short-term (days) or longer-term (weeks/months) forecasts?
Should we begin building batches from region-specific datasets instead of cropped global tiles?
Is fine-tuning the Aurora model necessary at this stage?

