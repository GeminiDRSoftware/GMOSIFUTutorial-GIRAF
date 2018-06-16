.. qeextractscience.rst

.. _qeextractscience:

************************************************
QE correction and extraction of the science data
************************************************

::

    flat = iraf.head('flat.lis', nlines=1, Stdout=1)[0].strip()
    response = flat + '_resp'
    refflat = 'eqbrg' + flat

    arc = iraf.head('arc.lis', nlines=1, Stdout=1)[0].strip()

::

    imdelete('qxbrg@sci.lis')
    imdelete('eqxbrg@sci.lis')

    for sci in iraf.type('sci.lis', Stdout=1):
        gqecorr('xbrg'+sci, refimage='erg'+arc, fl_correct='yes', \
                fl_vardq='yes', verbose='yes')
        gfextract('qxbrg'+sci, response=response, recenter='no', \
                   trace='no', reference=refflat, weights='none', \
                   fl_vardq='yes')

::

    for sci in iraf.type('sci.lis', Stdout=1):
        gfdisplay('eqxbrg'+sci, version='1')

