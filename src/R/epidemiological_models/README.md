# Epidemiological Models Module (R)

## Overview

The R epidemiological models module provides comprehensive mathematical modeling capabilities for disease transmission dynamics in the Next Pandemic Game. This module implements classical compartmental models (SIR, SEIR, etc.), agent-based models, network models, and advanced stochastic simulations to accurately represent epidemic spread patterns.

## Structure

```
src/R/epidemiological_models/
├── README.md
├── DESCRIPTION              # Package metadata and dependencies
├── NAMESPACE               # Package namespace definitions
├── R/
│   ├── compartmental_models.R    # SIR, SEIR, SIRD models
│   ├── stochastic_models.R       # Stochastic implementations
│   ├── network_models.R          # Contact network transmission
│   ├── metapopulation_models.R   # Multi-region models
│   ├── age_structured_models.R   # Age-stratified models
│   ├── vaccination_models.R      # Vaccination dynamics
│   ├── intervention_models.R     # Policy intervention effects
│   ├── parameter_estimation.R    # Model fitting and calibration
│   ├── sensitivity_analysis.R    # Parameter sensitivity testing
│   └── utils.R                   # Helper functions and utilities
├── data/
│   ├── parameter_sets.rda        # Default parameter values
│   ├── contact_matrices.rda      # Age-structured contact patterns
│   └── validation_data.rda       # Historical epidemic data
├── tests/
│   └── testthat/
│       ├── test-sir-models.R
│       ├── test-stochastic.R
│       ├── test-networks.R
│       └── test-parameter-estimation.R
├── vignettes/
│   ├── basic_models.Rmd
│   ├── advanced_modeling.Rmd
│   ├── parameter_calibration.Rmd
│   └── model_comparison.Rmd
├── inst/
│   ├── cpp/                      # C++ code for performance
│   └── python/                   # Python integration scripts
└── man/                          # Documentation files
```

## Key Features

### Compartmental Models
- **Basic Models**: SIR, SIS, SIRD, SEIR, SEIRD
- **Extended Models**: Age-structured, risk-stratified compartments
- **Vaccination Models**: Multi-dose vaccination dynamics
- **Waning Immunity**: Temporary immunity models (SIRS)

### Advanced Modeling Approaches
- **Stochastic Models**: Gillespie algorithm, tau-leaping
- **Network Models**: Contact networks, small-world, scale-free
- **Metapopulation Models**: Multi-city/region coupled dynamics
- **Agent-Based Models**: Individual-level simulation

### Parameter Estimation
- **Maximum Likelihood**: Parameter fitting to observed data
- **Bayesian Methods**: MCMC, particle filtering
- **Approximate Bayesian Computation**: Model-free inference
- **Cross-Validation**: Model selection and validation

### Intervention Modeling
- **Non-pharmaceutical Interventions**: Social distancing, lockdowns
- **Pharmaceutical Interventions**: Vaccination campaigns, treatments
- **Behavioral Changes**: Dynamic contact rate modifications
- **Policy Optimization**: Cost-benefit analysis of interventions

## Installation

### Prerequisites

```r
# Install required R packages
install.packages(c(
  "deSolve", "adaptivetau", "GillespieSSA2",
  "igraph", "statnet", "EpiModel",
  "pomp", "particle", "BayesianTools",
  "foreach", "doParallel", "future",
  "Rcpp", "RcppArmadillo", "reticulate"
))

# Install epidemiological packages
install.packages(c(
  "EpiEstim", "R0", "incidence",
  "outbreaks", "epitrix", "projections"
))
```

### Install Module

```r
# Install from local development
devtools::install("src/R/epidemiological_models")

# Compile C++ components if needed
Rcpp::sourceCpp("src/R/epidemiological_models/inst/cpp/fast_models.cpp")
```

## Quick Start

### Basic SIR Model

