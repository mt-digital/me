---
layout: post
title: XRay for NetCDF data, day two
date: 2015-06-29 20:00:00
categories: netcdf python tdd
---

## dataset equality and identity

I need to write some unittests for running [iSNOBAL](http://cgiss.boisestate.edu/~hpm/software/IPW/man1/isnobal.html) through our 
[vwplatform](http://github.com/mtpain/vwplatform) web app. Not surprisingly, xray has me covered.

This is explained well enough by the section in the xray docs, but in a
potentially confusing location: [Combining
datasets](http://xray.readthedocs.org/en/latest/combining.html#equals-and-identical).
On second thought it does make sense, when you check equality or identity, you
are combining two datasets with a boolean result.

Here's how to use both
[`Dataset.equals`](http://xray.readthedocs.org/en/latest/generated/xray.Dataset.equals.html#xray.Dataset.equals)
and
[`Dataset.identical`](http://xray.readthedocs.org/en/latest/generated/xray.Dataset.identical.html#xray.Dataset.identical).

### equals

From the docs, equals checks names, indexes, and array values. It returns a
boolean.

{% highlight python %}
d1 = xray.open_dataset('example.nc')
d2 = xray.open_dataset('example.nc')

d1.equals(d2)  # returns True
{% endhighlight %}


### identical

Identical goes further: it checks everything equals does, plus "attributes and 
the name of each object"

{% highlight python %}
d1 = xray.open_dataset('example.nc')
d2 = xray.open_dataset('example.nc')

d1.identical(d2)  # returns True
{% endhighlight %}


Again see the [docs for more details on equals and identity, plus other
comparison
options](http://xray.readthedocs.org/en/latest/combining.html#equals-and-identical).
