# Data Directory 📊

This directory contains all datasets used in the Next Pandemic Game simulation, organized by source and processing stage.

## Structure

### `raw/`
Unprocessed, original datasets as downloaded or collected:
- **Viral sequence data** from NCBI GenBank, GISAID
- **Spillover event records** from scientific literature and databases
- **Population and demographic data** from UN, World Bank, census data
- **Animal host and reservoir data** from wildlife databases
- **Climate and environmental data** from meteorological services

### `processed/`
Cleaned, standardized datasets ready for analysis:
- **Viral family characteristics** - standardized risk factors, transmission modes
- **Historical pandemic timeline** - dates, locations, impact metrics
- **Host-pathogen networks** - species associations and spillover pathways
- **Geographic risk maps** - spatial data on high-risk regions

### `external/`
Scripts and configurations for real-time data connections:
- **WHO surveillance APIs** - current outbreak monitoring
- **ECDC threat assessments** - European disease surveillance
- **ProMED alerts** - global disease outbreak reports
- **Scientific database APIs** - automated literature monitoring

## Data Sources

### Primary Databases
- **NCBI Virus Database** - Viral genome sequences and taxonomy
- **PREDICT Database** - Wildlife viral surveillance data  
- **Disease Outbreak News (WHO)** - Official outbreak reports
- **GISAID** - Global viral sequence sharing platform

### Literature Sources
- **PubMed/MEDLINE** - Peer-reviewed research articles
- **bioRxiv/medRxiv** - Preprint servers for latest research
- **Cochrane Reviews** - Systematic reviews and meta-analyses

## Data Standards

- **File naming**: `YYYY-MM-DD_source_description.format`
- **Encoding**: UTF-8 for all text files
- **Coordinates**: WGS84 decimal degrees for geographic data
- **Dates**: ISO 8601 format (YYYY-MM-DD)

## Update Schedule

- **Daily**: WHO/ECDC surveillance feeds, news alerts
- **Weekly**: Scientific database queries, literature searches  
- **Monthly**: Population/demographic data updates
- **Quarterly**: Major database snapshots and backups
