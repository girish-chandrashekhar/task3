1. Import and Preprocess the Dataset
The dataset was loaded from the provided CSV string into a Pandas DataFrame.

Preprocessing steps:

Binary categorical features (mainroad, guestroom, basement, hotwaterheating, airconditioning, prefarea) were mapped to numerical values: 'yes' → 1, 'no' → 0.
The furnishingstatus feature (with values 'furnished', 'semi-furnished', 'unfurnished') was one-hot encoded into three dummy variables: furnish_furnished, furnish_semi-furnished, furnish_unfurnished.
The target variable is price; all other columns are features.
No missing values or outliers were handled explicitly, as the data appeared clean.



The preprocessed DataFrame has 545 rows and 15 columns (after encoding).
2. Split Data into Train-Test Sets
The data was split into training and testing sets using an 80/20 ratio:

Training set: 436 samples.
Testing set: 109 samples.
Random seed: 42 (for reproducibility).
Features (X): All columns except price.
Target (y): price.

3. Fit a Linear Regression Model
A multiple linear regression model was fitted using sklearn.linear_model.LinearRegression on the training data. The model uses all preprocessed features to predict price.
4. Evaluate Model Using MAE, MSE, R²
The model was evaluated on the test set:

Mean Absolute Error (MAE): 970,043.40 (average absolute difference between predicted and actual prices).
Mean Squared Error (MSE): 1,754,318,687,330.66 (average squared difference; sensitive to larger errors).
R² Score: 0.653 (explains about 65.3% of the variance in house prices; indicates a moderate fit).

To arrive at these metrics:

Predictions were generated on the test set: y_pred = model.predict(X_test).
MAE = (1/n) * Σ |y_test - y_pred|
MSE = (1/n) * Σ (y_test - y_pred)²
R² = 1 - (SS_res / SS_tot), where SS_res is the sum of squared residuals and SS_tot is the total sum of squares.

5. Plot Regression Line and Interpret Coefficients
Plot Regression Line
Since this is a multivariate model (multiple features), a single "regression line" cannot be plotted in 2D space. Instead, for visualization purposes, a simple linear regression was fitted using only the area feature (a common key predictor for house prices) on the same train/test split.
The equation of the regression line (from the simple model) is:
price = 2,512,254 + 426 × area

To arrive at the simple model:

Fit LinearRegression on X_area (just the 'area' column) and y.
Coefficient for area: 426 (slope; price increases by ~426 per unit increase in area).
Intercept: 2,512,254 (base price when area=0).



If plotted (e.g., using matplotlib scatter plot of area vs. price with the line overlaid):

Scatter points would show actual data.
The red dashed line would represent the fitted predictions: price = intercept + coefficient × area.
This line shows a positive linear trend, but with scatter due to other unaccounted factors.

For the full multivariate model, an "actual vs. predicted" scatter plot could be used (with a diagonal line for perfect fit), but it aligns moderately well based on the R².
Interpret Coefficients (from the Multivariate Model)
The coefficients represent the estimated change in price for a one-unit increase in each feature, holding all other features constant. The intercept is the base price when all features are zero.
















































































FeatureCoefficientInterpretationarea236Price increases by ~236 for each additional square foot of area.bedrooms76,779Price increases by ~76,779 for each additional bedroom.bathrooms1,094,445Price increases by ~1,094,445 for each additional bathroom (strong impact).stories407,477Price increases by ~407,477 for each additional story.mainroad367,920Houses on a main road are ~367,920 more expensive.guestroom231,610Houses with a guest room are ~231,610 more expensive.basement390,251Houses with a basement are ~390,251 more expensive.hotwaterheating684,650Houses with hot water heating are ~684,650 more expensive.airconditioning791,427Houses with air conditioning are ~791,427 more expensive (strong premium).parking224,842Price increases by ~224,842 for each additional parking space.prefarea629,891Houses in a preferred area are ~629,891 more expensive.furnish_furnished180,176Fully furnished houses are ~180,176 more expensive than the baseline.furnish_semi-furnished53,294Semi-furnished houses are ~53,294 more expensive than the baseline.furnish_unfurnished-233,469Unfurnished houses are ~233,469 less expensive than the baseline.

Intercept: 79,857 (theoretical base price with no features).
Overall Interpretation: Features like bathrooms, air conditioning, and hot water heating have the largest positive impact on price. Furnishing status shows a clear premium for furnished over unfurnished. The model assumes linearity and no multicollinearity (though some features like bedrooms and area may correlate).
