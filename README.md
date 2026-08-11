# Moscow New-Building Price Analysis

Statistical analysis of price per square meter in Moscow new-build residential listings, with a focus on location, housing class, district effects, and price dynamics over time.

**Links:** [Kaggle notebook](https://www.kaggle.com/code/tonyryzhkov/moscow-new-building-price-analysis) · [Source dataset](https://www.kaggle.com/datasets/sergionefedov/moscow-real-estate-sales-and-rentals-20202026)

## Project overview

The project analyzes **8,000 Moscow new-building listings** and investigates how residential prices are associated with several market characteristics.

The analysis is structured around four main questions:

1. **How does the price per square meter change with distance from the city center?**
2. **How do housing classes differ in price across central and peripheral locations?**
3. **Do housing classes retain their price differences after accounting for district and distance from the center?**
4. **Do monthly price dynamics differ across housing classes?**

The final stage also examines model diagnostics and identifies market segments where prediction errors are larger.

## Data

The original Kaggle dataset covers the Moscow real-estate market from **2020 to 2026** and includes secondary-market sales, new construction, and rental listings across Moscow districts.

For this project, the analysis is restricted to the **new-building segment**.

Key variables used in the analysis include:

- `price_per_sqm` — price per square meter;
- `to_center_km` — distance from the city center;
- `district` — Moscow district;
- `complex_class` — housing class;
- `date_posted` — listing date.

Housing classes are represented by four segments:

- economy;
- comfort;
- business;
- premium.

## Analysis workflow

The project combines exploratory analysis with statistical inference and regression modeling.

### 1. Exploratory data analysis

- inspected data quality and variable distributions;
- analyzed price per square meter and potential outliers;
- compared housing classes and districts;
- examined the relationship between price and distance from the city center;
- explored price dynamics over time.

### 2. Segment comparison

Price differences were examined across housing classes and location segments using:

- non-parametric comparisons;
- bootstrap confidence intervals;
- distributional and graphical analysis.

This step was used to determine whether observed differences were substantial before introducing regression controls.

### 3. Multiple linear regression

A multiple OLS regression model was used to separate the associations of location, housing class, district, and time.

The final specification can be summarized as:

```text
price_per_sqm
    ~ log(1 + distance_to_center)
    + district
    + month
    + housing_class
    + month × housing_class
```

The logarithmic transformation of distance captures the nonlinear relationship between distance from the center and price.

The interaction between month and housing class allows each housing segment to have its own price trend.

### 4. Model diagnostics

The final model was evaluated using:

- residual analysis;
- heteroskedasticity diagnostics;
- Variance Inflation Factor (VIF);
- error comparison across housing classes and distance segments.

Because the residual variance is not constant, **HC3 heteroskedasticity-robust standard errors** are used for statistical inference.

## Key findings

### Distance from the city center

Price per square meter generally decreases as distance from the center increases.

The relationship is nonlinear: differences are more pronounced closer to the center and become less steep at larger distances. A logarithmic distance specification therefore describes the observed pattern better than a simple linear relationship.

### Housing class and location

Prices increase systematically from economy and comfort housing toward business and premium segments.

However, the distributions overlap substantially. Housing class alone does not determine price: location remains an important source of variation within every class.

### Housing class after controlling for location

Housing-class differences remain after accounting for both district and distance from the city center.

This suggests that housing class contains information about price that cannot be explained solely by where a property is located.

At the same time, district effects remain relevant even after controlling for distance, indicating that location cannot be reduced to a single center-distance variable.

### Price dynamics over time

Monthly price dynamics are not identical across housing classes.

The interaction between time and housing class allows the model to compare these class-specific trends rather than impose a common market trend on all segments.

Adjusted trend plots are therefore useful primarily for **comparing housing classes**, not as unconditional forecasts of the overall Moscow market.

### Model diagnostics

Residual analysis shows that model errors are not distributed uniformly across the market.

The model captures broad structural relationships well enough for explanatory analysis, but individual listings can still deviate substantially from the fitted values. This is especially important when interpreting results for specific market segments.

For this reason, the model should be viewed primarily as an **explanatory model of price structure**, rather than as a precise property-valuation or long-term forecasting model.

## Main limitations

- The dataset contains observational listing data, so the results describe **associations rather than causal effects**.
- Listing prices do not necessarily equal final transaction prices.
- The model includes a limited set of property and location characteristics; unobserved factors may explain part of the remaining price variation.
- District and distance from the center capture different aspects of location and are partly related to each other.
- Adjusted time trends depend on the model specification and should not be interpreted as unconditional forecasts of future prices.

## Tools

- Python
- pandas
- NumPy
- Matplotlib / Seaborn
- SciPy
- statsmodels

## Full notebook

The complete analysis, including code, statistical output, visualizations, diagnostics, and detailed interpretation, is available on Kaggle:

**[Moscow New-Building Price Analysis](https://www.kaggle.com/code/tonyryzhkov/moscow-new-building-price-analysis)**

## Data source

Original dataset:

**[Moscow Real Estate: Sales & Rentals (2020–2026)](https://www.kaggle.com/datasets/sergionefedov/moscow-real-estate-sales-and-rentals-20202026)**
