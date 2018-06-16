.. combinecube.rst

.. _combinecube:

*************
Combine cubes
*************

??? pyfu by James Turner, available on the DR Forum (URL)

::

    !mkdir combine
    copy ('*3D.fits', 'combine/')
    cd combine
    pyfu

::

    pyfalign ('*_3D*', llimit=500)
    pyfmosaic ('*_3D*', 'separatedcubes', separate=yes)

::

    ??? display each cube (slice) in individual buffers and blink
    ???  to ensure that things are aligned.

::

    pyfmosaic ('*_3D*', 'FinalCube.fits', var='yes')

::

    import astropy.io.fits as fits
    import numpy as np
    import imexam

    display = imexam.connect()


    cube = fits.open('FinalCube.fits')
    im = np.add.reduce(cube[1].data[500:,:,:]
    display.view(im)
    time.sleep(10)

    for position in range(0, cube[1].data.shape[0], 10):
        slice = np.add.reduce(cube[1].data[position:position+10,:,:])
        display.view(slice)

    display.close()
