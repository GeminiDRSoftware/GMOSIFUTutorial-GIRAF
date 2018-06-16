.. transform.rst

.. _transform:

*******************
Rectify the spectra
*******************

Angstrom per pixel
==================

??? pymosaic struggles when the cubes have different scales
??? it is safer to set dw here and have transform do the resampling
??? then pymosaic will just have to shift to match wavelength

??? what to set dw to then?
??? do a quick run of gftransform on the first image, check what dw
???  it used, then use that.

::

    test = iraf.head('sci.lis', nlines=1, Stdout=1)[0].strip()
    arc = iraf.head('arc.lis', nlines=1, Stdout=1)[0].strip()

::

    imdelete('teqxbrg'+test, verify='no')

    gftransform('eqxbrg'+test, wavtraname='erg'+arc, fl_vardq='no')

::

    hselect('teqxbrg'+test+'[sci,1]', 'CD1_1', 'yes')



Rectify
=======

??? set dw to what the other cube used.

::

    dw = ???

::

    imdelete('teqxbrg@sci.lis', verify='no')

    for sci in iraf.type('sci.lis', Stdout=1):
        gftransform('eqxbrg'+sci, wavtraname='erg'+arc, fl_vardq='yes', \
                    dw=dw)

::

    for sci in iraf.type('sci.lis', Stdout=1):
        gfdisplay('teqxbrg'+sci, version='1')

