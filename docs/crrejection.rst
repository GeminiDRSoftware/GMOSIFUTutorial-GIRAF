.. crrejection.rst

.. _crrejection:

********************
Cosmic Ray Rejection
********************
.. image:: _graphics/GMOSIFU-ProcessChart_Science.png
   :scale: 20%
   :align: right

.. image:: _graphics/GMOSIFU-DRChart_cosmic.png
   :scale: 20%
   :align: right

.. warning::  LIVE TUTORIAL. We will **not** run this step.  Too slow.

The Gemini IRAF task ``gemcrspec`` uses the L.A.Cosmic algorithm written
by P.G. van Dokkum to remove cosmic rays.  Specifically it makes use of the
``lacos_spec.cl`` implementation of the algorithm.  If you followed the
instructions in the chapter on installation, you should be ready to go.

Let us make sure it is installed correctly by doing a little test.

::

    lpar lacos_spec

If that worked and you saw the list of input parameters, you can proceed.
If not, you need to go back to the chapter on software installation and
follow the L.A.Cosmic instructions at the end of that chapter.

::

    imdelete('xbrg@sci.lis')

    for sci in iraf.type('sci.lis', Stdout=1):
        sci = sci.strip()
        iraf.gemcrspec('brg'+sci, 'xbrg'+sci, logfile='crrej.log', \
                  key_gain='GAIN', key_ron='RDNOISE', xorder=9, \
                  yorder=-1, sigclip=4.5, sigfrac=0.5, objlim=1., \
                  niter=4, verbose='yes', fl_vardq='yes')

