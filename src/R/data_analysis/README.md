# Data Analysis Module (R)

## Overview

The R data analysis module provides comprehensive statistical analysis capabilities for epidemiological data in the Next Pandemic Game. This module leverages R's powerful statistical computing environment to perform advanced analytics, hypothesis testing, and exploratory data analysis on disease surveillance and simulation data.

## Structure

```
src/R/data_analysis/
├── README.md
├── DESCRIPTION              # Package metadata and dependencies
├── NAMESPACE               # Package namespace definitions
├── R/
│   ├── data_import.R       # Data loading and validation functions
│   ├── descriptive_stats.R # Summary statistics and distributions
│   ├── hypothesis_testing.R # Statistical significance testing
│   ├── regression_analysis.R # Linear and non-linear regression
│   ├── time_series_analysis.R # Temporal pattern analysis
│   ├── survival_analysis.R  # Time-to-event analysis
│   ├── clustering.R        # Unsupervised learning methods
│   └── utils.R             # Helper functions and utilities
├── data/
│   ├── example_datasets.rda # Sample data for testing
│   └── reference_data.rda   # Reference populations and standards
├── tests/
│   └── testthat/
│       ├── test-descriptive.R
│       ├── test-hypothesis.R
│       └── test-regression.R
├── vignettes/
│   ├── getting_started.Rmd
│   ├── epidemic_analysis.Rmd
│   └── advanced_methods.Rmd
└── man/                    # Documentation files (auto-generated)
```

## Key Features

### Statistical Analysis Methods
- **Descriptive Statistics**: Measures of central tendency, dispersion, and distribution
- **Hypothesis Testing**: t-tests, chi-square, ANOVA, non-parametric tests
- **Regression Analysis**: Linear, logistic, Poisson, negative binomial regression
- **Time Series Analysis**: Trend detection, seasonality, autocorrelation
- **Survival Analysis**: Kaplan-Meier curves, Cox proportional hazards
- **Clustering**: k-means, hierarchical clustering, mixture models

### Epidemiological Specializations
- **Outbreak Detection**: Statistical process control, scan statistics
- **Case-Control Studies**: Odds ratios, matched analysis
- **Cohort Studies**: Risk ratios, incidence rates
- **Ecological Analysis**: Spatial and temporal correlation
- **Meta-Analysis**: Fixed and random effects models

## Installation

### Prerequisites

```r
# Install required R packages
install.packages(c(
  "tidyverse", "data.table", "lubridate",
  "survival", "cluster", "forecast",
  "epitools", "epiR", "surveillance",
  "ggplot2", "plotly", "DT",
  "knitr", "rmarkdown", "testthat"
))
```

### Install Module

```r
# Install from local development
devtools::install("src/R/data_analysis")

# Or install dependencies only
devtools::install_deps("src/R/data_analysis")
```

## Quick Start

### Basic Data Analysis Workflow

```r
library(pandemicgame.analysis)

# Load example dataset
data("covid_surveillance")

# Basic descriptive statistics
summary_stats <- describe_epidemic_data(
  data = covid_surveillance,
  cases_col = "daily_cases",
  date_col = "date",
  location_col = "state"
)

print(summary_stats)

# Time series analysis
ts_analysis <- analyze_epidemic_time_series(
  data = covid_surveillance,
  value_col = "daily_cases",
  date_col = "date",
  detect_outbreaks = TRUE
)

# Visualize results
plot_epidemic_curve(ts_analysis)
```

### Hypothesis Testing

```r
# Compare case rates between regions
region_comparison <- compare_epidemic_rates(
  data = covid_surveillance,
  cases_col = "daily_cases",
  population_col = "population",
  group_col = "region",
  method = "poisson_test"
)

# Print results
print(region_comparison$test_results)
print(region_comparison$effect_sizes)
```

### Regression Analysis

```r
# Model case counts with environmental factors
regression_model <- fit_epidemic_regression(
  formula = daily_cases ~ temperature + humidity + population_density + 
            intervention_level + lag(daily_cases, 7),
  data = covid_surveillance,
  family = "negative_binomial",
  offset = log(population)
)

# Model diagnostics
model_diagnostics <- check_regression_assumptions(regression_model)
plot(model_diagnostics)

# Prediction
predictions <- predict_epidemic_cases(
  model = regression_model,
  newdata = forecast_data,
  prediction_interval = 0.95
)
```

