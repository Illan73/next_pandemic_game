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
  
  return(result)
}

# Tau-leaping for faster approximate stochastic simulation
run_stochastic_sir_tauleap <- function(params, initial_state, max_time, tau = 0.1) {
  
  current_state <- initial_state
  current_time <- 0
  results <- list()
  
  while(current_time < max_time && current_state["I"] > 0) {
    
    # Calculate propensities
    infection_rate <- params$beta * current_state["S"] * current_state["I"] / params$N
    recovery_rate <- params$gamma * current_state["I"]
    
    # Sample number of events
    n_infections <- rpois(1, infection_rate * tau)
    n_recoveries <- rpois(1, recovery_rate * tau)
    
    # Ensure we don't have more events than possible
    n_infections <- min(n_infections, current_state["S"])
    n_recoveries <- min(n_recoveries, current_state["I"])
    
    # Update state
    current_state["S"] <- current_state["S"] - n_infections
    current_state["I"] <- current_state["I"] + n_infections - n_recoveries
    current_state["R"] <- current_state["R"] + n_recoveries
    
    # Store result
    results[[length(results) + 1]] <- c(time = current_time, current_state)
    
    current_time <- current_time + tau
  }
  
  return(do.call(rbind, results))
}
```

### Network Models

```r
# Contact network epidemic simulation
NetworkEpidemicModel <- R6::R6Class(
  "NetworkEpidemicModel",
  public = list(
    network = NULL,
    node_states = NULL,
    parameters = NULL,
    
    initialize = function(contact_network, transmission_prob, recovery_rate) {
      self$network <- contact_network
      self$parameters <- list(
        transmission_prob = transmission_prob,
        recovery_rate = recovery_rate
      )
      
      # Initialize all nodes as susceptible
      self$node_states <- rep("S", igraph::vcount(contact_network))
    },
    
    seed_infection = function(initial_infected_nodes) {
      self$node_states[initial_infected_nodes] <- "I"
    },
    
    step = function() {
      new_states <- self$node_states
      
      # Process infections
      infected_nodes <- which(self$node_states == "I")
      for(infected_node in infected_nodes) {
        # Get neighbors of infected node
        neighbors <- igraph::neighbors(self$network, infected_node)
        susceptible_neighbors <- neighbors[self$node_states[neighbors] == "S"]
        
        # Attempt transmission to each susceptible neighbor
        for(neighbor in susceptible_neighbors) {
          if(runif(1) < self$parameters$transmission_prob) {
            new_states[neighbor] <- "I"
          }
        }
      }
      
      # Process recoveries
      for(infected_node in infected_nodes) {
        if(runif(1) < self$parameters$recovery_rate) {
          new_states[infected_node] <- "R"
        }
      }
      
      self$node_states <- new_states
      return(self$get_state_counts())
    },
    
    get_state_counts = function() {
      table(factor(self$node_states, levels = c("S", "I", "R")))
    },
    
    run_simulation = function(time_steps) {
      results <- matrix(0, nrow = time_steps + 1, ncol = 4)
      colnames(results) <- c("time", "S", "I", "R")
      
      # Initial state
      initial_counts <- self$get_state_counts()
      results[1, ] <- c(0, initial_counts)
      
      # Run simulation
      for(t in 1:time_steps) {
        state_counts <- self$step()
        results[t + 1, ] <- c(t, state_counts)
        
        # Stop if no more infected individuals
        if(state_counts["I"] == 0) break
      }
      
      return(as.data.frame(results[1:(t+1), ]))
    }
  )
)
```

### Metapopulation Models

```r
# Multi-region coupled epidemic model
MetapopulationModel <- R6::R6Class(
  "MetapopulationModel",
  public = list(
    n_populations = NULL,
    populations = NULL,
    mobility_matrix = NULL,
    local_parameters = NULL,
    
    initialize = function(population_sizes, mobility_rates, local_params) {
      self$n_populations <- length(population_sizes)
      self$populations <- population_sizes
      self$mobility_matrix <- mobility_rates
      self$local_parameters <- local_params
    },
    
    run_metapopulation_sir = function(initial_conditions, time_span) {
      
      # ODE system for metapopulation SIR
      metapop_ode <- function(time, state, params) {
        
        n_pops <- params$n_populations
        
        # Reshape state vector into matrix (populations x compartments)
        state_matrix <- matrix(state, nrow = n_pops, ncol = 3)
        colnames(state_matrix) <- c("S", "I", "R")
        
        # Initialize derivatives
        dstate <- matrix(0, nrow = n_pops, ncol = 3)
        
        for(i in 1:n_pops) {
          S_i <- state_matrix[i, "S"]
          I_i <- state_matrix[i, "I"]
          R_i <- state_matrix[i, "R"]
          N_i <- params$populations[i]
          
          beta_i <- params$local_parameters[[i]]$beta
          gamma_i <- params$local_parameters[[i]]$gamma
          
          # Local transmission dynamics
          dS_local <- -beta_i * S_i * I_i / N_i
          dI_local <- beta_i * S_i * I_i / N_i - gamma_i * I_i
          dR_local <- gamma_i * I_i
          
          # Migration effects
          dS_migration <- 0
          dI_migration <- 0
          dR_migration <- 0
          
          for(j in 1:n_pops) {
            if(i != j) {
              # Migration from j to i
              dS_migration <- dS_migration + 
                params$mobility_matrix[j, i] * state_matrix[j, "S"] -
                params$mobility_matrix[i, j] * S_i
              
              dI_migration <- dI_migration + 
                params$mobility_matrix[j, i] * state_matrix[j, "I"] -
                params$mobility_matrix[i, j] * I_i
              
              dR_migration <- dR_migration + 
                params$mobility_matrix[j, i] * state_matrix[j, "R"] -
                params$mobility_matrix[i, j] * R_i
            }
          }
          
          # Combine local and migration effects
          dstate[i, "S"] <- dS_local + dS_migration
          dstate[i, "I"] <- dI_local + dI_migration
          dstate[i, "R"] <- dR_local + dR_migration
        }
        
        return(list(as.vector(dstate)))
      }
      
      # Solve ODE system
      times <- seq(0, time_span, by = 0.1)
      result <- deSolve::ode(
        y = as.vector(initial_conditions),
        times = times,
        func = metapop_ode,
        parms = list(
          n_populations = self$n_populations,
          populations = self$populations,
          mobility_matrix = self$mobility_matrix,
          local_parameters = self$local_parameters
        )
      )
      
      return(self$format_metapop_results(result))
    },
    
    format_metapop_results = function(raw_results) {
      n_pops <- self$n_populations
      times <- raw_results[, "time"]
      
      # Reshape results into list of data frames (one per population)
      formatted_results <- list()
      
      for(i in 1:n_pops) {
        pop_data <- data.frame(
          time = times,
          population = i,
          S = raw_results[, 3*(i-1) + 2],
          I = raw_results[, 3*(i-1) + 3],
          R = raw_results[, 3*(i-1) + 4]
        )
        formatted_results[[paste0("population_", i)]] <- pop_data
      }
      
      # Also create combined data frame
      formatted_results$combined <- do.call(rbind, formatted_results[1:n_pops])
      
      return(formatted_results)
    }
  )
)
```

### Age-Structured Models

```r
# Age-stratified SEIR model
run_age_structured_seir <- function(contact_matrix, age_groups, 
                                  age_specific_params, initial_conditions,
                                  time_span = 365) {
  
  n_age_groups <- length(age_groups)
  
  # Age-structured SEIR ODE system
  age_seir_ode <- function(time, state, params) {
    
    # Reshape state vector
    state_array <- array(state, dim = c(n_age_groups, 4))
    dimnames(state_array) <- list(age_groups, c("S", "E", "I", "R"))
    
    # Initialize derivatives
    dstate <- array(0, dim = c(n_age_groups, 4))
    
    # Calculate force of infection for each age group
    force_of_infection <- numeric(n_age_groups)
    
    for(i in 1:n_age_groups) {
      foi <- 0
      for(j in 1:n_age_groups) {
        foi <- foi + params$contact_matrix[i, j] * 
               params$age_specific_params[[j]]$beta * 
               state_array[j, "I"] / sum(state_array[j, ])
      }
      force_of_infection[i] <- foi
    }
    
    # Calculate derivatives for each age group
    for(i in 1:n_age_groups) {
      S_i <- state_array[i, "S"]
      E_i <- state_array[i, "E"]
      I_i <- state_array[i, "I"]
      R_i <- state_array[i, "R"]
      
      sigma_i <- params$age_specific_params[[i]]$sigma
      gamma_i <- params$age_specific_params[[i]]$gamma
      
      dstate[i, "S"] <- -force_of_infection[i] * S_i
      dstate[i, "E"] <- force_of_infection[i] * S_i - sigma_i * E_i
      dstate[i, "I"] <- sigma_i * E_i - gamma_i * I_i
      dstate[i, "R"] <- gamma_i * I_i
    }
    
    return(list(as.vector(dstate)))
  }
  
  # Solve ODE system
  times <- seq(0, time_span, by = 0.1)
  result <- deSolve::ode(
    y = as.vector(initial_conditions),
    times = times,
    func = age_seir_ode,
    parms = list(
      contact_matrix = contact_matrix,
      age_specific_params = age_specific_params
    )
  )
  
  return(format_age_structured_results(result, age_groups))
}

