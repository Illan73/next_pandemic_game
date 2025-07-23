# Viral Families Data 🧬

Comprehensive database of viral families and their pandemic risk characteristics based on scientific literature and genomic analysis.

## Overview

This directory contains structured data on viral families with documented or potential pandemic risk. Each family is characterized by biological, ecological, and epidemiological factors that influence spillover probability and pandemic potential.

## Viral Family Categories

### High-Risk Pandemic Families

#### 1. Coronaviridae
- **Members**: SARS-CoV, MERS-CoV, SARS-CoV-2, endemic human coronaviruses
- **Host range**: Bats (primary), intermediate hosts (civets, camels, pangolins)
- **Transmission**: Respiratory droplets, aerosols, fomites
- **Pandemic history**: SARS (2003), COVID-19 (2019-present)
- **Risk factors**: High mutation rate, receptor diversity, zoonotic potential
- **R₀ range**: 1.4-6.0 depending on variant and population

#### 2. Orthomyxoviridae (Influenza)
- **Members**: Influenza A, B, C, D viruses
- **Host range**: Birds (primary), mammals (pigs, humans)
- **Transmission**: Respiratory droplets, aerosols
- **Pandemic history**: 1918, 1957, 1968, 2009 pandemics
- **Risk factors**: Antigenic shift, reassortment, broad host range
- **R₀ range**: 1.2-2.0 for seasonal strains, up to 3.0 for pandemic strains

#### 3. Paramyxoviridae
- **Members**: Nipah, Hendra, measles, respiratory syncytial virus
- **Host range**: Bats (Nipah, Hendra), humans (measles)
- **Transmission**: Respiratory, direct contact, contaminated food
- **Pandemic history**: Measles historically, Nipah outbreaks in Asia
- **Risk factors**: High case fatality (Nipah ~70%), respiratory transmission
- **R₀ range**: 12-18 (measles), 0.3-0.5 (Nipah - limited human transmission)

#### 4. Filoviridae
- **Members**: Ebola virus, Marburg virus
- **Host range**: Bats (suspected), primates (intermediate)
- **Transmission**: Direct contact with bodily fluids
- **Pandemic history**: Multiple outbreaks, 2014-2016 West Africa epidemic
- **Risk factors**: Very high case fatality (25-90%), healthcare transmission
- **R₀ range**: 1.5-2.5 in community settings, higher in healthcare settings

### Moderate-Risk Families

#### 5. Arenaviridae
- **Members**: Lassa fever, Lujo virus, lymphocytic choriomeningitis virus
- **Host range**: Rodents (primary reservoir)
- **Transmission**: Inhalation of contaminated dust, direct contact
- **Risk factors**: Endemic in West Africa, climate-sensitive distribution
- **R₀ range**: <1 typically (limited human-to-human transmission)

#### 6. Bunyaviridae/Nairoviridae
- **Members**: Rift Valley fever, Crimean-Congo hemorrhagic fever, hantaviruses
- **Host range**: Arthropods (vectors), rodents, livestock
- **Transmission**: Vector-borne, inhalation, direct contact
- **Risk factors**: Climate change expanding vector range
- **R₀ range**: Vector-dependent, typically <1 for human transmission

#### 7. Togaviridae
- **Members**: Chikungunya, Eastern equine encephalitis, Ross River virus
- **Host range**: Birds, mammals, mosquito vectors
- **Transmission**: Mosquito-borne
- **Risk factors**: Urban adaptation, climate change effects
- **R₀ range**: Vector-dependent, can cause large outbreaks

#### 8. Flaviviridae
- **Members**: Zika, dengue, yellow fever, West Nile virus
- **Host range**: Birds, primates, mosquito vectors
- **Transmission**: Mosquito-borne, some sexual transmission (Zika)
- **Risk factors**: Urban mosquito adaptation, climate change
- **R₀ range**: 2-7 for dengue, 2-3 for Zika in suitable vector areas

### Emerging Concern Families

#### 9. Orthopoxviridae
- **Members**: Monkeypox, vaccinia, cowpox
- **Host range**: Rodents, primates
- **Transmission**: Direct contact, respiratory (close contact)
- **Risk factors**: Waning smallpox immunity, international spread
- **R₀ range**: 0.6-1.0 (monkeypox 2022 outbreak)

