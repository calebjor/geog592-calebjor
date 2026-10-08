# Week 07 Homework Geopandas Geoprocessing

## Data

### 1. North Carolina County Boundaries

**File:** `nc_counties.geojson`

**Description:**  
Polygon boundaries for counties in North Carolina. This dataset will be used to relate shellfish lease locations to counties.

**Source:**  
NC OneMap / State of North Carolina GIS

**Original source:**  
https://services.gis.nc.gov/secure/rest/services/ImageryProject/CO20_Summary/MapServer/0

### 2. North Carolina Shellfish Leases

**File:** `nc_shellfish_leases.geojson`

**Description:**  
Polygon locations of shellfish aquaculture leases administered by the North Carolina Division of Marine Fisheries.

**Source:**  
North Carolina Division of Marine Fisheries Shellfish Leasing Tool

**Original source:** 
https://services6.arcgis.com/DZHaqZm9cxOD4CWM/arcgis/rest/services/Shellfish/FeatureServer/8

### 3. North Carolina Beach and Waterfront Access

**File:** `nc_coastal_access.geojson`

**Description:**  
Point locations of public beach and waterfront access sites along the North Carolina coast.

**Source:**  
North Carolina Division of Coastal Management

**Original source:** 
https://services2.arcgis.com/kCu40SDxsCGcuUWO/arcgis/rest/services/DCM_Beach_and_Waterfront_Access/FeatureServer/0

## Geoprocessing Layers

### 4. Shellfish Leases Assigned to Counties

**File:** leases_by_county.geojson

**Description:**
Polygon layer containing 869 shellfish lease records with county assignments determined through a spatial join. Representative points were used to assign each lease to a county while preserving the original polygon geometries.

**Method:** Spatial join

**Input datasets:** nc_shellfish_leases.geojson and nc_counties.geojson

### 5. Coastal Access Sites in Counties with Shellfish Leases

**File:** access_in_lease_counties.geojson

**Description:**
Point layer containing 799 public coastal access sites located within the 10 counties identified as containing shellfish lease records.

**Method:** Clip

**Input datasets:** nc_coastal_access.geojson and county polygons selected using the results of the first spatial join.

### 6. Coastal Access Sites Within 5 km of Shellfish Leases

**File:** access_near_leases.geojson

**Description:**
Point layer containing 377 public coastal access sites within 5 kilometers of mapped shellfish lease records, regardless of lease status.

**Methods:** Buffer and spatial clip

**Input datasets:** nc_shellfish_leases.geojson and the coastal access sites selected in the previous analysis. A projected coordinate system (EPSG:32119) was used to calculate distances in meters.

# County-Level Analysis

### 7. County Shellfish and Coastal Access Summary

**File:** county_shellfish_summary.csv

**Description:**
Nonspatial table containing statistics for 27 North Carolina counties, including:

- Number of active shellfish leases
- Total public coastal access sites
- Number of access sites within 5 km of active leases
- Percentage of access sites within 5 km of active leases

These statistics were calculated using spatial joins, buffering, and county-level aggregation.

### 8. County Shellfish Analysis

**File:** county_shellfish_analysis.geojson

**Description:**
County polygon layer containing the statistics from county_shellfish_summary.csv, joined using the shared county name attribute.

**Method:** Attribute/table join

**Input datasets:** nc_counties.geojson and county_shellfish_summary.csv

This layer was used to create the final county-level choropleth map comparing public access proximity to active shellfish aquaculture leases.