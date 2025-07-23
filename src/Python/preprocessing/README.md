# Preprocessing Module

## Overview

The preprocessing module transforms raw epidemiological data into clean, standardized formats suitable for the Next Pandemic Game simulation engine. This module handles data cleaning, normalization, feature engineering, and preparation for machine learning models and game mechanics.

## Structure

```
src/Python/preprocessing/
├── __init__.py
├── cleaners/
│   ├── __init__.py
│   ├── base_cleaner.py      # Abstract base class for data cleaners
│   ├── missing_data.py      # Missing value imputation strategies
│   ├── outlier_detection.py # Statistical outlier identification
│   └── data_quality.py      # Data quality assessment and repair
├── transformers/
│   ├── __init__.py
│   ├── normalizers.py       # Data normalization and scaling
│   ├── aggregators.py       # Temporal and spatial aggregation
│   ├── feature_engineering.py # Derived feature creation
│   └── time_series.py       # Time series specific transformations
├── validators/
│   ├── __init__.py
│   ├── consistency_check.py # Cross-dataset consistency validation
│   ├── completeness.py      # Data completeness assessment
│   └── accuracy_metrics.py  # Data accuracy measurement
├── pipelines/
│   ├── __init__.py
│   ├── disease_pipeline.py  # Disease data preprocessing pipeline
│   ├── demographic_pipeline.py # Demographic data pipeline
│   └── mobility_pipeline.py # Mobility data preprocessing
└── utils/
    ├── __init__.py
    ├── data_profiler.py     # Automated data profiling
    ├── transformation_log.py # Change tracking and audit trail
    └── benchmark_tools.py   # Performance benchmarking utilities
```

## Key Features

### Data Cleaning
- **Missing Value Handling**: Multiple imputation strategies (mean, median, interpolation, ML-based)
- **Outlier Detection**: Statistical and ML-based anomaly detection
- **Duplicate Removal**: Intelligent deduplication with conflict resolution
- **Data Type Correction**: Automatic type inference and conversion

### Data Transformation
- **Normalization**: Min-max, z-score, robust scaling
- **Aggregation**: Temporal (daily to weekly/monthly) and spatial (county to state/country)
- **Feature Engineering**: Lag features, rolling statistics, epidemiological indicators
- **Time Series Processing**: Seasonality detection, trend decomposition

### Quality Assurance
- **Consistency Validation**: Cross-dataset relationship verification
- **Completeness Assessment**: Missing data pattern analysis
- **Accuracy Metrics**: Comparison with ground truth where available
- **Transformation Auditing**: Complete change history tracking

## Installation

```bash
# Install required dependencies
pip install -r requirements.txt

# Install scientific computing dependencies
pip install pandas numpy scipy scikit-learn

# Install the module
pip install -e .
```

## Quick Start

```python
from preprocessing import DataProcessor
from preprocessing.pipelines import DiseaseDataPipeline

# Initialize processor
processor = DataProcessor(config_path="config/preprocessing.yaml")

# Load raw data
raw_data = pd.read_csv("data/raw/disease_surveillance.csv")

# Create preprocessing pipeline
pipeline = DiseaseDataPipeline(
    missing_strategy="interpolate",
    outlier_method="isolation_forest",
    normalization="minmax"
)

# Process data
cleaned_data = pipeline.fit_transform(raw_data)

# Validate results
quality_report = pipeline.get_quality_report()
print(f"Data quality score: {quality_report['overall_score']}")
```

## Configuration

Example `preprocessing.yaml`:

```yaml
general:
  log_level: "INFO"
  parallel_processing: true
  n_jobs: -1

cleaning:
  missing_data:
    strategy: "multiple_imputation"  # mean, median, interpolate, forward_fill, ml_impute
    max_missing_percentage: 0.3
    
  outliers:
    method: "isolation_forest"  # iqr, zscore, isolation_forest, local_outlier_factor
    contamination: 0.1
    
  duplicates:
    method: "fuzzy_matching"
    threshold: 0.95

transformation:
  normalization:
    method: "robust_scaler"  # minmax, standard, robust, quantile
    
  aggregation:
    temporal:
      from: "daily"
      to: "weekly"
      method: "sum"  # mean, sum, max, min
      
    spatial:
      from: "county"
      to: "state"
      population_weighted: true

feature_engineering:
  lag_features:
    enabled: true
    lags: [1, 7, 14]
    
  rolling_statistics:
    windows: [7, 14, 30]
    statistics: ["mean", "std", "min", "max"]
    
  epidemiological_indicators:
    - "reproduction_number"
    - "doubling_time"
    - "attack_rate"

validation:
  consistency_checks: true
  completeness_threshold: 0.95
  accuracy_benchmarks: true
```

## Processing Pipelines

### Disease Data Pipeline

Specialized pipeline for epidemiological data:

```python
from preprocessing.pipelines import DiseaseDataPipeline

pipeline = DiseaseDataPipeline()
pipeline.add_step("missing_imputation", method="interpolate")
pipeline.add_step("outlier_removal", method="isolation_forest")
pipeline.add_step("feature_engineering", include_lags=True)
pipeline.add_step("normalization", method="robust")

processed_data = pipeline.transform(raw_disease_data)
```

### Demographic Pipeline

For population and demographic data:

```python
from preprocessing.pipelines import DemographicPipeline

demo_pipeline = DemographicPipeline()
demo_pipeline.configure({
    "age_binning": {"bins": [0, 18, 65, 100], "labels": ["child", "adult", "elderly"]},
    "population_density": {"method": "log_transform"},
    "income_normalization": {"method": "percentile_ranks"}
})

processed_demographics = demo_pipeline.transform(demographic_data)
```

## Feature Engineering

### Epidemiological Indicators

```python
from preprocessing.transformers import FeatureEngineering

fe = FeatureEngineering()

# Calculate reproduction number
data_with_rt = fe.calculate_reproduction_number(
    cases_data, 
    serial_interval=7,
    window_size=14
)

# Add lag features
lagged_data = fe.add_lag_features(
    data_with_rt,
    columns=["daily_cases", "daily_deaths"],
    lags=[1, 7, 14]
)

# Rolling statistics
final_data = fe.add_rolling_statistics(
    lagged_data,
    windows=[7, 14, 30],
    statistics=["mean", "std", "trend"]
)
```

### Time Series Features

```python
from preprocessing.transformers import TimeSeriesTransformer

ts_transformer = TimeSeriesTransformer()

# Decompose time series
decomposed = ts_transformer.seasonal_decompose(
    time_series_data,
    period=52  # weekly seasonality
)

# Detect change points
change_points = ts_transformer.detect_change_points(
    time_series_data,
    method="pelt",
    min_segment_length=14
)
```

## Data Quality Assessment

### Quality Metrics

```python
from preprocessing.validators import DataQualityAssessor

assessor = DataQualityAssessor()
quality_report = assessor.assess(processed_data)

print(f"""
Quality Assessment:
- Completeness: {quality_report['completeness']:.2%}
- Consistency: {quality_report['consistency']:.2%}
- Accuracy: {quality_report['accuracy']:.2%}
- Timeliness: {quality_report['timeliness']:.2%}
- Overall Score: {quality_report['overall_score']:.2f}/10
""")
```

### Validation Rules

```python
from preprocessing.validators import ValidationRules

rules = ValidationRules()
rules.add_rule("positive_cases", lambda df: (df['cases'] >= 0).all())
rules.add_rule("date_sequence", lambda df: df['date'].is_monotonic_increasing)
rules.add_rule("population_bounds", lambda df: df['population'].between(0, 1e9).all())

validation_results = rules.validate(processed_data)
```

## Performance Optimization

### Parallel Processing

```python
from preprocessing.utils import ParallelProcessor

processor = ParallelProcessor(n_jobs=4)
processed_chunks = processor.process_in_parallel(
    large_dataset,
    chunk_size=10000,
    processing_func=cleanup_function
)
```

### Memory Management

```python
from preprocessing.utils import MemoryOptimizer

optimizer = MemoryOptimizer()
optimized_data = optimizer.optimize_dtypes(large_dataframe)
memory_usage = optimizer.get_memory_usage(optimized_data)
```

## Testing

```bash
# Run all preprocessing tests
pytest tests/preprocessing/

# Test specific components
pytest tests/preprocessing/test_cleaners.py
pytest tests/preprocessing/test_transformers.py
pytest tests/preprocessing/test_pipelines.py

# Performance benchmarks
pytest tests/preprocessing/test_performance.py --benchmark-only
```

## Data Schemas

### Input Schema (Raw Data)
```json
{
  "date": "2024-01-01",
  "location_id": "US-NY-061",
  "cases": 150,
  "deaths": 3,
  "population": 1000000,
  "data_source": "state_health_dept",
  "quality_flags": ["estimated"]
}
```

### Output Schema (Processed Data)
```json
{
  "date": "2024-01-01",
  "location_id": "US-NY-061",
  "cases": 150,
  "cases_per_100k": 15.0,
  "cases_7day_avg": 142.3,
  "cases_lag_7": 134,
  "deaths": 3,
  "death_rate": 0.02,
  "reproduction_number": 1.2,
  "doubling_time": 14.5,
  "population": 1000000,
  "processing_metadata": {
    "processed_at": "2024-01-02T10:30:00Z",
    "pipeline_version": "1.2.0",
    "quality_score": 0.95
  }
}
```

## Error Handling

Common exceptions and handling:

- **MissingDataError**: Too much missing data for reliable processing
- **OutlierError**: Extreme outliers detected that may indicate data issues
- **ConsistencyError**: Cross-dataset validation failures
- **TransformationError**: Mathematical transformation failures

## Contributing

1. Follow the pipeline pattern for new preprocessing workflows
2. Add comprehensive unit tests with sample data
3. Document transformation logic and assumptions
4. Update schema documentation for new features
5. Include performance benchmarks for computationally intensive operations

## License

See project root LICENSE file for details.
