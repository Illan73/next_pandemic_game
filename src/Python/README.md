# Visualization Module

## Overview

The visualization module provides comprehensive data visualization and interactive dashboard capabilities for the Next Pandemic Game. It transforms epidemiological data into intuitive charts, maps, and interactive displays that support both gameplay elements and scientific analysis.

## Structure

```
src/Python/visualization/
├── __init__.py
├── charts/
│   ├── __init__.py
│   ├── base_chart.py        # Abstract base class for all charts
│   ├── epidemiological.py   # Disease-specific visualizations
│   ├── geographic.py        # Maps and spatial visualizations
│   ├── time_series.py       # Temporal trend visualizations
│   └── comparative.py       # Multi-dataset comparison charts
├── dashboards/
│   ├── __init__.py
│   ├── game_dashboard.py    # Main game interface dashboard
│   ├── analytics_dashboard.py # Scientific analysis dashboard
│   ├── real_time_monitor.py # Live data monitoring
│   └── scenario_comparison.py # Scenario analysis interface
├── interactive/
│   ├── __init__.py
│   ├── map_widgets.py       # Interactive map components
│   ├── timeline_slider.py   # Time navigation controls
│   ├── parameter_controls.py # Model parameter adjustment
│   └── data_explorer.py     # Interactive data exploration
├── exports/
│   ├── __init__.py
│   ├── static_exports.py    # PNG, PDF, SVG export utilities
│   ├── interactive_exports.py # HTML export with interactivity
│   └── report_generator.py  # Automated report generation
└── utils/
    ├── __init__.py
    ├── color_schemes.py     # Consistent color palettes
    ├── styling.py           # Chart styling and themes
    ├── data_adapters.py     # Data format conversion utilities
    └── performance.py       # Rendering optimization
```

## Key Features

### Chart Types
- **Epidemiological Curves**: Case counts, incidence rates, mortality trends
- **Geographic Maps**: Choropleth maps, bubble maps, heat maps
- **Network Visualizations**: Disease transmission networks, contact tracing
- **Comparative Analysis**: Side-by-side scenario comparisons
- **Real-time Monitoring**: Live updating dashboards

### Interactive Elements
- **Zoom and Pan**: Detailed exploration of time series and maps
- **Filtering**: Dynamic data filtering by location, time, demographics
- **Parameter Controls**: Real-time model parameter adjustment
- **Drill-down**: Hierarchical data exploration (country → state → county)
- **Animation**: Temporal disease spread visualization

### Export Capabilities
- **Static Formats**: High-resolution PNG, PDF, SVG
- **Interactive HTML**: Standalone interactive visualizations
- **Automated Reports**: Scheduled report generation
- **Presentation Mode**: Full-screen dashboard displays

## Installation

```bash
# Install required dependencies
pip install -r requirements.txt

# Install visualization libraries
pip install plotly dash bokeh matplotlib seaborn folium

# Install the module
pip install -e .
```

## Quick Start

### Basic Epidemiological Chart

```python
from visualization.charts import EpidemiologicalChart
import pandas as pd

# Load your data
data = pd.read_csv("processed_data/disease_trends.csv")

# Create epidemic curve
epi_chart = EpidemiologicalChart(
    data=data,
    x_column="date",
    y_column="daily_cases",
    title="COVID-19 Daily Cases"
)

# Customize appearance
epi_chart.set_style(
    color_scheme="epidemic",
    show_trend_line=True,
    highlight_peaks=True
)

# Display or save
epi_chart.show()
epi_chart.save("epidemic_curve.png", dpi=300)
```

### Interactive Geographic Map

```python
from visualization.charts import GeographicChart

# Create choropleth map
geo_chart = GeographicChart(
    data=county_data,
    geographic_level="county",
    color_column="incidence_rate",
    hover_columns=["cases", "population", "death_rate"]
)

# Add interactivity
geo_chart.add_time_slider(date_column="date")
geo_chart.add_zoom_controls()
geo_chart.set_color_scale("Reds", reverse=False)

# Display interactive map
geo_chart.show()
```

### Dashboard Creation

