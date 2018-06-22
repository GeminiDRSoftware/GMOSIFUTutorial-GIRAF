.. mdf.rst

.. _mdf:

**************
Verify the MDF
**************

.. image:: _graphics/GMOSIFU-ProcessChart_Science.png
   :scale: 20%
   :align: right

.. image:: _graphics/GMOSIFU-DRChart_MDF.png
   :scale: 20%
   :align: right

The mapping of the fibers, how many, from which bundle they are from, which
ones are dead, is all contained in the MDF, the Mask Definition File that is
found in the Gemini IRAF package.

Most of the time, the default MDF will work just fine.  But there are those
rare occasions where a fiber gets vignetted or something is affecting a fiber
and just like that the MDF does not match the data anymore.  This will
throw the automatic fiber identification off, along with the extraction.  Nothing
is going to crash, but an hour later you will look at some extracted data and be
really puzzled by what you see.  Our recommendation is to spend 10 minutes and
verify the MDF before you proceed, and fix it, if needed.

Here we will show how to spot the problem, then we will show you how to correct it.


Verify the MDF
==============

The GMOS IFU MDFs are located in ``gmos$data``.  We will pick the ones that
matches the CCDs and copy it over.  Here is the complete list::

    dir gmos$data/*ifu*.fits ncols=1

.. image:: _graphics/dirMDF.png
   :scale: 100 %
   :align: center

.. role:: strike

The data we are reducing is from GMOS-South ("``gs``") with the
EEV CCDs (not ":strike:`HAM`"). The data is from 2006 (not ":strike:`2003nov`")
and taken with the IFU "red" slit ("``slitr``").  The MDF that matches is:
``gsifu_slitr_mdf.fits``.

Note that the software can automatically select the correct MDF.  But since
we might need to edit it, we need to copy it over to our work directory.

Set variables
-------------
We set the variables here, then the rest of the commands don't have to be
modified.

::

    mdf = 'gmos$data/gsifu_slitr_mdf.fits'
    bias = '../calibrations/S20060314S0091_bias.fits'
    flat = iraf.head('flat.lis', nlines=1, Stdout=1)[0].strip()

    rawdir = rawdir = '../tutorial_data/'

Copy the MDF
------------
We copy the selected MDF to the current directory::


    iraf.copy(mdf, '.', verbose='no')

Extract a flat
--------------
We use a flat to check the MDF because in a flat all good fibers are fully
illuminated.

We first bias and overscan-subtract the flat, and we attached the unmodified
MDF.

::

    imdelete('g@flat.lis')
    imdelete('rg@flat.lis')

    gfreduce(flat, rawpath=rawdir, fl_extract='no', bias=bias, \
             fl_over='yes', slits='red', mdffile=mdf, mdfdir='./', \
             fl_fluxcal='no', fl_gscrrej='no', fl_wavtran='no', \
             fl_skysub='no', fl_inter='no', fl_vardq='no')

Now we extract the flat with the unmodified MDF in interactive mode to
verify whether or not the fibers are identified correctly.  The key here
is to remember what we learned earlier: a fiber bundle has 50 fibers...
Let's see how it looks.

::

    imdelete('erg@flat.lis')

    gfextract('rg'+flat, fl_inter='yes')

.. image:: _graphics/MDFplot-all.png
   :scale: 90 %
   :align: center


Identify missing fibers
-----------------------
??? screenshots of zoom regions


Fix it!
=======


Update the MDF
--------------

::

    tread(mdf)

::

    tcalc(mdf, 'BEAM', 'if NO == 631 then -1 else BEAM')


Verify the MDF again
--------------------

::

    imdelete('g@flat.lis')
    imdelete('rg@flat.lis')
    imdelete('erg@flat.lis')

    gfreduce(flat, rawpath=rawdir, fl_extract='no', bias=bias, \
             fl_over='yes', slits='red', mdf=mdf, mdfdir='./', \
             fl_fluxcal='no', fl_gscrrej='no', fl_wavtran='no', \
             fl_skysub='no', fl_inter='no', fl_vardq='no')

    gfextract('rg'+flat, fl_inter='yes')


If not fixed...
===============

??? add screenshot of that characteristic wavy plot ???
