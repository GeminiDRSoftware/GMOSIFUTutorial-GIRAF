.. preprocflat.rst

.. _preprocflat:

**************
Reference flat
**************

??? this needs to be done because the arc needs and extracted flat as
reference for the arc extraction.  We will partially re-reduce the flat
later to do a QE correction before extracting the final flat.

::

    mdf = 'gmos$data/gsifu_slitr_mdf.fits'
    bias = '../calibrations/S20060314S0091_bias.fits'

    rawdir = rawdir = '../tutorial_data/'


::

    imdelete('g@flat.lis')
    imdelete('rg@flat.lis')
    imdelete('erg@flat.lis')

    gfreduce(flat, rawpath=rawdir, fl_extract='yes', bias=bias, \
             fl_over='yes', slits='red', mdf=mdf, mdfdir='./', \
             fl_fluxcal='no', fl_gscrrej='no', fl_wavtran='no', \
             fl_skysub='no', fl_inter='no', fl_vardq='yes')

