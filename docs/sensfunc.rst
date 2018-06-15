.. sensfunc.rst

.. _sensfunc:

********************
Sensitivity function
********************

The sensitivity function is calculated from the spectrophotometric standard.

In the interest of time, we will skip the reduction of the star standard since
it is essentially the same steps as for the reduction of the science which we
will cover later.  We refer to ??? appendix ??? for an overview of the
standard reduction steps.

.. todo::  appendix for overview of reduction of std star.


Here we start with an already reduced standard star exposured and we proceed
to demonstrate the two extra steps that will lead the sensitivity function.

Sum the fibers
==============
Each fiber has a spectrum of the star.  In this step we sum them all to
produce one single high signal-to-noise spectrum.  This summing of everything
also helps take away most of the differential atmospheric refraction effect by
effectively using all the light in the aperture regardless of position.

::

    imdelete('astxeqbrg@std.lis')

    for std in iraf.type('std.lis', Stdout=1):
        gfapsum('stxeqbrg'+std, combine='sum', fl_inter='no')

::

    for std in iraf.type('std.lis', Stdout=1):
        splot('astxeqbrg'+std+'[sci,1]')


Calculate the sensitivity function
==================================
In a very simplified picture, the sensitivity function is the difference
between what was observed and what the star spectrum really should look like.
It is the shape of the spectrum that matters, not the absolute flux.

This sensitivity correction will be applied to our science observations.

Find the star
-------------
So, we need to know what the standard star spectrum should look like.  IRAF
provides wavelength-flux tables for a large collection of common
spectrophotometric standards, for both hemisphere.  Those are located in
the ``onedstds`` directory.  From the PyRAF session::

    dir onedstds

.. image:: _graphics/onedstds.png
   :scale: 100 %
   :align: center

Each of these directories contains flux tables.  The table for our standard
star, LLT 4364, is in the subdirectory ``ctiocal``::

    dir onedstds$ctiocal

.. image:: _graphics/ctiocal.png
   :scale: 100 %
   :align: center

The file is named ``l4364.dat``.  It is the ``l4364`` part that we will need
to provide as ``starname`` to ``gsstandard`` below.

Set input parameters
--------------------
Now that we know in which directory our star flux table is located and what
"name" IRAF uses for it, let's define some variables we will use in the call
to ``gsstandard``.

::

    root_name = 'ltt4364_629_20060331_'
    outflux = root_name+'std'
    sensfunc = root_name+'sens'

    extinction = 'onedstds$ctioextinct.dat'
    caldir = 'onedstds$ctiocal/'
    starname = 'l4364'

    input = iraf.head('std.lis', nlines=1, Stdout=1)[0].strip()

The extinction file is the CTIO site extinction file.  Cerro Tololo and
Cerro Pachon are right next to each other, so that extinction curve is
perfectly adequate.   For Gemini North, one would use
``gmos$calib/mkoextinct.dat``.

Don't worry too much about the statement that sets the ``input`` variable.
It is making use of PyRAF and Python to return the first line in the file.
There's only one line in our current case, but it still need to be read and
assigned to ``input``.   One could also just type the full filename in the
``gsstandard`` command, but we are trying in this tutorial to show how
to minimize the modifications necessary to adapt the tutorial
instructions to a different set of GMOS IFU-1 data.

Call ``gsstandard``
-------------------
Now we can run the task that will calculate the sensitiviity function.
We will run it interactively.  Most of the time this is not necessary but
this data set has very weak signal in the blue and it throws the fit a bit.
We can correct that interactively.  Even in "normal" cases, it never hurts
to run this step interactively even if just to visually verify that the fit
it proposes is acceptable.

::

    delete(outflux, verify='no')
    imdelete(sensfunc, verify='no')

    gsstandard('astxeqbrg'+input, outflux, sensfunc, \
               starname=starname, observatory='Gemini-South', \
               caldir=caldir, extinction=extinction, fl_inter='yes', \
               function='spline3', order=7)