## Core Functions

### Data Import and Validation

```r
# Import and validate epidemiological data
validate_epidemic_data <- function(data, required_cols = NULL, 
                                 date_format = "%Y-%m-%d") {
  # Validation logic
}

# Load multiple data sources
load_surveillance_data <- function(sources = c("WHO", "CDC", "ECDC")) {
  # Data loading logic
}
```

### Descriptive Statistics

```r
# Comprehensive epidemic data summary
describe_epidemic_data <- function(data, cases_col, date_col, 
                                 location_col = NULL, 
                                 demographics = NULL) {
  list(
    temporal_summary = describe_temporal_patterns(data, cases_col, date_col),
    spatial_summary = describe_spatial_patterns(data, cases_col, location_col),
    demographic_summary = describe_demographic_patterns(data, demographics),
    distribution_analysis = analyze_case_distribution(data, cases_col)
  )
}

# Calculate epidemic indicators
calculate_epidemic_indicators <- function(data, cases_col, date_col,
                                        population_col = NULL) {
  list(
    attack_rate = calculate_attack_rate(data, cases_col, population_col),
    doubling_time = calculate_doubling_time(data, cases_col, date_col),
    growth_rate = calculate_growth_rate(data, cases_col, date_col),
    peak_timing = identify_epidemic_peak(data, cases_col, date_col)
  )
}
```

### Time Series Analysis

```r
# Comprehensive time series analysis
analyze_epidemic_time_series <- function(data, value_col, date_col,
                                       detect_outbreaks = TRUE,
                                       seasonal_adjustment = TRUE) {
  
  # Convert to time series
  ts_data <- create_epidemic_timeseries(data, value_col, date_col)
  
  # Decomposition
  decomposition <- decompose_epidemic_series(ts_data, seasonal_adjustment)
  
  # Outbreak detection
  outbreaks <- if(detect_outbreaks) {
    detect_epidemic_outbreaks(ts_data, method = "farrington")
  } else NULL
  
  # Autocorrelation analysis
  acf_results <- analyze_autocorrelation(ts_data)
  
  list(
    timeseries = ts_data,
    decomposition = decomposition,
    outbreaks = outbreaks,
    autocorrelation = acf_results,
    trend_analysis = analyze_trend(ts_data)
  )
}

# Forecast epidemic trends
forecast_epidemic <- function(ts_data, horizon = 30, method = "auto_arima") {
  switch(method,
    "auto_arima" = forecast::auto.arima(ts_data) %>% forecast(h = horizon),
    "ets" = forecast::ets(ts_data) %>% forecast(h = horizon),
    "prophet" = forecast_with_prophet(ts_data, horizon)
  )
}
```

### Statistical Testing

```r
# Compare epidemic patterns between groups
compare_epidemic_patterns <- function(data, value_col, group_col,
                                    test_type = "parametric") {
  
  if(test_type == "parametric") {
    # ANOVA or t-test
    if(length(unique(data[[group_col]])) > 2) {
      aov(formula(paste(value_col, "~", group_col)), data = data)
    } else {
      t.test(formula(paste(value_col, "~", group_col)), data = data)
    }
  } else {
    # Non-parametric alternatives
    if(length(unique(data[[group_col]])) > 2) {
      kruskal.test(formula(paste(value_col, "~", group_col)), data = data)
    } else {
      wilcox.test(formula(paste(value_col, "~", group_col)), data = data)
    }
  }
}

# Test for epidemic threshold exceedance
test_epidemic_threshold <- function(data, cases_col, threshold,
                                  method = "poisson") {
  observed <- data[[cases_col]]
  
  switch(method,
    "poisson" = poisson.test(sum(observed), r = threshold * length(observed)),
    "binomial" = binom.test(sum(observed > threshold), length(observed)),
    "farrington" = farrington_threshold_test(observed, threshold)
  )
}
```

### Survival Analysis