```r
library(pandemicgame.models)

# Define parameters
params <- list(
  beta = 0.3,     # transmission rate
  gamma = 0.1,    # recovery rate
  N = 1000000     # total population
)

# Initial conditions
initial_state <- c(S = 999999, I = 1, R = 0)

# Run deterministic SIR model
sir_result <- run_sir_model(
  parameters = params,
  initial_conditions = initial_state,
  time_span = 365
)

# Plot results
plot_epidemic_curves(sir_result)

# Calculate key metrics
epidemic_metrics <- calculate_epidemic_metrics(sir_result)
print(epidemic_metrics)
```

### Stochastic SEIR Model

```r
# SEIR with stochasticity
seir_params <- list(
  beta = 0.4,      # transmission rate
  sigma = 0.2,     # incubation rate (1/latent period)
  gamma = 0.1,     # recovery rate
  N = 100000
)

# Run stochastic simulation
stochastic_result <- run_stochastic_seir(
  parameters = seir_params,
  initial_conditions = c(S = 99999, E = 1, I = 0, R = 0),
  time_span = 200,
  n_simulations = 100
)

# Analyze ensemble results
ensemble_summary <- summarize_stochastic_runs(stochastic_result)
plot_stochastic_envelope(ensemble_summary)
```

### Network-Based Transmission

```r
# Create contact network
contact_network <- generate_contact_network(
  n_nodes = 10000,
  network_type = "small_world",
  avg_degree = 8,
  rewiring_prob = 0.1
)

# Run network epidemic model
network_result <- run_network_epidemic(
  network = contact_network,
  transmission_prob = 0.05,
  recovery_rate = 0.1,
  initial_infected = 10,
  time_steps = 200
)

# Visualize network spread
plot_network_epidemic(network_result, layout = "spring")
```

## Core Model Classes

### Compartmental Models

```r
# Base class for all compartmental models
CompartmentalModel <- R6::R6Class(
  "CompartmentalModel",
  public = list(
    parameters = NULL,
    initial_conditions = NULL,
    
    initialize = function(params, initial_state) {
      self$parameters <- params
      self$initial_conditions <- initial_state
    },
    
    set_parameters = function(new_params) {
      self$parameters <- modifyList(self$parameters, new_params)
    },
    
    run = function(time_span, method = "ode45") {
      # Implementation depends on specific model
    },
    
    calculate_R0 = function() {
      # Basic reproduction number calculation
      with(self$parameters, beta / gamma)
    }
  )
)

# SIR Model implementation
SIRModel <- R6::R6Class(
  "SIRModel",
  inherit = CompartmentalModel,
  public = list(
    run = function(time_span = 365, dt = 0.1) {
      
      # Define ODE system
      sir_ode <- function(time, state, params) {
        with(as.list(c(state, params)), {
          dS <- -beta * S * I / N
          dI <- beta * S * I / N - gamma * I
          dR <- gamma * I
          
          list(c(dS, dI, dR))
        })
      }
      
      # Solve ODE system
      times <- seq(0, time_span, by = dt)
      result <- deSolve::ode(
        y = self$initial_conditions,
        times = times,
        func = sir_ode,
        parms = self$parameters
      )
      
      return(as.data.frame(result))
    }
  )
)
```

### Stochastic Models

```r
# Stochastic SIR using Gillespie algorithm
run_stochastic_sir_gillespie <- function(params, initial_state, max_time) {
  
  # Define reaction rates
  rate_functions <- list(
    infection = function(state, params) {
      params$beta * state["S"] * state["I"] / params$N
    },
    recovery = function(state, params) {
      params$gamma * state["I"]
    }
  )
  
  # Define state changes
  state_changes <- list(
    infection = c(S = -1, I = 1, R = 0),
    recovery = c(S = 0, I = -1, R = 1)
  )
  
  # Run Gillespie simulation
  result <- GillespieSSA2::ssa(
    initial_state = initial_state,
    reactions = list(
      reaction(rate = rate_functions$infection, effect = state_changes$infection),
      reaction(rate = rate_functions$recovery, effect = state_changes$recovery)
    ),
    params = params,
    method = ssa_exact(),
    final_time = max_time
  )
