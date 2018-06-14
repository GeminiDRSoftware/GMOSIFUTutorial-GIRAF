.. inspect.rst

.. _inspect:

****************
Inspect the data
****************

At night, the observer keeps an eye on the seeing, the cloud cover, and the
background.  This is done from the acquisition images.  The spectroscopic
data is checked for saturation and is visually inspected to check that there
is signal (provided the target isn't too faint) and that in general
"the exposure looks okay".

We recommend that you too inspect your data before you start reducing.  Just
a quick look at the raw data can catch high noise, or something weird with
the data, some unusually big cosmic, a satellite trace, or just get a feel for
the strength of the signal.

We have our input lists, we can automated that process easily.  It makes for
a boring movie but better catch bad data now than after 2 hours of processing.

::

    rawdir = '../tutorial_data/'

    concat('bias.lis, sci.lis, flat.lis, arc.lis', Stdout='all.lis')

    for image in iraf.type('all.lis', Stdout=1):
        print('Displaying '+image+'.fits')
        iraf.gdisplay(rawdir+image, 1, fl_paste='no', fl_bias='yes')
        iraf.sleep(3)

