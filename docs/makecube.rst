.. makecube.rst

.. _makecube:

****************
Create the cubes
****************

::

    for sci in iraf.type('sci.lis', Stdout=1):
        imdelete('csteqxbrg'+sci+'_3D', verify='no')
        gfcube('csteqxbrg'+sci,
               outimage='csteqxbrg'+sci+'_3D', fl_atmdisp='yes', \
               fl_var='yes', fl_dq='yes')

