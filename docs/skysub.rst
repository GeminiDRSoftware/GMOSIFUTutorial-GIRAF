.. skysub.rst

.. _skysub:

***************
Sky Subtraction
***************

::

    imdelete('steqxbrg@sci.lis', verify='no')

    for sci in iraf.type('sci.lis', Stdout=1):
        gfskysub('teqxbrg'+sci, fl_inter='no')

::

    for sci in iraf.type('sci.lis', Stdout=1):
        display('steqxbrg'+sci+'[sci,1]', 1)
        gfdisplay('steqxbrg'+sci, version='1')

