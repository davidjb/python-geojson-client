# python-geojson-client

[![Build Status](https://travis-ci.org/exxamalte/python-geojson-client.svg)](https://travis-ci.org/exxamalte/python-geojson-client)
[![Coverage Status](https://coveralls.io/repos/github/exxamalte/python-geojson-client/badge.svg?branch=master)](https://coveralls.io/github/exxamalte/python-geojson-client?branch=master)
[![PyPi](https://img.shields.io/pypi/v/geojson-client.svg)](https://pypi.python.org/pypi/geojson-client)
[![Version](https://img.shields.io/pypi/pyversions/geojson-client.svg)](https://pypi.python.org/pypi/geojson-client)
[![Maintainability](https://api.codeclimate.com/v1/badges/8c87f480c5043a8599df/maintainability)](https://codeclimate.com/github/exxamalte/python-geojson-client/maintainability)

This library provides convenient access to [GeoJSON](https://tools.ietf.org/html/rfc7946) Feeds.


## Installation
`pip install geojson-client`

## Usage
See below for examples of how this library can be used for particular GeoJSON feeds. After instantiating a particular class and supply the required parameters, you can call `update` to retrieve the feed data. The return value will be a tuple of a status code and the actual data in the form of a list of feed entries specific to the selected feed.

Status Codes
* _UPDATE_OK_: Update went fine and data was retrieved. The library may still return empty data, for example because no entries fulfilled the filter criteria.
* _UPDATE_OK_NO_DATA_: Update went fine but no data was retrieved, for example because the server indicated that there was not update since the last request.
* _UPDATE_ERROR_: Something went wrong during the update

## Supported GeoJSON Feeds

### Generic Feed

**Supported Filters**

| Filter |                 | Description |
|--------|-----------------|-------------|
| Radius | `filter_radius` | Radius in kilometers around the home coordinates in which events from feed are included. |

**Example**
```python
from geojson_client.generic_feed import GenericFeed
# Home Coordinates: Latitude: -33.0, Longitude: 150.0
# Filter radius: 500 km
feed = GenericFeed((-33.0, 150.0), filter_radius=500, 
                   url="https://earthquake.usgs.gov/earthquakes/feed/v1.0/summary/all_day.geojson")
status, entries = feed.update()
```

### [NSW Rural Fire Service](https://www.rfs.nsw.gov.au/fire-information/fires-near-me)

**Supported Filters**

| Filter     |                     | Description |
|------------|---------------------|-------------|
| Radius     | `filter_radius`     | Radius in kilometers around the home coordinates in which events from feed are included. |
| Categories | `filter_categories` | Array of category names. Only events with a category matching any of these is included. |

**Example**
```python
from geojson_client.nsw_rural_fire_service_feed import NswRuralFireServiceFeed
# Home Coordinates: Latitude: -33.0, Longitude: 150.0
# Filter radius: 50 km
# Filter categories: 'Advice'
feed = NswRuralFireServiceFeed((-33.0, 150.0), filter_radius=50, filter_categories=['Advice'])
status, entries = feed.update()
```

### [U.S. Geological Survey Earthquake Hazards Program](https://earthquake.usgs.gov/earthquakes/feed/v1.0/geojson.php)

**Supported Feeds**

| Category                               | Feed                                 |
|----------------------------------------|--------------------------------------|
| Past Hour - Significant Earthquakes    | `past_hour_significant_earthquakes`  |
| Past Hour - M4.5+ Earthquakes          | `past_hour_m45_earthquakes`          |
| Past Hour - M2.5+ Earthquakes          | `past_hour_m25_earthquakes`          |
| Past Hour - M1.0+ Earthquakes          | `past_hour_m10_earthquakes`          |
| Past Hour - All Earthquakes            | `past_hour_all_earthquakes`          |
| Past Day - Significant Earthquakes     | `past_day_significant_earthquakes`   |
| Past Day - M4.5+ Earthquakes           | `past_day_m45_earthquakes`           |
| Past Day - M2.5+ Earthquakes           | `past_day_m25_earthquakes`           |
| Past Day - M1.0+ Earthquakes           | `past_day_m10_earthquakes`           |
| Past Day - All Earthquakes             | `past_day_all_earthquakes`           |
| Past 7 Days - Significant Earthquakes  | `past_week_significant_earthquakes`  |
| Past 7 Days - M4.5+ Earthquakes        | `past_week_m45_earthquakes`          |
| Past 7 Days - M2.5+ Earthquakes        | `past_week_m25_earthquakes`          |
| Past 7 Days - M1.0+ Earthquakes        | `past_week_m10_earthquakes`          |
| Past 7 Days - All Earthquakes          | `past_week_all_earthquakes`          |
| Past 30 Days - Significant Earthquakes | `past_month_significant_earthquakes` |
| Past 30 Days - M4.5+ Earthquakes       | `past_month_m45_earthquakes`         |
| Past 30 Days - M2.5+ Earthquakes       | `past_month_m25_earthquakes`         |
| Past 30 Days - M1.0+ Earthquakes       | `past_month_m10_earthquakes`         |
| Past 30 Days - All Earthquakes         | `past_month_all_earthquakes`         |

**Supported Filters**

| Filter            |                            | Description |
|-------------------|----------------------------|-------------|
| Radius            | `filter_radius`            | Radius in kilometers around the home coordinates in which events from feed are included. |
| Minimum Magnitude | `filter_minimum_magnitude` | Minimum magnitude as float value. Only event with a magnitude equal or above this value are included. |

**Example**
```python
from geojson_client.usgs_earthquake_hazards_program_feed import UsgsEarthquakeHazardsProgramFeed
# Home Coordinates: Latitude: 21.3, Longitude: -157.8
# Feed: Past Day - All Earthquakes
# Filter radius: 500 km
# Filter minimum magnitude: 4.0
feed = UsgsEarthquakeHazardsProgramFeed((21.3, -157.8), 'past_day_all_earthquakes', 
                                        filter_radius=500, filter_minimum_magnitude=4.0)
status, entries = feed.update()
```
