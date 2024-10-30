# Data-512 Part 1: Common Analysis

## Project Overview
This project aims to analyze the annual wildfire smoke impact on air quality in the Assigned City (Springfield, MA here) for the past 60 years. Using geospatial and historical wildfire data, we estimate the smoke impact from wildfires within a 650-mile radius of the city. By quantifying smoke impact over time, this analysis can inform policymakers and city officials about trends and potential mitigative actions for the future. The final model will also predict annual wildfire smoke estimates for the city over the next 25 years (2025-2050).

## Research Implications

- Estimate Annual Smoke Impact: Quantify the impact of wildfires on air quality in Springfield over the past six decades.
- Comparison with AQI Data: Compare these estimates with available AQI (Air Quality Index) data from the US Environmental Protection Agency (EPA) to validate the results.
- Predictive Model: Develop a predictive model to project wildfire smoke impacts for 2025-2050.
- Visualizations: Visualize the spatial and temporal aspects of wildfire frequency and the corresponding smoke impact on the given city

## Methodology

### Data Acquisition
- Wildfire Data: The analysis uses the Combined Wildland Fire Datasets for the US and Territories (1800s-Present), provided by the US Geological Survey.
- Air Quality Data: AQI data is sourced from the US EPA’s Air Quality System (AQS) API, focusing on counties or areas within proximity to the city.
- Distance Criteria: We consider wildfires up to 650 miles from the city to account for regional impacts, especially during peak wildfire months (May through October).

### Smoke Impact Estimation
- The smoke impact metric is developed by evaluating wildfire characteristics such as burn area and proximity to the city.
- We assume larger and closer fires have a more significant effect on the city's air quality than smaller or distant fires.
- Each year's smoke impact is the aggregate of fire impacts within 650 miles of the city during that year's fire season.
  
### Comparison with AQI Data
- AQI data is used to validate the estimated smoke impact. As the EPA’s AQI records start in the 1980s and vary in availability, data interpolation or extrapolation may be necessary for consistency.
- Comparisons are made based on the temporal trends observed in both datasets, examining correlations between high smoke-impact years and elevated AQI levels.

### Predictive Model
A predictive model forecasts wildfire smoke impacts for the years 2025-2050 based on historical data. The model is designed to account for uncertainty in both wildfire frequency and intensity due to climate and other factors.

### Visualization
Key visualizations include:
- Distance Histogram: Shows fire occurrences by distance from the city in 50-mile increments, extending up to 1800 miles. The 650-mile cutoff is highlighted.
- Time Series for Burned Acres: Displays annual burn area trends for fires within 650 miles.
- Smoke Impact vs. AQI: Compares annual smoke impact estimates with AQI data, visualizing potential correlations and discrepancies.

## Data Description and Provenance

### Data Source
- [Wildfire Dataset](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81): Provided in ArcGIS and GeoJSON formats by the US Geological Survey, this dataset details wildfire locations and burn areas across the US and its territories.
- [AQI Dataset](https://aqs.epa.gov/aqsweb/documents/data_api.html): Retrieved from the EPA’s AQS API, the AQI data represents air quality metrics for various pollutants. AQI data is used to benchmark smoke estimates against actual air quality readings where available.


### APIs and Tools
- EPA’s AQS API: Accessed to gather AQI data; requires an API key available via EPA’s AQS access request.
- Geospatial Analysis: Python libraries such as GeoPandas are used to filter and analyze fires within specified distances of Springfield

### Libraries and Tools Used
- `pandas`: Data manipulation and analysis.
- `GeoJson`: A library to parse geospatial data based Json files
- `GeoPandas`: Geospatial data handling and distance calculations.
- `requests`: API requests to the EPA’s AQS API.
- `matplotlib`, seaborn: Visualization libraries for plotting data trends and histograms.
- `scikit-learn`: Building the predictive model.

## How to Reproduce the Analysis

To reproduce the analysis, follow these steps:
1. Clone the repository:
```bash
git clone <repository-url>
cd <repository-directory>
```
2. Install the required Python packages:
Install using pip:
```bash
pip install pandas geojson geopandas requests matplotlib seaborn scikit-learn
```
3. Acquire Data:
- Download the wildfire dataset from ScienceBase Catalog.
- Register and obtain an API key from the EPA AQS for AQI data retrieval.

4. Run the Analysis Notebook: Execute the code in [Data Analysis.ipynb]() to reproduce the smoke estimates and visualizations.

5. Review Visualizations: Generated visualizations provide insights into wildfire frequency, burn area, and their annual impact on air quality in Springfield MA.

## APIs Used

### EPA's Air Quality System (AQS) API

The **EPA AQS API** is utilized to retrieve real-time air quality data for the analysis. This API provides access to historical air quality data across the United States, allowing for the validation of smoke impact estimates based on actual air quality measurements.

#### Accessing the AQS API

1. **Obtain an API Key**:
   - To access the AQS API, you must create an account and request an API key from the [EPA AQS API](https://aqs.epa.gov/api/). Follow the instructions provided on their website to complete this process.

2. **Making API Requests**:
   - The API provides various endpoints to retrieve data based on specific parameters, including location, date range, and pollutants.
   - Example endpoint to retrieve AQI data for a specific county:
     ```
     https://aqs.epa.gov/data/api/annualData.v1?email=<your_email>&key=<your_api_key>&bdate=2021-01-01&edate=2021-12-31&param=44201&city=[Your_City]&state=[Your_State]
     ```

#### Example Code for API Request
Here’s a snippet on how to access the AQS API within your code:

```python
import requests
import pandas as pd

def get_aqi_data(city, state, start_date, end_date, api_key):
    url = f"https://aqs.epa.gov/data/api/annualData.v1"
    params = {
        'email': 'your_email@example.com',
        'key': api_key,
        'bdate': start_date,
        'edate': end_date,
        'param': '44201',  # Parameter for PM2.5
        'city': city,
        'state': state
    }
    response = requests.get(url, params=params)
    if response.status_code == 200:
        return pd.DataFrame(response.json()['Data'])
    else:
        print(f"Error: {response.status_code} - {response.text}")
        return None
```


## Files in the Repository

- `Data Analysis.ipynb`: Jupyter Notebook with full code for data acquisition, transformation, smoke estimation, AQI comparison, and visualization.
- `Data 512 Part 1 - Common Analysis.pdf`: Document with the images of Step 2 outputs and reflection on those analysis
- `README.md`: Project description and instructions (this document).
- `LICENSE`: Terms of use and distribution.


## License Information

The code for this project is licensed under the MIT License (see the LICENSE file). 