```python
from visualization.dashboards import GameDashboard

# Initialize dashboard
dashboard = GameDashboard(
    data_sources={
        "cases": cases_data,
        "demographics": demo_data,
        "interventions": intervention_data
    }
)

# Add components
dashboard.add_overview_panel()
dashboard.add_geographic_panel()
dashboard.add_intervention_controls()
dashboard.add_scenario_comparison()

# Launch dashboard
dashboard.run(host="localhost", port=8050, debug=True)
```

## Configuration

Example `visualization_config.yaml`:

```yaml
# Global visualization settings
global:
  theme: "pandemic_game"
  default_width: 800
  default_height: 600
  color_palette: "viridis"
  font_family: "Arial"

# Chart-specific settings
charts:
  epidemiological:
    default_chart_type: "line"
    show_confidence_intervals: true
    highlight_weekends: true
    
  geographic:
    default_projection: "mercator"
    default_zoom: 4
    tile_server: "openstreetmap"
    
  time_series:
    default_aggregation: "daily"
    show_trend_lines: true
    seasonal_adjustment: false

# Dashboard settings
dashboards:
  refresh_interval: 30  # seconds
  cache_duration: 300   # seconds
  max_data_points: 10000
  
# Export settings
exports:
  image_dpi: 300
  vector_format: "svg"
  include_metadata: true
  
# Performance settings
performance:
  enable_webgl: true
  data_sampling_threshold: 50000
  lazy_loading: true
```

## Chart Types

### Epidemiological Charts

```python
from visualization.charts import EpidemiologicalChart

# Epidemic curve
epidemic_curve = EpidemiologicalChart.create_epidemic_curve(
    data, 
    cases_column="daily_cases",
    deaths_column="daily_deaths"
)

# SIR model visualization
sir_chart = EpidemiologicalChart.create_sir_visualization(
    susceptible=S_data,
    infected=I_data,
    recovered=R_data
)

# Reproduction number over time
rt_chart = EpidemiologicalChart.create_rt_chart(
    rt_data,
    confidence_intervals=True
)
```

### Geographic Visualizations

```python
from visualization.charts import GeographicChart

# Choropleth map
choropleth = GeographicChart.create_choropleth(
    data=regional_data,
    locations="state_code",
    values="incidence_rate",
    color_scale="Reds"
)

# Bubble map for absolute numbers
bubble_map = GeographicChart.create_bubble_map(
    data=city_data,
    lat="latitude",
    lon="longitude",
    size="total_cases",
    color="mortality_rate"
)

# Heat map overlay
heat_map = GeographicChart.create_heat_map(
    data=point_data,
    lat="lat",
    lon="lon",
    intensity="case_density"
)
```

### Time Series Analysis

```python
from visualization.charts import TimeSeriesChart

# Multi-series comparison
comparison = TimeSeriesChart.create_comparison(
    data=multi_region_data,
    x="date",
    y="cases_per_100k",
    group_by="region",
    normalize=True
)

# Forecasting visualization
forecast = TimeSeriesChart.create_forecast_chart(
    historical_data=past_data,
    forecast_data=predicted_data,
    confidence_bands=prediction_intervals
)
```

## Interactive Dashboards

### Game Dashboard

Main interface for the pandemic simulation game:

```python
from visualization.dashboards import GameDashboard

game_dash = GameDashboard()
game_dash.add_world_map()
game_dash.add_epidemic_curves()
game_dash.add_intervention_panel()
game_dash.add_score_tracker()
game_dash.add_news_feed()

game_dash.run()
```

### Analytics Dashboard

Scientific analysis and research interface:

```python
from visualization.dashboards import AnalyticsDashboard

analytics = AnalyticsDashboard()
analytics.add_data_explorer()
analytics.add_statistical_summaries()
analytics.add_model_diagnostics()
analytics.add_parameter_sensitivity()

analytics.run()
```

## Interactive Components

### Map Widgets

```python
from visualization.interactive import MapWidget

map_widget = MapWidget(
    initial_data=geo_data,
    zoom_level=4,
    center=[39.8283, -98.5795]  # US center
)

# Add layers
map_widget.add_choropleth_layer("cases", color_scale="Reds")
map_widget.add_bubble_layer("hospitals", size_column="capacity")
map_widget.add_route_layer("supply_chains")

# Add controls
map_widget.add_layer_selector()
map_widget.add_time_slider()
```

