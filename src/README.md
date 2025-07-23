# Source Code Directory 💻

Implementation of the Next Pandemic Game simulation using R for statistical modeling and Python for data processing and visualization.

## Architecture

### `R/`
Statistical analysis and epidemiological modeling:
- **`epidemiological_models/`** - SIR/SEIR disease dynamics, transmission models
- **`risk_assessment/`** - Spillover probability estimation, risk scoring algorithms  
- **`statistical_analysis/`** - Parameter estimation, uncertainty quantification
- **`validation/`** - Model testing against historical data

### `python/`
Data management and interactive components:
- **`data_collection/`** - API clients, web scraping, database connections
- **`preprocessing/`** - Data cleaning, standardization, quality control
- **`visualization/`** - Interactive plots, dashboards, web interface
- **`utilities/`** - Helper functions, configuration management

## Key Components

### Risk Assessment Engine (R)
- **Viral risk scoring** based on literature-derived factors
- **Geographic risk mapping** using environmental and demographic data  
- **Spillover probability modeling** with Bayesian approaches
- **Uncertainty propagation** through Monte Carlo methods

### Data Pipeline (Python)
- **Automated data collection** from WHO, ECDC, and scientific databases
- **Real-time surveillance processing** for early warning indicators
- **Literature monitoring** for new research on pandemic risk factors
- **Quality assurance** and data validation workflows

### Simulation Core
- **Agent-based modeling** for complex population dynamics
- **Stochastic modeling** for uncertainty in viral evolution and spread
- **Scenario generation** for exploring different risk conditions
- **Intervention modeling** for assessing preparedness measures

## Development Standards

### Code Quality
- **Documentation**: All functions documented with roxygen2 (R) and docstrings (Python)
- **Testing**: Unit tests for critical functions using testthat (R) and pytest (Python)
- **Style**: Following tidyverse style guide (R) and PEP 8 (Python)
- **Version control**: Semantic versioning with clear commit messages

### Dependencies
- **R packages**: tidyverse, ggplot2, dplyr, epidemics, BayesianTools
- **Python packages**: pandas, numpy, scipy, plotly, requests, beautifulsoup4
- **Environment management**: renv (R) and conda/pip (Python)

## Getting Started

1. **Setup environments**: Run `setup_environment.R` and `setup_environment.py`
2. **Install dependencies**: Use `renv::restore()` and `pip install -r requirements.txt`
3. **Run tests**: Execute test suites to verify installation
4. **Example workflow**: Follow tutorials in `examples/` directory

## Performance Considerations

- **Parallel processing** implemented for computationally intensive operations
- **Memory management** optimized for large datasets
- **Caching strategies** for expensive API calls and calculations
- **Profiling tools** integrated for performance monitoring