# Load age-specific contact matrices
load_contact_matrix <- function(country = "USA", setting = "all") {
  # This would load empirical contact matrices
  # For example, from Prem et al. 2017 or Mossong et al. 2008
  
  if(country == "USA" && setting == "all") {
    # Example 16x16 age contact matrix
    data("usa_contact_matrix", package = "pandemicgame.models")
    return(usa_contact_matrix)
  }
  
  # Default synthetic contact matrix
  n_age_groups <- 16
  contact_matrix <- matrix(1, nrow = n_age_groups, ncol = n_age_groups)
  
  # Higher contact within age groups
  diag(contact_matrix) <- 3
  
  # Higher contact for adjacent age groups
  for(i in 1:(n_age_groups-1)) {
    contact_matrix[i, i+1] <- 2
    contact_matrix[i+1, i] <- 2
  }
  
  return(contact_matrix)
}
```

## Parameter Estimation

### Maximum Likelihood Estimation

```r
# Fit SIR model to observed case data
fit_sir_to_data <- function(case_data, population_size, 
                           initial_guess = list(beta = 0.3, gamma = 0.1)) {
  
  # Define likelihood function
  sir_likelihood <- function(params, data, N) {
    
    if(params[1] <= 0 || params[2] <= 0) return(-Inf)
    
    beta <- params[1]
    gamma <- params[2]
    
    # Run SIR model with current parameters
    sir_params <- list(beta = beta, gamma = gamma, N = N)
    initial_state <- c(S = N - data$cases[1], I = data$cases[1], R = 0)
    
    model_result <- run_sir_model(
      parameters = sir_params,
      initial_conditions = initial_state,
      time_span = length(data$cases) - 1
    )
    
    # Extract model predictions at observation times
    model_cases <- model_result$I[1:length(data$cases)]
    
    # Calculate log-likelihood (assuming Poisson observation model)
    log_likelihood <- sum(dpois(data$cases, lambda = model_cases, log = TRUE))
    
    return(log_likelihood)
  }
  
  # Optimize parameters
  fit_result <- optim(
    par = c(initial_guess$beta, initial_guess$gamma),
    fn = sir_likelihood,
    data = case_data,
    N = population_size,
    method = "L-BFGS-B",
    lower = c(0.001, 0.001),
    upper = c(10, 1),
    control = list(fnscale = -1)  # Maximize likelihood
  )
  
  # Calculate confidence intervals using profile likelihood
  confidence_intervals <- calculate_profile_confidence_intervals(
    fit_result, sir_likelihood, case_data, population_size
  )
  
  return(list(
    parameters = list(beta = fit_result$par[1], gamma = fit_result$par[2]),
    log_likelihood = fit_result$value,
    convergence = fit_result$convergence,
    confidence_intervals = confidence_intervals,
    R0 = fit_result$par[1] / fit_result$par[2]
  ))
}