### Parameter Controls

```python
from visualization.interactive import ParameterControls

controls = ParameterControls()
controls.add_slider("transmission_rate", min=0, max=2, step=0.1, value=1.0)
controls.add_dropdown("intervention", options=["none", "lockdown", "masks"])
controls.add_date_range("simulation_period", start="2024-01-01", end="2024-12-31")

# Link to model updates
controls.on_change(callback=update_simulation)
```

## Styling and Themes

### Color Schemes

```python
from visualization.utils import ColorSchemes

# Predefined pandemic-themed palettes
epidemic_colors = ColorSchemes.get_epidemic_palette()
severity_colors = ColorSchemes.get_severity_scale()
intervention_colors = ColorSchemes.get_intervention_palette()

# Custom color mapping
custom_colors = ColorSchemes.create_custom_palette(
    colors=["#FF6B35", "#F7931E", "#FFD23F", "#06FFA5", "#118AB2"],
    name="pandemic_response"
)
```

### Chart Styling

```python
from visualization.utils import ChartStyling

styling = ChartStyling()
styling.set_theme("pandemic_game")
styling.apply_branding(logo_path="assets/logo.png")
styling.set_accessibility_features(
    high_contrast=True,
    screen_reader_compatible=True
)
```

## Export and Reporting

### Static Exports

```python
from visualization.exports import StaticExporter

exporter = StaticExporter()

# High-quality image export
exporter.export_chart(
    chart_object,
    filename="epidemic_analysis.png",
    dpi=300,
    format="png"
)

# Multi-page PDF report
exporter.create_pdf_report(
    charts=[chart1, chart2, chart3],
    filename="pandemic_report.pdf",
    include_metadata=True
)
```

### Interactive Exports

```python
from visualization.exports import InteractiveExporter

interactive_exporter = InteractiveExporter()

# Standalone HTML with full interactivity
interactive_exporter.export_dashboard(
    dashboard_object,
    filename="interactive_dashboard.html",
    include_dependencies=True
)
```

### Automated Reporting

```python
from visualization.exports import ReportGenerator

report_gen = ReportGenerator()
report_gen.schedule_report(
    template="weekly_summary",
    data_source="live_api",
    output_format="pdf",
    schedule="weekly",
    recipients=["team@example.com"]
)
```

## Performance Optimization

### Large Dataset Handling

```python
from visualization.utils import PerformanceOptimizer

optimizer = PerformanceOptimizer()

# Data sampling for large datasets
sampled_data = optimizer.smart_sample(
    large_dataset,
    target_size=10000,
    preserve_patterns=True
)

# Progressive loading for dashboards
optimizer.enable_progressive_loading(dashboard)
```

## Testing

```bash
# Run visualization tests
pytest tests/visualization/

# Visual regression testing
pytest tests/visualization/test_visual_regression.py

# Performance benchmarks
pytest tests/visualization/test_performance.py --benchmark-only

# Dashboard integration tests
pytest tests/visualization/test_dashboards.py --selenium
```

## API Reference

### BaseChart Class

```python
class BaseChart:
    def __init__(self, data, **kwargs):
        pass
    
    def set_style(self, **style_kwargs):
        pass
    
    def add_annotation(self, text, x, y):
        pass
    
    def show(self):
        pass
    
    def save(self, filename, **export_kwargs):
        pass
```

### Dashboard Class

```python
class Dashboard:
    def add_component(self, component, layout_position):
        pass
    
    def set_layout(self, rows, columns):
        pass
    
    def add_callback(self, inputs, outputs, callback_func):
        pass
    
    def run(self, host="localhost", port=8050):
        pass
```

## Examples and Tutorials

See the `examples/` directory for:
- Basic chart creation tutorials
- Interactive dashboard examples
- Custom visualization development
- Integration with game mechanics
- Performance optimization examples

## Contributing

1. Follow the established chart class hierarchy
2. Maintain consistent styling and color schemes
3. Add comprehensive visual tests
4. Document all interactive features
5. Include accessibility considerations
6. Update example notebooks for new features

## Browser Compatibility

- Chrome 80+
- Firefox 75+
- Safari 13+
- Edge 80+

Note: Some advanced features may require modern browser capabilities.

## License

See project root LICENSE file for details.
