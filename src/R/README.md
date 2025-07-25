# Risk Assessment Module - Next Pandemic Game (R Project)

## Overview

This R package provides risk assessment functionality for the Next Pandemic Game simulation. It implements epidemiological models, healthcare capacity analysis, and economic impact assessment tools using R's statistical computing capabilities.

## Installation

### Prerequisites
- R >= 4.0.0
- RStudio (recommended)

### Required Packages
```r
# Core packages
install.packages(c(
  "dplyr",
  "ggplot2", 
  "tidyr",
  "readr",
  "lubridate",
  "here"
))

# Epidemiological modeling
install.packages(c(
  "EpiModel",
  "R0",
  "surveillance", 
  "epitools",
  "incidence"
))

# Statistical analysis
install.packages(c(
  "MASS",
  "boot",
  "forecast",
  "mgcv"
))

# Visualization and reporting
install.packages(c(
  "plotly",
  "DT",
  "flexdashboard",
  "rmarkdown",
  "shiny"
))
```

### Development Installation
```r
# Install from local development
devtools::install_local(".")

# Or from GitHub (when available)
devtools::install_github("pandemic-game/risk_assessment")
```

## Project Structure

```
risk_assessment/
├── R/                          # R source code
│   ├── risk_models.R          # Core risk assessment functions
│   ├── epi_models.R           # Epidemiological modeling
│   ├── healthcare_capacity.R  # Healthcare system analysis
│   ├── economic_impact.R      # Economic modeling functions
│   ├── visualization.R        # Plotting and dashboard functions
│   └── utils.R                # Utility functions
├── data/                      # Data files
│   ├── population_data.rda    # Population demographics
│   ├── healthcare_baseline.rda # Healthcare capacity baselines
│   └── economic_indicators.rda # Economic baseline data
├── data-raw/                  # Raw data processing scripts
│   ├── process_population.R
│   ├── process_healthcare.R
│   └── process_economic.R
├── inst/                      # Installed files
│   ├── shiny-apps/           # Shiny applications
│   │   └── risk_dashboard/   # Interactive risk dashboard
│   └── rmarkdown/            # Report templates
├── man/                       # Documentation
├── tests/                     # Unit tests
│   └── testthat/
├── vignettes/                 # Package vignettes
│   ├── getting_started.Rmd
│   ├── risk_modeling.Rmd
│   └── dashboard_usage.Rmd
├── DESCRIPTION               # Package metadata
├── NAMESPACE                # Package namespace
└── README.md                # This file
```

## Quick Start

### Basic Risk Assessment
```r
library(riskassessment)

# Load example data
data("population_data")
data("healthcare_baseline")

# Calculate basic reproduction number
r0_estimate <- calculate_r0(
  cases = daily_cases,
  serial_interval = 5.2,
  method = "ML"
)

# Assess healthcare capacity risk
capacity_risk <- assess_healthcare_capacity(
  current_icu_occupancy = 150,
  total_icu_beds = 200,
  surge_capacity = 0.3,
  covid_patients = 45
)

# Generate risk score
overall_risk <- calculate_risk_score(
  transmission_risk = r0_estimate$risk_level,
  capacity_risk = capacity_risk$risk_level,
  economic_risk = "medium"
)
```

### Interactive Dashboard
```r
# Launch the risk assessment dashboard
launch_risk_dashboard()

# Or run specific components
run_transmission_model()
run_capacity_planner()
```

## Core Functions

### Epidemiological Risk Assessment

#### `calculate_r0()`
Estimates the basic reproduction number using various methods.
```r
r0_result <- calculate_r0(
  cases = daily_cases_vector,
  serial_interval = 5.2,
  method = c("ML", "EG", "TD"),
  confidence_level = 0.95
)
```

#### `assess_transmission_risk()`
Evaluates community transmission risk based on multiple indicators.
```r
transmission_risk <- assess_transmission_risk(
  r0 = 1.3,
  test_positivity = 0.08,
  case_growth_rate = 0.05,
  contact_tracing_coverage = 0.7
)
```

### Healthcare Capacity Assessment

#### `assess_healthcare_capacity()`
Analyzes healthcare system stress and capacity constraints.
```r
capacity_assessment <- assess_healthcare_capacity(
  region = "state_name",
  current_occupancy = list(
    icu = 150,
    general_beds = 800,
    ventilators = 45
  ),
  covid_patients = 60,
  surge_multiplier = 1.3
)
```

#### `project_healthcare_demand()`
Projects future healthcare resource needs.
```r
demand_projection <- project_healthcare_demand(
  current_cases = daily_cases,
  r0 = 1.2,
  hospitalization_rate = 0.05,
  icu_rate = 0.015,
  projection_days = 30
)
```

### Economic Impact Assessment

#### `assess_economic_risk()`
Evaluates economic impact across sectors.
```r
economic_risk <- assess_economic_risk(
  lockdown_stringency = 7,
  sectors_affected = c("hospitality", "retail", "transport"),
  employment_data = employment_by_sector,
  gdp_baseline = quarterly_gdp
)
```

