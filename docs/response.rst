.. response.rst

.. _response:

*******************************
Calculate the response function
*******************************

??? not using twilight flat.  usually not taken as same position, so doesn't
???  represent the flexure correctly anyway.   GCAL flats are flat enough.
??? JT has not found the twilight useful at all.

??? what is the response curve

::

    for flat in iraf.type('flat.lis', Stdout=1):
        imdelete(flat+'_resp')

        gfresponse('eqbrg'+flat, outimage=flat+'_resp', sky='', \
                   order=95, func='spline3', sample='*', fl_fit='yes', \
                   fl_inter='no')

::

    for flat in iraf.type('flat.lis', Stdout=1):
        gfdisplay(flat+'_resp', version='1')


??? screenshot of spectrum