# Bayesian parameter estimation using MCMC
fit_sir_bayesian <- function(case_data, population_size, 
                           prior_params = list(beta = c(0.5, 0.2), 
                                              gamma = c(0.1, 0.05))) {
  
  # Define prior distributions
  log_prior <- function(params) {
    beta <- params[1]
    gamma <- params[2]
    
    log_p_beta <- dnorm(beta, mean = prior_params$beta[1], 
                       sd = prior_params$beta[2], log = TRUE)
    log_p_gamma <- dnorm(gamma, mean = prior_params$gamma[1], 
                        sd = prior_params$gamma[2], log = TRUE)
    
    return(log_p_beta + log_p_gamma)
  }
  
  # Define posterior (likelihood + prior)
  log_posterior <- function(params, data, N) {
    if(params[1] <= 0 || params[2] <= 0) return(-Inf)
    
    log_lik <- sir_likelihood(params, data, N)
    log_pri <- log_prior(params)
    
    return(log_lik + log_pri)
  }
  
  # Run MCMC using BayesianTools
  setup <- BayesianTools::createBayesianSetup(
    likelihood = function(params) log_posterior(params, case_data, population_size),
    lower = c(0.001, 0.001),
    upper = c(5, 1)
  )
  
  mcmc_result <- BayesianTools::runMCMC(
    bayesianSetup = setup,
    sampler = "DEzs",
    settings = list(iterations = 10000, burnin = 2000)
  )
  
  # Extract posterior samples
  posterior_samples <- BayesianTools::getSample(mcmc_result)
  
  return(list(
    posterior_samples = posterior_samples,
    parameter_estimates = apply(posterior_samples, 2, mean),
    credible_intervals = apply(posterior_samples, 2, quantile, 
                              probs = c(0.025, 0.975)),
    mcmc_diagnostics = BayesianTools::gelmanDiagnostics(mcmc_result)
  ))
}
```

### Model Selection and Validation

```r
# Compare multiple epidemic models
compare_epidemic_models <- function(case_data, models = c("SIR", "SEIR", "SIRD")) {
  
  model_fits <- list()
  
  for(model_name in models) {
    
    if(model_name == "SIR") {
      fit <- fit_sir_to_data(case_data, population_size = 1000000)
    } else if(model_name == "SEIR") {
      fit <- fit_seir_to_data(case_data, population_size = 1000000)
    } else if(model_name == "SIRD") {
      fit <- fit_sird_to_data(case_data, population_size = 1000000)
    }
    
    # Calculate AIC and BIC
    n_params <- length(fit$parameters)
    n_obs <- length(case_data$cases)
    
    fit$AIC <- -2 * fit$log_likelihood + 2 * n_params
    fit$BIC <- -2 * fit$log_likelihood + log(n_obs) * n_params
    
    model_fits[[model_name]] <- fit
  }
  
  # Create comparison table
  comparison_table <- data.frame(
    model = names(model_fits),
    log_likelihood = sapply(model_fits, function(x) x$log_likelihood),
    AIC = sapply(model_fits, function(x) x$AIC),
    BIC = sapply(model_fits, function(x) x$BIC),
    stringsAsFactors = FALSE
  )
  
  # Calculate model weights
  comparison_table$delta_AIC <- comparison_table$AIC - min(comparison_table$AIC)
  comparison_table$AIC_weight <- exp(-0.5 * comparison_table$delta_AIC) / 
                                sum(exp(-0.5 * comparison_table$delta_AIC))
  
  return(list(
    model_fits = model_fits,
    comparison_table = comparison_table,
    best_model = comparison_table$model[which.min(comparison_table$AIC)]
  ))
}

