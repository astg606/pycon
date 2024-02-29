
<div align="center">
<table>
<tbody>
<td align="center">
<img width="2000" height="0"><br>
<h2>Python Tool for Tracking the Movement of the International Space Station and Identifying the Weather Condition along the Track</h2>
<img width="2000" height="0">
</td>
</tbody>
</table>
</div>

<p>
<b>Jules Kouatchou</b>
NASA/GSFC - SSAI <br />
Code 606 <br />
Greenbelt, MD 20771 <br />
Jules.Kouatchou@nasa.gov
</p>

<p>
<b>Cindy Chen</b>
Acton Boxborough Regional High School <br />
36 Charter Rd, Acton, MA 01720 <br />
cinderellazhouchen@gmail.com 
</p>

<p align="center">
<b>Abstract:</b> <br />
At any given time, it is possible to know the planar position 
(latitude, longitude) of the International Space Station (ISS). 
The ISS cycles the globe every 90 minutes. 
We write a Python application 
(relying on the packages Pandas, Shapely, GeoPandas and MovingPandas) 
that collects a time series (over several hours) of the positions of the ISS, 
identifies the cycles (each time the ISS crosses the central meridian), 
and plots the tracks. 
We also determine the weather current conditions (temperature, wind speed) 
of each land point along the track and perform interactive visualizations.
</p>

## Introduction

## How the time series data was generated

## Processing the data

## Future work

## Conclusion


## References

## Appendix

> [!NOTE]  
> Highlights information that users should take into account, even when skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]  
> Crucial information necessary for users to succeed.

> [!WARNING]  
> Critical content demanding immediate user attention due to potential risks.

> [!CAUTION]
> Negative potential consequences of an action.


### Source code

The code we use to generate the time series dataset is available at:

<p align="center">
<a html="">LINK TO CODE</a>
</p>

