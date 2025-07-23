# Outputs Directory 📈

Generated results, visualizations, reports, and model outputs from the Next Pandemic Game simulation.

## Structure

### `reports/`
Comprehensive analysis reports:
- **Risk assessment summaries** - Current global pandemic risk levels
- **Scenario analysis** - "What-if" modeling results under different conditions
- **Trend analysis** - Temporal patterns in risk factors and surveillance data
- **Validation reports** - Model performance against historical events

### `visualizations/`
Charts, graphs, and interactive plots:
- **Risk heat maps** - Geographic visualization of pandemic risk
- **Viral family trees** - Phylogenetic relationships and risk scoring
- **Time series plots** - Temporal trends in surveillance indicators
- **Network diagrams** - Host-pathogen interaction networks
- **Dashboard screenshots** - Interactive visualization examples

### `data_products/`
Processed datasets and derived metrics:
- **Risk scores** - Quantitative pandemic risk assessments by pathogen/region
- **Probability estimates** - Statistical forecasts for emergence scenarios
- **Ranking tables** - Prioritized lists of high-risk viral families
- **Summary statistics** - Descriptive analysis of global surveillance data

### `simulations/`
Model run results and scenario outputs:
- **Baseline scenarios** - Standard parameter runs for comparison
- **Sensitivity analyses** - Parameter variation impact assessment
- **Monte Carlo results** - Uncertainty quantification through repeated runs
- **Counterfactual scenarios** - Alternative history "what-if" analyses

## File Naming Convention

- **Reports**: `YYYY-MM-DD_report-type_version.pdf`
- **Visualizations**: `YYYY-MM-DD_plot-description_version.png/html`
- **Data products**: `YYYY-MM-DD_dataset-name_version.csv/json`
- **Simulations**: `YYYY-MM-DD_scenario-name_run-id.csv`

## Reproducibility

All outputs include:
- **Generation timestamp** and software version information
- **Parameter files** used for that specific run
- **Random seeds** for stochastic simulations
- **Source code version** (git commit hash) used

## Archive Policy

- **Working outputs** - Kept in main directory during active development
- **Published results** - Archived in version-tagged folders
- **Large files** (>100MB) - Stored with Git LFS or external storage
- **Temporary files** - Cleaned automatically after successful runs
