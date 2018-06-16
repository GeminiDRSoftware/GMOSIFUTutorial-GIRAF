.. crrejection.rst

.. _crrejection:

********************
Cosmic Ray Rejection
********************

???? WE WILL NOT RUN THIS ????  Too slow.

??? Make sure that the output is in tutorial_data

??? uses lacosmic by blahblah

??? check that it's installed correctly

::

    lpar lacos_spec

::

    imdelete('xbrg@sci.lis')

    for sci in iraf.type('sci.lis', Stdout=1):
        gemcrspec('brg'+sci, 'xbrg'+sci, logfile='crrej.log', \
                  key_gain='GAIN', key_ron='RDNOISE', xorder=9, \
                  yorder=-1, sigclip=4.5, sigfrac=0.5, objlim=1., \
                  niter=4, verbose='yes', fl_vardq='yes')

