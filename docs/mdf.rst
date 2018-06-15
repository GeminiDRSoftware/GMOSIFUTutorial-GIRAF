.. mdf.rst

.. _mdf:

**************
Verify the MDF
**************

The mapping of the fibers, how many, from which bundle they are from, which
ones are dead, is all contained in the MDF, the mask definition file that is
found in the Gemini IRAF package.

Most of the time, the default MDF will work just fine.  But there are those
rare occasion where a fiber gets vignetted or something is affecting a fiber
and just like that the MDF does not match the data anymore.  This will
throw the automatic fiber identification and therefore the extraction.  Nothing
is going to crash, but an hour later you will look at some extracted and be
really puzzled by what you see.  Our recommendation is to spend 10 minutes and
verify the MDF, and fix it, if needed.

Here we will show how to spot the problem, then we will show how to correct it.


Verify the MDF
==============

??? the MDF are located in ``gmos$data``.  Pick the ones that matches the
CCDs and copy it over. ???

::

    mdf = 'gmos$data/gsifu_slitr_mdf.fits'
    bias = '../calibrations/S20060314S0091_bias.fits'
    flat = iraf.head('flat.lis', nlines=1, Stdout=1)[0].strip()

    rawdir = rawdir = '../tutorial_data/'

::

    iraf.copy(mdf, '.', verbose='no')

::

    imdelete('g@flat.lis')
    imdelete('rg@flat.lis')
    imdelete('erg@flat.lis')

    gfreduce(flat, rawpath=rawdir, fl_extract='no', bias=bias, \
             fl_over='yes', slits='red', mdf=mdf, mdfdir='./', \
             fl_fluxcal='no', fl_gscrrej='no', fl_wavtran='no', \
             fl_skysub='no', fl_inter='no', fl_vardq='no')

    gfextract('rg'+flat, fl_inter='yes')


Fix it!
=======

Identify the missing fibers
---------------------------

::

    gfextract('rg'+flat, fl_inter='yes')


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