# Cross-validation for model assessment
cross_validate_model <- function(case_data, model_type = "SIR", 
                                k_folds = 5, forecast_horizon = 14) {
  
  n_obs <- nrow(case_data)
  fold_size <- floor(n_obs / k_folds)
  
  cv_results <- list()
  
  for(fold in 1:k_folds) {
    
    # Define training and testing sets
    test_start <- (fold - 1) * fold_size + 1
    test_end <- min(fold * fold_size, n_obs - forecast_horizon)
    
    train_data <- case_data[1:(test_start - 1), ]
    test_data <- case_data[test_start:(test_start + forecast_horizon - 1), ]
    
    if(nrow(train_data) < 10) next  # Skip if training set too small
    
    # Fit model to training data
    if(model_type == "SIR") {
      model_fit <- fit_sir_to_data(train_data, population_size = 1000000)
    }
    
    # Generate predictions
    predictions <- predict_epidemic_model(
      model_fit, 
      forecast_horizon = forecast_horizon,
      initial_conditions = tail(train_data, 1)
    )
    
    # Calculate prediction errors
    mae <- mean(abs(predictions$cases - test_data$cases))
    rmse <- sqrt(mean((predictions$cases - test_data$cases)^2))
    
    cv_results[[fold]] <- list(
      fold = fold,
      mae = mae,
      rmse = rmse,
      predictions = predictions,
      observations = test_data
    )
  }
  
  # Summarize cross-validation results
  cv_summary <- data.frame(
    fold = sapply(cv_results, function(x) x$fold),
    mae = sapply(cv_results, function(x) x$mae),
    rmse = sapply(cv_results, function(x) x$rmse)
  )
  
  return(list(
    cv_results = cv_results,
    cv_summary = cv_summary,
    mean_mae = mean(cv_summary$mae, na.rm = TRUE),
    mean_rmse = mean(cv_summary$rmse, na.rm = TRUE)
  ))
}
```

## Intervention Modeling

### Vaccination Campaigns

```r
# SIRV model with vaccination
run_sirv_model <- function(params, initial_conditions, vaccination_schedule,
                          time_span = 365) {
  
  # SIRV ODE system
  sirv_ode <- function(time, state, parameters) {
    with(as.list(c(state, parameters)), {
      
      # Get current vaccination rate
      current_vax_rate <- interpolate_vaccination_rate(time, vaccination_schedule)
      
      dS <- -beta * S * I / N - current_vax_rate * S
      dI <- beta * S * I / N - gamma * I
      dR <- gamma * I
      dV <- current_vax_rate * S
      
      list(c(dS, dI, dR, dV))
    })
  }
  
  times <- seq(0, time_span, by = 0.1)
  result <- deSolve::ode(
    y = initial_conditions,
    times = times,
    func = sirv_ode,
    parms = params
  )
  
  return(as.data.frame(result))
}