<details>
<summary>"Click here to view the code"</summary>
```python
import time
import math
import json
import pandas as pd
import datetime as dt
import requests as reqs
from global_land_mask import globe
import reverse_geocode

def get_country_name(lat: float, lon: float) -> str:
    """
    Extract the country name given the latitude/longitude information.
    This function provides a country name even when a location is on
    the ocean. We wish it was not the case.

    Parameters
    ----------
    lat : float
       Latitude of the location
    lon : float
       Longitude of the location

    Returns
    -------
    country : str
       Country name (empty string if no country)
    """
    # Get location with geocode
    lat_lon = (lat, lon),
    loc_name = reverse_geocode.search(lat_lon)
    return loc_name[0].get('country', '')

def retrieve_weather_data(lat: float, lon: float) -> tuple():
    """
    Extract the current, weather condition (temperature, windspeed)
    at a given location (latitude/longitude).

    Parameters
    ----------
    lat : float
       Latitude of the location
    lon : float
       Longitude of the location

    Returns
    -------
    temp : float
       Current temperature at the location
    windspeed : float
       Current windspeed at the location
    """
    base_url = "https://api.open-meteo.com/v1/forecast"
    payload = {'latitude': lat,
               'longitude': lon,
               'current_weather': 'true'
              }
    resp = reqs.get(base_url, params=payload).json()
    forecast = pd.DataFrame(resp)['current_weather']
    return forecast['temperature'], forecast['windspeed']

def cross_180longitude(lon: float, prev_lon: float) -> bool:
    """
    Given the current longitide (lon) location and the previous one (prev_lon),
    determine if the moving object crossed the longitude 180.
    This function is needed to create a new track when an object moves around
    the earth.

    Parameters
    ----------
    lon : float
       Current longitude between -180 and 180
    prev_lon : float
       Previous longitude between -180 and 180

    Returns
    -------
    flag : bool
       Boolean parameter to determine if the moving object crossed the longitude 180.
    """

    # We cross the longitude 180 when lon and prev_lon have opposite signs
    # and the absolute value of prev_lon is greater than 178.5.
    # Two numbers a & b are of the same sign if |a+b|=|a|+|b|.
    flag = False
    if math.fabs(lon) > 178.5:
        flag = not (math.fabs(lon + prev_lon) == math.fabs(lon) + math.fabs(prev_lon))
    return flag

def track_iss(num_records: int) -> None:
    """
    - Collect the timeseries positions (time, latitude, longitude) of the ISS
    - For each location, determine:
        - the current weather conditions
        - if it is a land or not
    - Create a Pandas DataFrame with all the data and write everything in a CSV file.

    Note that we collect the above data every 5 seconds num_records times.

    Parameters
    ---------
    num_records : int
       Number of records (positions) we want to produce.
    """

    beg_time = dt.datetime.now()
    print()
    print(f"{'-'*75}")
    print()

    #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # Create an empty Pandas DataFrame to store the data
    #           t: date/time
    #    latitude: latitude of location
    #   longitude: longitude of location
    # temperature: current temperature at the location
    #   windspeed: current wind speed at the location
    #   land_flag: flag to determine if location is land (True/False)
    #     traj_id: trajectory id to be used by MovingPandas
    #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    columns = ['t', 'latitude', 'longitude', 'land_flag',
               'temperature', 'windspeed',
               'traj_id', 'country'
              ]
    iss_df = pd.DataFrame(columns=columns)

    #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    # Create a loop to get the ISS info every 5 seconds
    # for a prescribed number of iterations (num_records).
    #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    sleep_time = 5 # in seconds
    count = 0
    traj_id = 0
    while count <= num_records:

        #~~~~~~~~~~~~~~~~~~~~~~~
        # Connect to the ISS API
        #~~~~~~~~~~~~~~~~~~~~~~~
        api_url = "http://api.open-notify.org/iss-now.json"
        response = reqs.get(api_url)
        status = response.status_code

        #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        # Check status code for an appropriate response fromt the API
        #~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        if response.status_code != 200:
            print(f'Error improper response code. Code is {status}')
            return None
        else:
            # Turn the API response into JSON
            resp = response.json()

            # Extract the latitude, longitude
            lat = float(resp['iss_position']['latitude'])
            lon = float(resp['iss_position']['longitude'])#%360

            # Extract the timestamp and convert it to a datetime object
            tim = float(resp['timestamp'])
            dt_obj = dt.datetime.fromtimestamp(tim)

            # Determine if a location is a land or not
            land_flag = globe.is_land(lat, lon)

            temp, wind = retrieve_weather_data(lat, lon)
            country = get_country_name(lat, lon)
            #if land_flag:
            #    country = get_country_name(lat, lon)
            #    #temp, wind = retrieve_weather_data(lat, lon)
            #else:
            #    country = ''
            #    #temp, wind = -9999.0, -9999.0

            # Determine the trajectory id
            # A new trajectory is created whenever we pass the central meridian
            # A trajectory id is for a pass with longitude between -180 and 180
            if count == 0:
                prev_lon = lon

            if cross_180longitude(lon, prev_lon):
                traj_id += 1

            # Add a new row to the DataFrame
            data = [dt_obj, lat, lon, land_flag, temp, wind, traj_id, country]
            iss_df.loc[len(iss_df.index)] = data

            # pause the loop for 5 seconds to allow the ISS to move slightly
            time.sleep(sleep_time)

            # Add to the count so it doesn't access the API too many times
            count += 1
            prev_lon = lon

            print(f"{count:>5} "
                  f"Lat/Lon: {lat:.3f}/{lon:.3f} "
                  f"traj_id: {traj_id:>2} "
                  f"Land: {int(land_flag)} "
                  f"Country: {country}"
                 )

    #~~~~~~~~~~~~~~~~~~~
    # Save in a cvs file
    #~~~~~~~~~~~~~~~~~~~
    end_time = dt.datetime.now()
    file_name = beg_time.strftime("iss_timeseries_trajectories_%Y%m%d_%H%M%S.csv")
    iss_df.to_csv(file_name, sep=',', index=False)

    print()
    print(f"Data written into: {file_name}")
    print()
    print(f"{'-'*75}")
    print(f'Starting date: {beg_time.strftime("%Y/%m/%d %H/%M/%S")}')
    print(f'Ending date:   {end_time.strftime("%Y/%m/%d %H/%M/%S")}')
    print(f"Elapsed time:  {(end_time-beg_time).total_seconds()} s")
    print(f"{'-'*75}")
    print()

if __name__ == '__main__':

    num_records = 2000
    track_iss(num_records)
```
</details>

