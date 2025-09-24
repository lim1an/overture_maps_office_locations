# Processed Corporate Office Locations from Overture Maps

## Overview
This repository provides processed datasets of corporate office locations derived from **[Overture Maps](https://docs.overturemaps.org/)**, a modern, open-source geospatial dataset.

The data covers selected metropolitan areas in the United States. For each firm, we extract candidate office locations from Overture Maps and apply rule-based filters to distinguish likely corporate offices from other types of locations (e.g., retail, warehouses) or places with similar names.


## How the Processed Data Was Generated
The processed data was created in three main steps:  
1. **Raw data extraction** – Downloading Overture Maps [building](https://docs.overturemaps.org/guides/buildings/#14/32.58453/-117.05154/0/60) and [place](https://docs.overturemaps.org/guides/places/#14/32.58453/-117.05154/0/60) layers for metropolitan areas of interest.  
2. **Firm-specific filtering** – Matching potential locations using company name patterns, alternative spellings, and industry context. See configs files.  
3. **Office classification** – Applying rule-based filters (based on industry-specific categories and keywords) to identify likely corporate office locations and exclude retail or non-office sites. See configs files.  

The full raw Overture Maps datasets are not included here. Instead, we publish only the processed CSV outputs for each firm and city.

## Repository Structure

```
├── configs/ 
│ ├── firm_configs_overture/ # Firm configurations for Overture processing 
│ │ ├── firm_configs_a.yaml
│ │ ├── firm_configs_b.yaml
│ │ ├── ...
│ └── configs_industry_overture.yaml # Industry-specific rules for Overture 
└── office_overture/ # Processed Overture results by firm CSVs (`is_office` results included)
```

- **configs/** contains YAML files that define rules used to identify office locations.  
  - *Firm configs* include company name patterns, alternative names, and exclusions.  
  - *Industry configs* contain general office-identifying keywords and category filters.  
- **office_overture/** contains the final cleaned datasets in CSV format.

## Example Firm Config (YAML)

```yaml
firm_info:
  qcom:
    name_pattern:
      - "Qualcomm"
      - "QCOM"
    industry:
      - "technology"
    office_categories: []
    business_categories: []
    exclusions:
      name: []
      primary_category: []
```

## Data Dictionary
Each CSV in `office_overture/` corresponds to a single firm and 169 metropolitan cities, and contains the following fields:  

- **id_left** – Place identifier from Overture Maps   
- **primary_name_left** – Place name from Overture Maps  
- **primary_category** & **alternate_category**– Place category values from Overture Maps  
- **locality** – Address information from Overture Maps 
- **region** – Standardized region information  
- **zipcode** – Standardized zipcode information  
- **address** – Standardized address information  
- **latitude** & **longitude**– Location geometry (latitude/longitude) 
- **source_city** - The metropolitan cities that are defined differently from the city in the address information 
- **industry** – Industry classification of the firm which the office is operating for (from configs)  
- **is_office_by_pipeline** – Boolean flag indicating whether the location is classified as a corporate office  
- **id_right** – Building identifier from Overture Maps   
- **primary_name_right** – Building name from Overture Maps  
- **subtype** & **class** – Building information from Overture Maps  
- **plc_source_dataset** & **plc_source_update_time** & **bldg_dataset** & **bldg_update_time** – Data source information from Overture Maps (where and when Overture pulls the data)
- **plc_source_confidence** & **bldg_confidence**– How confident Overture believes that the place or building is still open/operating


## License
The processed data builds on **Overture Maps** datasets. Please refer to the [Overture Maps license](https://overturemaps.org) for terms of use.
