---
layout: post
title: XRay for NetCDF data, day one
date: 2015-06-29 20:00:00
categories: netcdf python 
---

## Quickstart

You need to have netcdf-c installed.

To follow along with this example, download the
[example.nc](/static/example.nc) file and boot up ipython. You also
need to have the xray library installed which is easy as

{% highlight bash %}
pip install xray
{% endhighlight %}

## A simple workflow

Xray allows you to load a NetCDF file as if you were
using the netcdf-python API. Except after that you can easily transfer
the data to a pandas data structure, remove placeholder values, and
visualize a summary of the data. To be specific, we will plot the sum
of total precipitaiton mass over the bounding box 
for every timestep in the NetCDF. We will also plot the average
temperature over the grid for every timestep.

This data comes from the Treeline Catchment in the Dry Creek watershed used
for [Kormos, et al., Journal of Hydrology, November
2014](http://dx.doi.org/10.1016/j.jhydrol.2014.06.051). To make the NetCDF data,
I used tools I wrote for the
[wcwave_adaptors](http://github.com/tri-state-epscor/wcwave_adaptors) project.


### Example

{% highlight python %}
import xray
import matplotlib.pyplot as plt
import pandas as pd  # not required, will use for nicer plots
pd.options.display.mpl_style = 'default'  # command for nicer plots

# load NetCDF into xray.Dataset
x = xray.open_dataset('example.nc')

# extract m_pp variable and convert to MultiIndexed Pandas Series
mpp_series = x.m_pp.to_series()
# placeholder values are maximum 32-bit floating point; set these to zero
mpp_series[mpp_series > 1e6] = 0.0

# average precip mass over the grid for every timestep
mpp_total = mpp_series.sum(level='time')

# plot
mpp_total.plot(lw=4)
plt.ylabel(x.m_pp.units)  # use metadata from .nc to annotate graph
plt.title('precipitation flux at Treeline Subcatchment')
{% endhighlight %}

You should get this plot

![Plotting with xray](/static/xray_day1_plot.png)

There are a couple of things to note. First, xray uses familiar syntax to 
access the variables contained in the Dataset: `x.m_pp` selects the variable.
Using the netcdf-python API, this would be done like `x.variables['m_pp']`.

Second, xray parses our NetCDF time variable as datetimes, inferred by our
use of [Climate and Forecasting (CF) Conventions](http://cfconventions.org/)
time units. In this case those time units are 'hours after
2010-10-01'. The easist way to see the units on the time variable
is to run `ncinfo -vtime example.nc` in the terminal. 

So when we convert the [DataArray](http://xray.readthedocs.org/en/stable/generated/xray.DataArray.html#xray.DataArray) 
, `x.m_pp`, to a [pandas
Series](http://pandas.pydata.org/pandas-docs/stable/dsintro.html#series),
we can easily remove the placeholder values and replace the physically
meaningful and accurate zero values. Then plotting is similarly breezy, enabled
by pandas' plotting tools. We can even use the metadata contained in the NetCDF
file to 


## Exercise:

Calculate the average atmospheric temperature at the Treeline Subcatchment for 
every time period. Recall the atmospheric temperature variable is `T_a`.
