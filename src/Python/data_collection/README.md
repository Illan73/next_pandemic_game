# Data Collection Module

## Overview

The data collection module is responsible for gathering, fetching, and managing real-world epidemiological data to support the Next Pandemic Game simulation engine. This module provides standardized interfaces for accessing various data sources including WHO, CDC, and other public health databases.

## Structure

```
src/Python/data_collection/
├── __init__.py
├── sources/
│   ├── __init__.py
│   ├── who_api.py          # WHO Global Health Observatory data
│   ├── cdc_api.py          # CDC surveillance data
│   ├── ourworld_data.py    # Our World in Data COVID-19 dataset
│   └── custom_feeds.py     # Custom data source integrations
├── collectors/
│   ├── __init__.py
│   ├── base_collector.py   # Abstract base class for collectors
│   ├── disease_data.py     # Disease outbreak and surveillance data
│   ├── demographic_data.py # Population and demographic statistics
│   └── mobility_data.py    # Human mobility and transportation data
├── validators/
│   ├── __init__.py
│   ├── data_validator.py   # Data quality and consistency checks
│   └── schema_validator.py # Schema validation for incoming data
└── utils/
    ├── __init__.py
    ├── rate_limiter.py     # API rate limiting utilities
    ├── cache_manager.py    # Data caching and storage
    └── data_formatter.py   # Data standardization utilities
```

## Key Features

### Data Sources
- **WHO Global Health Observatory**: Disease surveillance, outbreak reports
- **CDC Surveillance Systems**: US-specific epidemiological data
- **Our World in Data**: COVID-19 and historical pandemic data
- **Custom APIs**: Integration with research institutions and health departments

### Data Types Collected
- Disease incidence and prevalence rates
- Mortality and morbidity statistics
- Population demographics and density
- Healthcare system capacity
- Mobility and transportation patterns
- Environmental and climate data

### Quality Assurance
- Automated data validation and cleaning
- Schema compliance checking
- Missing data detection and handling
- Outlier identification and flagging

## Installation

```bash
# Install required dependencies
pip install -r requirements.txt

# Install the module in development mode
pip install -e .
```

## Quick Start

```python
from data_collection import DataCollector
from data_collection.sources import WHOCollector, CDCCollector

# Initialize collectors
who_collector = WHOCollector(api_key="your_api_key")
cdc_collector = CDCCollector()

# Collect disease surveillance data
disease_data = who_collector.get_disease_data(
    disease="influenza",
    countries=["USA", "GBR", "FRA"],
    date_range=("2020-01-01", "2024-01-01")
)

# Collect demographic data
demographic_data = cdc_collector.get_population_data(
    regions=["US-NY", "US-CA"],
    metrics=["population", "age_distribution", "density"]
)
```

## Configuration

Create a `config.yaml` file:

```yaml
data_sources:
  who:
    api_key: ${WHO_API_KEY}
    base_url: "https://ghoapi.azureedge.net/api/"
    rate_limit: 100  # requests per hour
  
  cdc:
    base_url: "https://data.cdc.gov/api/"
    rate_limit: 1000
  
cache:
  enabled: true
  ttl: 3600  # seconds
  backend: "redis"  # or "memory"
  
validation:
  strict_mode: true
  required_fields: ["date", "location", "value"]
```

## API Reference

### BaseCollector

Abstract base class for all data collectors.

```python
class BaseCollector:
    def collect(self, **kwargs) -> Dict[str, Any]:
        """Collect data from the source"""
        pass
    
    def validate(self, data: Dict[str, Any]) -> bool:
        """Validate collected data"""
        pass
```

### WHOCollector

Collects data from WHO Global Health Observatory.

```python
who = WHOCollector(api_key="key")
data = who.get_disease_data(disease="covid-19", country="USA")
```

### CDCCollector

Interfaces with CDC surveillance systems.

```python
cdc = CDCCollector()
surveillance = cdc.get_surveillance_data(system="FluView", week=202401)
```

## Data Schemas

### Disease Data Schema
```json
{
  "date": "2024-01-01",
  "location": {
    "country": "USA",
    "state": "NY",
    "county": "New York"
  },
  "disease": "influenza",
  "metrics": {
    "incidence_rate": 15.2,
    "deaths": 45,
    "hospitalizations": 234
  },
  "confidence_interval": [12.1, 18.3],
  "data_quality": "high"
}
```

## Testing

```bash
# Run all tests
pytest tests/

# Run specific test categories
pytest tests/test_collectors.py
pytest tests/test_validators.py

# Run with coverage
pytest --cov=data_collection tests/
```

## Environment Variables

```bash
export WHO_API_KEY="your_who_api_key"
export CDC_API_KEY="your_cdc_api_key"
export REDIS_URL="redis://localhost:6379"
export DATA_CACHE_TTL="3600"
```

## Error Handling

The module includes comprehensive error handling:

- **NetworkError**: Issues with API connectivity
- **ValidationError**: Data quality or schema violations
- **RateLimitError**: API rate limit exceeded
- **DataNotFoundError**: Requested data unavailable

## Contributing

1. Follow the established patterns in `BaseCollector`
2. Add comprehensive tests for new collectors
3. Update schemas in `schemas/` directory
4. Document new data sources in this README

## License

See project root LICENSE file for details.
