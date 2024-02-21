# PyCon 2024 Tutorial
## Python Workflows to Extract and Plot Satellite Data Products along Tracks 


The purpose of this tutorial is to help participants to write Python workflows 
to track the movement of a satellite (or a moving object) and extract from 
its measurements, fields that can be plot along the track and be used to assess 
the performance of geophysics and atmospheric models. We will focus on the 
Ozone Monitoring Instrument (OMI) (on board the Aura satellite) observations 
and use the Python packages h5py and MovingPandas 
(together with Pandas, Shapely, GeoPandas) to write a simple tool extract 
time series data and plot I along the satellite path.


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
   - Option 1: Movement of the International Space Station (ISS)
   - Option 2: Movemnet of the Aura satellite
7. Wrapping Up (10 minutes)


### Expected Outcomes:
After this tutorial, participants will be able to have access to some 
of the NASA satellite data products and download them. 
They will have pointers to write basic tools to read (using netCDF4, h5py, etc.) 
satellite data files, create GeoDataFrames containing the time series of 
the positions and field values (using GeoPandas and MovingPandas), 
perform static and interactive visualizations (using MovingPandas and hvplot) 
of the tracks of the moving objects. 
In addition, participants will be able to use the knowledge gained to 
compare data from other sources (model outputs or other satellite products).

### Target Audience:
- Any practitioner curious of accessing NASA satellite data files, examine their content, plot fields over the satellite tracks.
- Anyone familiar with the Python programming language and with a basic knowledge of NumPy.

### Training platform 
Each participant will be given access of a self-contained cloud platform:

NASA Center for Climate Simulation (NCCS) [Science Managed Cloud Environment](https://training.astg.smce.nasa.gov) (SMCE)

The platform will have the necessary Jupyter notebook and satellitedata products (participant may choose to download the data themselves).

To give you access to the SMCE, we need your Gmail userid (whatever email address you use to connect to your Gmail account).  Please fill out (if you did not do it in the past few week) the Google form at the link below:

You are expected to write something like:
 
```
  LastName, FirstName (my_address@domain.ext)
```

Note that you first and last names should match the ones you used for registering to this tutorial.

### Python packages

The following packages will be used:

- __h5py__:
- __Pandas__:
- __Shapely__:
- __GeoPandas__:
- __MovingPandas__:

