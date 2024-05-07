
<div align="center">
<table>
<tbody>
 <tr>
    <td>
       <img src="https://portal.nccs.nasa.gov/datashare/astg/training/python/logos/nasa-logo.svg" width="100" hspace="90">
       <img src="https://portal.nccs.nasa.gov/datashare/astg/training/python/logos/ASTG_logo.png?raw=true" width="80" hspace="130">
        <img src="https://www.nccs.nasa.gov/sites/default/files/NCCS_Logo_0.png" width="130">
    </td>
 </tr>
 <tr>
<td align="center">
<img width="2000" height="0"><br>
<h1> PyCon 2024 Tutorial </h1>
<h2>Python Workflows to Extract and Plot Satellite Data Products along Tracks</h2>
<img width="2000" height="0">
</td>
 </tr>
</tbody>
</table>
</div>

[Jules Kouatchou](mailto:Jules.Kouatchou@nasa.gov) • [Bruce Van Aartsen](mailto:bruce.vanaartsen@nasa.gov) • [Website](https://github.com/astg606/pycon/tree/main/year2024/tutorial)

This tutorial aims to assist participants in writing Python workflows 
to monitor the movement of a satellite or any moving planar object. 
The goal is to extract data from its measurements, generate plots 
along its trajectory, and evaluate the effectiveness of geophysics and 
atmospheric models. 
Specifically, we will concentrate on utilizing data from the 
Ozone Monitoring Instrument (OMI) aboard the Aura satellite. 
To accomplish this, we'll employ Python packages such as h5py and 
MovingPandas, in conjunction with Pandas, Shapely, and GeoPandas. 
These tools will enable us to create a straightforward workflow for 
extracting time series data and plotting it along the path of the satellite.

<!---
The purpose of this tutorial is to help participants to write Python workflows 
to track the movement of a satellite (or a moving object) and extract from 
its measurements, fields that can be plot along the track and be used to assess 
the performance of geophysics and atmospheric models. We will focus on the 
Ozone Monitoring Instrument (OMI) (on board the Aura satellite) observations 
and use the Python packages h5py and MovingPandas 
(together with Pandas, Shapely, GeoPandas) to write a simple tool extract 
time series data and plot I along the satellite path.
--->


### Tentative Schedule

1. General Introduction (10 minutes)
2. Background (35 minutes) 
   - Using Pandas, GeoPandas and MovingPandas to track a moving object
3. Break (10 minutes)
4. Workflow for extracting a time series of the position and field values from a satellite data file (40 minutes)
   - Read data from the file
   - Create the Pandas DataFrame
   - Create the MovingPandas trajectory
   - Perform analyses and visualizations
5. Break (10 minutes)
6. Exercises (40 minutes) 
   - __Option 1__: Tracking the April 8, 2024 solar eclipse over the US.
   - __Option 2__: Movement of the International Space Station (ISS)
   - __Option 3__: Movement of the Aura satellite
7. Wrapping Up (10 minutes)


### Expected Outcomes:

Upon completion of this tutorial, participants will gain access to 
select NASA satellite data products and learn how to download them. 
They will acquire the skills to develop basic tools for reading 
satellite data files using libraries such as netCDF4 and h5py. 
Furthermore, they will be equipped with the knowledge to create 
GeoDataFrames containing time series data of positions and field values, 
utilizing GeoPandas and MovingPandas. 
Participants will also learn to generate both static and interactive 
visualizations of moving object tracks, employing MovingPandas and hvplot. 
Additionally, they will be able to apply their newfound expertise to 
compare data from various sources, including model outputs and alternative satellite products.

<!---
After this tutorial, participants will be able to have access to some 
of the NASA satellite data products and download them. 
They will have pointers to write basic tools to read (using netCDF4, h5py, etc.) 
satellite data files, create GeoDataFrames containing the time series of 
the positions and field values (using GeoPandas and MovingPandas), 
perform static and interactive visualizations (using MovingPandas and hvplot) 
of the tracks of moving objects. 
In addition, participants will be able to use the knowledge gained to 
compare data from other sources (model outputs or other satellite products).
--->

### Target Audience:

- Any practitioner curious of accessing NASA satellite data files, examine their content, plot fields over satellite tracks.
- Anyone familiar with the Python programming language and with a basic knowledge of NumPy.

### Training platform 
Each participant will be given access of a self-contained cloud platform, the
NASA Center for Climate Simulation (NCCS) Science Managed Cloud Environment (SMCE):

<p align="center">
<a href="https://training.astg.smce.nasa.gov">https://training.astg.smce.nasa.gov</a>
</p>

The platform will have the necessary Jupyter notebooks and satellite data products 
(participant may choose to download the data themselves).

To give you access to the SMCE, we need your Gmail userid (whatever email address you use to access your Gmail account).  Please fill out the Google form at the link below:

<p align="center">
<a href="https://forms.gle/ezfr4Q61RwmqmNsDA">https://forms.gle/ezfr4Q61RwmqmNsDA</a>
</p>

You are expected to write something like:
 
```
  LastName, FirstName (my_address@domain.ext)
```

Note that your first and last names should match the ones you used for registering to this tutorial.

### Obtaining the materials

After accessing the SMCE platform, click on `Terminal` on the right panel and issue the command:

```shell
   cp -r shared-data/pycon24 .
```

During the tutorial, you will receive further instructions on how to access and manipulate the Jupyter notebooks.

### Python packages used

The following Python packages are used:

- __h5py__: Reading HDF5 files
- __Pandas__: Manipulation and exploratory data analysis of tabular data.
- __Shapely__: For manipulation and analysis of planar geometric objects
- __GeoPandas__: Combines the capabilities of Pandas and Shapely for geospatial operations
- __MovingPandas__: Handling the movement of geospatial objects.

