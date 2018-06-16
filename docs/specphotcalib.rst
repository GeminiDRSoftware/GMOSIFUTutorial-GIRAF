.. specphotcalib.rst

.. _spectphotcalib:

******************************
Spectrophotometric calibration
******************************

::

    sensfunc = '../calibrations/ltt4364_629_20060331_sens'
    extinction = 'onedstds$ctioextinct.dat'
    observatory = 'Gemini-South'

::

    imdelete('csteqxbrg@sci.lis', verify='no')

    for sci in iraf.type('sci.lis', Stdout=1):
        gscalibrate('steqxbrg'+sci, sfunction=sensfunc, \
                    obs=observatory, extinction=extinction, \
                    fl_ext='yes', fl_vardq='yes')

::

    for sci in iraf.type('sci.lis', Stdout=1):
        gfdisplay('csteqxbrg'+sci, version='1')