# Multi-dose vaccination model
run_multidose_vaccination_model <- function(params, n_doses = 2) {
  
  # Extended compartments for multiple vaccine doses
  # S, I, R, V1, V2, ..., Vn (n vaccine compartments)
  
  multidose_ode <- function(time, state, parameters) {
    
    n_compartments <- length(state)
    n_vaccine_compartments <- n_doses
    
    S <- state[1]
    I <- state[2]  
    R <- state[3]
    V <- state[4:(3 + n_vaccine_compartments)]  # Vaccine compartments
    
    N <- sum(state)
    
    # Vaccine effectiveness increases with doses
    vaccine_effectiveness <- parameters$ve_schedule[1:n_doses]
    
    # Calculate effective susceptible population
    effective_susceptibles <- S + sum((1 - vaccine_effectiveness) * V)
    
    dS <- -parameters$beta * S * I / N - parameters$vaccination_rate * S
    dI <- parameters$beta * effective_susceptibles * I / N - parameters$gamma * I
    dR <- parameters$gamma * I
    
    # Vaccination compartment dynamics
    dV <- numeric(n_vaccine_compartments)
    dV[1] <- parameters$vaccination_rate * S - parameters$dose_interval_rate * V[1]
    
    if(n_doses > 1) {
      for(i in 2:n_doses) {
        dV[i] <- parameters$dose_interval_rate * V[i-1] - 
                 ifelse(i < n_doses, parameters$dose_interval_rate * V[i], 0)
      }
    }
    
    return(list(c(dS, dI, dR, dV)))
  }
  
  # Implementation continues...
}
```

### Non-Pharmaceutical Interventions

```r
# Model with time-varying contact rates due to interventions
run_sir_with_interventions <- function(params, initial_conditions, 
                                     intervention_schedule, time_span = 365) {
  
  sir_intervention_ode <- function(time, state, parameters) {
    with(as.list(c(state, parameters)), {
      
      # Get current intervention effect
      intervention_effect <- get_intervention_effect(time, intervention_schedule)
      
      # Modify transmission rate based on interventions
      effective_beta <- beta * (1 - intervention_effect)
      
      dS <- -effective_beta * S * I / N
      dI <- effective_beta * S * I / N - gamma * I
      dR <- gamma * I
      
      list(c(dS, dI, dR))
    })
  }
  
  times <- seq(0, time_span, by = 0.1)
  result <- deSolve::ode(
    y = initial_conditions,
    times = times,
    func = sir_intervention_ode,
    parms = params
  )
  
  return(as.data.frame(result))
}

# Define intervention schedules
create_intervention_schedule <- function(interventions) {
  # interventions is a data frame with columns: start_time, end_time, type, effectiveness
  
  schedule <- data.frame(
    time = numeric(0),
    contact_reduction = numeric(0),
    transmission_reduction = numeric(0)
  )
  
  for(i in 1:nrow(interventions)) {
    intervention <- interventions[i, ]
    
    # Add intervention start
    schedule <- rbind(schedule, data.frame(
      time = intervention$start_time,
      contact_reduction = intervention$effectiveness,
      transmission_reduction = intervention$effectiveness
    ))
    
    # Add intervention end
    schedule <- rbind(schedule, data.frame(
      time = intervention$end_time,
      contact_reduction = 0,
      transmission_reduction = 0
    ))
  }
  
  return(schedule[order(schedule$time), ])
}
```

## Performance Optimization

### C++ Integration for Speed

```cpp
// Example C++ code for fast SIR simulation (in inst/cpp/fast_models.cpp)
#include <Rcpp.h>
using namespace Rcpp;