```r
# Analyze time to recovery or death
analyze_epidemic_survival <- function(data, time_col, event_col,
                                    covariates = NULL) {
  
  # Kaplan-Meier survival curves
  km_fit <- survival::survfit(
    formula = Surv(time_col, event_col) ~ 1,
    data = data
  )
  
  # Cox proportional hazards model if covariates provided
  cox_model <- if(!is.null(covariates)) {
    cov_formula <- paste(covariates, collapse = " + ")
    full_formula <- paste("Surv(", time_col, ",", event_col, ") ~", cov_formula)
    survival::coxph(formula(full_formula), data = data)
  } else NULL
  
  list(
    kaplan_meier = km_fit,
    cox_model = cox_model,
    median_time = median(km_fit),
    survival_summary = summary(km_fit)
  )
}
```

## Configuration

### Analysis Settings

Create `config/analysis_config.yaml`:

```yaml
data_validation:
  required_columns: ["date", "cases", "location"]
  date_formats: ["%Y-%m-%d", "%m/%d/%Y", "%d-%m-%Y"]
  missing_data_threshold: 0.1

time_series:
  frequency: "daily"  # daily, weekly, monthly
  seasonal_periods: [7, 365.25]  # weekly, yearly
  outbreak_detection:
    method: "farrington"
    threshold: 0.05
    minimum_cases: 5

regression:
  default_family: "negative_binomial"
  significance_level: 0.05
  include_diagnostics: true
  bootstrap_iterations: 1000

clustering:
  methods: ["kmeans", "hierarchical", "mixture"]
  optimal_clusters: "auto"  # or specify number
  distance_metric: "euclidean"

reporting:
  output_format: "html"
  include_plots: true
  confidence_level: 0.95
```

## Advanced Analysis Examples

### Outbreak Detection and Characterization

```r
# Multi-method outbreak detection
detect_outbreaks_comprehensive <- function(data, cases_col, date_col) {
  
  # Farrington algorithm
  farrington_alerts <- surveillance::farrington(
    create_sts_object(data, cases_col, date_col),
    control = list(
      range = 52:nrow(data),
      noPeriods = 10,
      populationOffset = FALSE,
      fitFun = "algo.farrington.fitGLM.flexible"
    )
  )
  
  # CUSUM method
  cusum_alerts <- detect_cusum_outbreaks(data, cases_col)
  
  # Scan statistics
  scan_alerts <- detect_scan_outbreaks(data, cases_col, date_col)
  
  # Combine results
  combine_outbreak_alerts(farrington_alerts, cusum_alerts, scan_alerts)
}

# Characterize detected outbreaks
characterize_outbreak <- function(data, outbreak_periods, cases_col) {
  map(outbreak_periods, function(period) {
    outbreak_data <- filter(data, date >= period$start, date <= period$end)
    
    list(
      duration = as.numeric(period$end - period$start),
      total_cases = sum(outbreak_data[[cases_col]]),
      peak_cases = max(outbreak_data[[cases_col]]),
      attack_rate = calculate_attack_rate(outbreak_data, cases_col),
      doubling_time = calculate_doubling_time(outbreak_data, cases_col, "date")
    )
  })
}
```

### Spatial-Temporal Analysis

```r
# Analyze spatial-temporal epidemic patterns
analyze_spatiotemporal_patterns <- function(data, cases_col, location_col, 
                                          date_col, coordinates = NULL) {
  
  # Temporal clustering
  temporal_clusters <- cluster_temporal_patterns(data, cases_col, date_col)
  
  # Spatial clustering (if coordinates available)
  spatial_clusters <- if(!is.null(coordinates)) {
    cluster_spatial_patterns(data, cases_col, coordinates)
  } else NULL
  
  # Space-time interaction
  spacetime_interaction <- test_spacetime_interaction(
    data, cases_col, location_col, date_col
  )
  
  list(
    temporal_clusters = temporal_clusters,
    spatial_clusters = spatial_clusters,
    spacetime_interaction = spacetime_interaction,
    hotspot_analysis = identify_epidemic_hotspots(data, cases_col, location_col)
  )
}
```

### Meta-Analysis Functions