#### 10. Rhabdoviridae
- **Members**: Rabies virus, vesicular stomatitis virus
- **Host range**: Mammals, arthropods
- **Transmission**: Animal bites, vector-borne
- **Risk factors**: Nearly 100% case fatality if untreated
- **R₀ range**: <1 (no sustained human transmission)

## Risk Assessment Metrics

### Biological Characteristics
- **Genome type**: RNA vs DNA (RNA viruses typically higher mutation rates)
- **Genome size**: Smaller genomes often more adaptable
- **Replication strategy**: Error rates and mutation accumulation
- **Host cell receptors**: Breadth of cellular tropism
- **Immune evasion**: Mechanisms to avoid host defenses

### Ecological Factors
- **Reservoir host abundance**: Population size and distribution
- **Host-human contact probability**: Interface frequency and intensity
- **Environmental stability**: Survival outside hosts
- **Vector ecology**: For vector-borne viruses, vector distribution and behavior
- **Seasonality**: Climate-dependent transmission patterns

### Epidemiological Parameters
- **Basic reproduction number (R₀)**: Inherent transmissibility
- **Serial interval**: Time between successive infections
- **Incubation period**: Time from infection to symptoms
- **Infectious period**: Duration of viral shedding
- **Case fatality rate**: Proportion of infections resulting in death
- **Age-specific susceptibility**: Demographic risk patterns

### Spillover Risk Factors
- **Historical spillover frequency**: Documented animal-to-human events
- **Geographic spillover hotspots**: Regions with highest spillover probability
- **Anthropogenic drivers**: Human activities increasing spillover risk
- **Surveillance sensitivity**: Probability of detecting spillover events
- **Intervention availability**: Vaccines, therapeutics, public health measures

## Data Structure

family_name, taxonomy_classification, genome_type, genome_size,
host_range, primary_reservoir, transmission_modes, zoonotic_potential,
pandemic_history, current_distribution, surveillance_priority

### Species-Level Data

species_name, family, R0_estimate, case_fatality_rate, incubation_period,
infectious_period, mutation_rate, antigentic_diversity, vaccine_available,
therapeutic_options, diagnostic_tests, outbreak_frequency

### Risk Scoring

family_name, spillover_risk_score, transmission_potential, severity_score,
countermeasure_availability, overall_pandemic_risk, confidence_interval,
last_updated, data_sources, expert_consensus_score

## Literature Integration

### Primary Sources
- **Virology journals**: Nature Microbiology, Journal of Virology, PLoS Pathogens
- **Epidemiology journals**: Lancet, NEJM, Emerging Infectious Diseases
- **Surveillance reports**: WHO, CDC, ECDC technical reports
- **Genomic databases**: GenBank, GISAID, Virus Pathogen Database

### Key Review Papers
- Carroll et al. (2018) - "The Global Virome Project"
- Morse et al. (2012) - "Prediction and prevention of next pandemic"
- Olival et al. (2017) - "Host and viral traits predict zoonotic spillover"
- Johnson et al. (2020) - "Global shifts in mammalian population trends"

### Database Cross-References
- **NCBI Taxonomy**: Standardized viral classification
- **ICTV**: International Committee on Taxonomy of Viruses
- **ViralZone**: Swiss-Prot viral protein database
- **Virus-Host DB**: Documented virus-host associations

## Risk Model Integration

### Quantitative Parameters
- **Spillover probability models**: Bayesian inference from historical data
- **Transmission dynamics**: Mathematical models (SIR, SEIR, agent-based)
- **Geographic risk mapping**: Species distribution models
- **Uncertainty quantification**: Monte Carlo simulation of parameter uncertainty

### Qualitative Assessments
- **Expert elicitation**: Structured expert judgment protocols
- **Consensus building**: Delphi method for uncertain parameters
- **Scenario planning**: Structured "what-if" analysis
- **Red team exercises**: Challenge assumptions and model structure

## Update Protocols

### Routine Updates
- **Monthly**: New literature screening and integration
- **Quarterly**: Risk score recalculation with new data
- **Annually**: Comprehensive review and model validation
- **Event-driven**: Immediate updates following significant outbreaks

### Quality Control
- **Data validation**: Cross-reference multiple sources
- **Expert review**: Domain specialist verification
- **Version control**: All changes tracked and documented
- **Reproducibility**: All analyses scripted and version-controlled

This viral families database provides the biological foundation for pandemic risk assessment and guides surveillance and preparedness priorities.


### Family-Level Data