// [[Rcpp::export]]
NumericMatrix fast_sir_simulation(double beta, double gamma, int N, 
                                 int initial_infected, int time_steps, 
                                 double dt = 0.1) {
  
  NumericMatrix result(time_steps + 1, 4);
  colnames(result) = CharacterVector::create("time", "S", "I", "R");
  
  // Initial conditions
  double S = N - initial_infected;
  double I = initial_infected;
  double R = 0;
  
  result(0, 0) = 0;  // time
  result(0, 1) = S;
  result(0, 2) = I;
  result(0, 3) = R;
  
  // Euler integration
  for(int t = 1; t <= time_steps; t++) {
    double dS = -beta * S * I / N * dt;
    double dI = (beta * S * I / N - gamma * I) * dt;
    double dR = gamma * I * dt;
    
    S += dS;
    I += dI;
    R += dR;
    
    result(t, 0) = t * dt;
    result(t, 1) = S;
    result(t, 2) = I;
    result(t, 3) = R;
  }
  
  return result;
}
```

### Parallel Processing

```r
# Parallel ensemble simulations
run_parallel_stochastic_simulations <- function(params, initial_conditions,
                                               n_simulations = 1000, 
                                               n_cores = parallel::detectCores() - 1) {
  
  # Setup parallel backend
  cl <- parallel::makeCluster(n_cores)
  doParallel::registerDoParallel(cl)
  
  # Export necessary objects to workers
  parallel::clusterExport(cl, c("run_stochastic_sir_gillespie", "params", 
                               "initial_conditions"))
  
  # Run simulations in parallel
  results <- foreach::foreach(
    i = 1:n_simulations,
    .combine = 'list',
    .packages = c('deSolve', 'GillespieSSA2')
  ) %dopar% {
    run_stochastic_sir_gillespie(params, initial_conditions, max_time = 365)
  }
  
  # Cleanup
  parallel::stopCluster(cl)
  
  return(results)
}
```

## Testing and Validation

```r
# Unit tests for model accuracy
test_that("SIR model produces expected behavior", {
  
  # Test basic SIR dynamics
  params <- list(beta = 0.3, gamma = 0.1, N = 1000)
  initial_state <- c(S = 999, I = 1, R = 0)
  
  result <- run_sir_model(params, initial_state, time_span = 100)
  
  # Check conservation of population
  expect_equal(result$S + result$I + result$R, rep(1000, nrow(result)), 
               tolerance = 1e-6)
  
  # Check that epidemic eventually dies out
  expect_lt(tail(result$I, 1), 1e-6)
  
  # Check R0 calculation
  expect_equal(calculate_R0(params), 3.0, tolerance = 1e-10)
})

# Validation against historical data
validate_against_historical_data <- function(model_function, historical_data,
                                           parameter_bounds) {
  
  # Fit model to historical data
  fitted_model <- fit_model_to_data(
    model_function = model_function,
    observed_data = historical_data,
    parameter_bounds = parameter_bounds
  )
  
  # Generate predictions
  predictions <- predict_from_fitted_model(fitted_model, 
                                          forecast_horizon = 30)
  
  # Calculate validation metrics
  validation_metrics <- calculate_validation_metrics(
    predictions = predictions,
    observations = historical_data$validation_period
  )
  
  return(validation_metrics)
}
```

## Documentation and Examples

Comprehensive vignettes are provided in the `vignettes/` directory:

1. **Basic Models** (`basic_models.Rmd`): Introduction to SIR, SEIR models
2. **Advanced Modeling** (`advanced_modeling.Rmd`): Stochastic and network models  
3. **Parameter Calibration** (`parameter_calibration.Rmd`): Fitting models to data
4. **Model Comparison** (`model_comparison.Rmd`): Selecting the best model

## Contributing

1. Follow object-oriented design patterns using R6 classes
2. Include comprehensive unit tests for all model functions
3. Optimize performance-critical code with C++ (Rcpp)
4. Document all functions with roxygen2
5. Include validation against known analytical solutions
6. Add examples and vignettes for new models

## Performance Benchmarks

Expected performance on modern hardware:
- Deterministic SIR (365 days): ~1ms
- Stochastic SIR (1000 runs): ~100ms  
- Network epidemic (10,000 nodes): ~1s
- Metapopulation model (50 regions): ~10ms

## License

See project root LICENSE file for details.