```r
# Combine results from multiple studies/regions
perform_meta_analysis <- function(study_data, effect_col, variance_col,
                                method = "random_effects") {
  
  # Calculate pooled effect size
  if(method == "fixed_effects") {
    meta_result <- metafor::rma(
      yi = study_data[[effect_col]],
      vi = study_data[[variance_col]],
      method = "FE"
    )
  } else {
    meta_result <- metafor::rma(
      yi = study_data[[effect_col]],
      vi = study_data[[variance_col]],
      method = "REML"
    )
  }
  
  # Heterogeneity assessment
  heterogeneity <- assess_heterogeneity(meta_result)
  
  # Publication bias tests
  pub_bias <- test_publication_bias(meta_result)
  
  list(
    pooled_estimate = meta_result,
    heterogeneity = heterogeneity,
    publication_bias = pub_bias,
    forest_plot_data = prepare_forest_plot_data(meta_result)
  )
}
```

## Visualization Integration

```r
# Create analysis plots using ggplot2
plot_epidemic_analysis <- function(analysis_results, plot_type = "all") {
  
  plots <- list()
  
  if(plot_type %in% c("all", "timeseries")) {
    plots$timeseries <- plot_epidemic_timeseries(analysis_results$timeseries)
  }
  
  if(plot_type %in% c("all", "distribution")) {
    plots$distribution <- plot_case_distribution(analysis_results$distribution)
  }
  
  if(plot_type %in% c("all", "regression")) {
    plots$regression <- plot_regression_diagnostics(analysis_results$regression)
  }
  
  if(plot_type %in% c("all", "survival")) {
    plots$survival <- plot_survival_curves(analysis_results$survival)
  }
  
  return(plots)
}

# Interactive plots with plotly
create_interactive_analysis <- function(data, analysis_type = "dashboard") {
  
  if(analysis_type == "dashboard") {
    # Create Shiny dashboard
    create_analysis_dashboard(data)
  } else {
    # Create individual interactive plots
    plotly::ggplotly(create_static_plot(data, analysis_type))
  }
}
```

## Testing

```r
# Run all tests
devtools::test()

# Run specific test files
testthat::test_file("tests/testthat/test-descriptive.R")
testthat::test_file("tests/testthat/test-hypothesis.R")

# Check package
devtools::check()

# Test coverage
covr::package_coverage()
```

## Example Analyses

### Complete Epidemic Analysis Workflow

```r
# Load data
data("pandemic_simulation_data")

# 1. Data validation and cleaning
validated_data <- validate_epidemic_data(
  pandemic_simulation_data,
  required_cols = c("date", "cases", "deaths", "location"),
  date_format = "%Y-%m-%d"
)

# 2. Descriptive analysis
descriptive_results <- describe_epidemic_data(
  validated_data,
  cases_col = "cases",
  date_col = "date",
  location_col = "location"
)

# 3. Time series analysis
ts_results <- analyze_epidemic_time_series(
  validated_data,
  value_col = "cases",
  date_col = "date",
  detect_outbreaks = TRUE
)

# 4. Regression modeling
model_results <- fit_epidemic_regression(
  formula = cases ~ poly(day_of_epidemic, 2) + temperature + 
            intervention_strength + lag(cases, 7),
  data = validated_data,
  family = "negative_binomial"
)

# 5. Generate comprehensive report
generate_analysis_report(
  descriptive = descriptive_results,
  timeseries = ts_results,
  regression = model_results,
  output_file = "epidemic_analysis_report.html"
)
```

## Contributing

1. Follow R package development best practices
2. Use `roxygen2` for documentation
3. Include comprehensive unit tests with `testthat`
4. Follow tidyverse style guidelines
5. Add examples to all exported functions
6. Update NEWS.md for significant changes

## Dependencies

### Core Dependencies
- `tidyverse`: Data manipulation and visualization
- `data.table`: High-performance data operations
- `lubridate`: Date/time handling

### Statistical Analysis
- `survival`: Survival analysis methods
- `cluster`: Clustering algorithms
- `forecast`: Time series forecasting
- `metafor`: Meta-analysis methods

### Epidemiological Specializations
- `surveillance`: Outbreak detection algorithms
- `epitools`: Epidemiological calculations
- `epiR`: Epidemiological analysis tools

## License

See project root LICENSE file for details.