### Risk Integration and Scoring

#### `calculate_composite_risk()`
Combines multiple risk dimensions into overall assessment.
```r
composite_risk <- calculate_composite_risk(
  epi_risk = transmission_risk,
  healthcare_risk = capacity_assessment,
  economic_risk = economic_risk,
  weights = c(0.4, 0.4, 0.2)
)
```

## Data Format Requirements

### Case Data
```r
# Daily case data format
daily_cases <- data.frame(
  date = seq.Date(from = as.Date("2024-01-01"), by = "day", length.out = 100),
  cases = rpois(100, lambda = 50),
  deaths = rpois(100, lambda = 2),
  tests = rpois(100, lambda = 1000)
)
```

### Healthcare Data
```r
# Healthcare capacity data format
healthcare_data <- data.frame(
  region = "State_Name",
  total_beds = 1000,
  icu_beds = 200,
  ventilators = 150,
  staff_nurses = 500,
  staff_doctors = 100
)
```

### Population Data
```r
# Population demographics format
population_data <- data.frame(
  age_group = c("0-17", "18-64", "65+"),
  population = c(150000, 400000, 80000),
  comorbidity_rate = c(0.05, 0.15, 0.45)
)
```

## Model Configuration

### Risk Thresholds
```r
# Configure risk level thresholds
risk_thresholds <- list(
  r0 = list(
    low = 0.9,
    medium = 1.1,
    high = 1.5,
    critical = 2.0
  ),
  icu_occupancy = list(
    low = 0.6,
    medium = 0.8,
    high = 0.9,
    critical = 0.95
  ),
  test_positivity = list(
    low = 0.03,
    medium = 0.05,
    high = 0.10,
    critical = 0.15
  )
)
```

### Model Parameters
```r
# Set model parameters
model_params <- list(
  incubation_period = 5.1,
  infectious_period = 10,
  serial_interval = 5.2,
  hospitalization_delay = 7,
  icu_delay = 10,
  death_delay = 18
)
```

## Visualization and Reporting

### Generate Risk Dashboard
```r
# Create comprehensive risk report
generate_risk_report(
  output_file = "risk_assessment_report.html",
  data_date = Sys.Date(),
  region = "State_Name",
  include_projections = TRUE,
  projection_days = 30
)
```

### Custom Plots
```r
# Plot transmission risk over time
plot_transmission_risk(daily_cases, r0_estimates)

# Visualize healthcare capacity
plot_capacity_utilization(capacity_data, projections)

# Economic impact visualization
plot_economic_impact(gdp_data, employment_data)
```

## Testing

### Run Unit Tests
```r
# Run all tests
testthat::test_package("riskassessment")

# Run specific test files
testthat::test_file("tests/testthat/test-risk-models.R")
```

### Validation Tests
```r
# Validate against known scenarios
validate_model_performance(
  historical_data = historical_pandemic_data,
  model_outputs = model_results
)
```

## Shiny Applications

### Risk Dashboard
Interactive dashboard for real-time risk monitoring:
```r
# Launch main dashboard
shiny::runApp(system.file("shiny-apps/risk_dashboard", package = "riskassessment"))
```

### Scenario Planner
Tool for exploring different intervention scenarios:
```r
# Launch scenario planning tool
shiny::runApp(system.file("shiny-apps/scenario_planner", package = "riskassessment"))
```

## Configuration Files

### `config.yml`
```yaml
default:
  data_sources:
    cases: "data/daily_cases.csv"
    healthcare: "data/healthcare_capacity.csv"
    population: "data/population_demographics.csv"
  
  model_parameters:
    incubation_period: 5.1
    serial_interval: 5.2
    hospitalization_rate: 0.05
  
  risk_thresholds:
    r0_high: 1.5
    icu_critical: 0.9
    positivity_high: 0.1
```

## Development Workflow

### Contributing
1. Fork the repository
2. Create feature branch: `git checkout -b feature/new-risk-model`
3. Make changes and add tests
4. Run `devtools::check()` to ensure package passes checks
5. Submit pull request

### Code Style
- Follow [tidyverse style guide](https://style.tidyverse.org/)
- Use `styler::style_pkg()` to format code
- Add roxygen2 documentation for all functions

### Documentation
- Update function documentation in R files
- Add examples to function documentation
- Update vignettes for new features

## Troubleshooting

### Common Issues

**Package Dependencies**
```r
# If packages fail to install, try:
options(repos = c(CRAN = "https://cran.rstudio.com/"))
install.packages("package_name", dependencies = TRUE)
```

**Memory Issues with Large Datasets**
```r
# Use data.table for large datasets
library(data.table)
# Or process data in chunks
```

**Convergence Issues in Models**
```r
# Adjust model parameters or starting values
# Check data quality and completeness
```

## Support and Contact

- **Issues**: Submit via GitHub Issues
- **Documentation**: See package vignettes and help files
- **Development**: Contact maintainer at dev@pandemicgame.org

## License

MIT License - see LICENSE file for details

---

*This package is part of the Next Pandemic Game R project ecosystem.*
