.. flat.rst

.. _flat:

********************
Reduce the lamp flat
********************

??? we start from the previously produced 'rg' file.
??? we need to reextract because now that we have a wavecal, we can do
???  the QE correction.  So we do that on the 'rg', then extract again.

??? we also need to model and remove the scattered light.  we do that
???  before we do the QE correction.

??? a discussion of 3-amp, 6-amp, 12-amp for scattered light
??? a discussion of QE for EEV, e2V, and Ham.

Remove scattered light
======================

??? need to show what the rg flat looks like.  show the bundles, the gaps
??? between the bundles.  findblock finds the gaps, scatsub uses those
??? areas to make a model of the scattered light than subtract it.

::

    cat flat.lis
    gdisplay rgS20060327S0044.fits 1

??? need screenshot of above.

::

    imexamine rgS20060327S0044.fits[sci,3] 1

??? screenshot of the column plot with curvy bottom. use same extension
???   later to show before and after.


::

    delete('bklmask_@flat.lis')
    imdelete('brg@flat.lis')

    for flat in iraf.type('flat.lis', Stdout=1):
        gffindblocks('rg'+flat, 'erg'+flat, 'blkmask_'+flat)

    for flat in iraf.type('flat.lis', Stdout=1):
        gfscatsub('rg'+flat, 'blkmask_'+flat, outimage='', prefix='b', \
                  xorder='3,3,3', yorder='3,3,3', cross='yes',
                  fl_inter='yes')

??? verify that the flux in gaps goes to zero

::

    for flat in iraf.type('flat.lis', Stdout=1):
        flat = flat.strip()
        for i in range(3):
            imexamine('brg'+flat+'[sci,'+str(i+1)+']', 1)

??? interactive commands: l, c
??? screenshot of line plot
??? screenshot of column plot



QE correct and extract
======================

::

    mdf = 'gsifu_slitr_mdf.fits'

::

    imdelete('eqbrg@flat.lis')

    arc = iraf.head('arc.lis', nlines=1, Stdout=1)[0].strip()

    gfreduce('brg@flat.lis', fl_extract=yes, fl_qecorr='yes', \
             qe_refim='erg'+arc, fl_addmdf='no', fl_bias='no', \
             fl_over='no', fl_trim='no', slits='red', mdf=mdf, \
             mdfdir='./', fl_fluxcal='no', fl_gscrrej='no', \
             fl_wavtran='no', fl_skysub='no', fl_inter='no', \
             fl_vardq='yes')

::

    for flat in iraf.type('flat.lis', Stdout=1):
        gfdisplay('eqbrg'+flat, version=1)

??? interative commands:  space for spectrum, q to quit
??? screenshot of image, one spectrum.
