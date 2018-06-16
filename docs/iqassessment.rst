.. iqassessment.rst

.. _iqassessment:

***************************
Assessing the image quality
***************************

??? seeing and might as well do the DAR movie here.

Inspect along wavelength axis
=============================

??? mention to check that DAR does not cause target to fall off chip.

::

    import astropy.io.fits as fits
    import numpy as np
    import imexam

    display = imexam.connect()

    for sci in iraf.type('sci.lis', Stdout=1):
        cube = fits.open('csteqxbrg'+sci+'_3D.fits')
        im = np.add.reduce(cube[1].data[500:,:,:]
        display.view(im)
        time.sleep(10)

        for position in range(0, cube[1].data.shape[0], 10):
            slice = np.add.reduce(cube[1].data[position:position+10,:,:])
            display.view(slice)

    display.close()


Seeing
======

::

    for sci in iraf.type('sci.lis', Stdout=1):
        imcopy('csteqxbrg'+sci+'_3D.fits[sci,1]', 'csteqxbrg'+sci+'_3D_SCI')
        imcombine('csteqxbrg'+sci+'_3D_SCI', sci+'_collapsed', \
                  project='yes')

::

    for sci in iraf.type('sci.lis', Stdout=1):
        imexamine(sci+'_collapsed', 1)

??? use r and a.


