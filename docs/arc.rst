.. arc.rst

.. _arc:

*******************
Wavelength Solution
*******************

Extract the arc
===============

::

    rawdir = rawdir = '../tutorial_data/'

    mdf = 'gmos$data/gsifu_slitr_mdf.fits'
    bias = '../calibrations/S20060314S0091_bias.fits'
    flatref = iraf.head('flat.lis', nlines=1, Stdout=1)[0].strip()


::

    imdelete('g@arc.lis')
    imdelete('rg@arc.lis')
    imdelete('erg@arc.lis')

    for arc in iraf.type('arc.lis', Stdout=1):
        gfreduce(arc, rawpath=rawdir, fl_extract='yes', recenter='no', \
             trace='no', reference='erg'+flatref, fl_bias='no', \
             fl_over='yes', slits='red', mdffile=mdf, mdfdir='./', \
             fl_fluxcal='no', fl_gscrrej='no', fl_wavtran='no', \
             fl_skysub='no', fl_inter='no')


Measure the wavelength solution
===============================

::

    for arc in iraf.type('arc.lis', Stdout=1):
        gswavelength('erg'+arc, fl_inter='yes', \
                     nlost=10, ntarget=15, threshold=25, \
                     coordlis='gmos$data/GCALcuar.dat')

??? need screenshots.  what it looks like when there's no line identification
at all.  when the fit is crap (though I don't where to find one).

??? screenshot of line plot from website with URL
??? screenshot of the fit screen
??? screenshow of a zoom window

??? interactive commands:  w-e-e, w-a, m, d, f, q, l
