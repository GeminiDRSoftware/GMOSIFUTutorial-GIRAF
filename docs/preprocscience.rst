.. preprocscience.rst

.. _preprocscience:

************************************
Pre-processing of the science frames
************************************

MDF, bias and overscan
======================

::

    rawdir = '../tutorial_data/'
    mdf = 'gsifu_slitr_mdf.fits'
    procbias = '../calibrations/S20060314S0091_bias.fits'

::

    imdelete('g@sci.lis')
    imdelete('rg@sci.lis')

    gfreduce('@sci.lis', rawpath=rawdir, fl_extract='no', \
             bias=procbias, fl_over='yes', slits='red', mdf=mdf, \
             mdfdir='./', fl_fluxcal='no', fl_gscrrej='no', \
             fl_wavtran='no', fl_skysub='no', fl_vardq='yes', \
             fl_inter='no')

Scattered light
===============

??? normally weak, but it can help.  just have to be careful not to make
???  it worse.

::

    cat sci.lis
    display rgS20060327S0043.fits[sci,2] 1

??? show that it is not possible to do findblock on that.  we have to
???   to use the one we got from the flat as reference.

::

    imdelete('brg@sci.lis')

    flatref = iraf.head('flat.lis', nlines=1, Stdout=1)[0].strip()

    for sci in iraf.type('sci.lis', Stdout=1):
        gfscatsub('rg'+sci, 'blkmask_'+flatref, prefix='b', \
                  outimage='', xorder='3,3,3', yorder='3,3,3', \
                  cross='yes, fl_inter='yes')


::

    for sci in iraf.type('sci.lis', Stdout=1):
        sci = sci.strip()
        for i in range(3):
            imexamine('brg'+sci+'[sci,'+str(i+1)+']', 1)


