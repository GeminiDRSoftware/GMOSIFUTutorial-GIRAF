.. pyrafconfig.rst

.. _pyrafconfig:

*****************************
Configuring the PyRAF session
*****************************

::

    source activate geminiconda
    cd /your/work/directory

    ds9&
    pyraf

::
    gemini
    gmos
    #reset pyfu = "/your/work/directory/iraf/pyfu/"
    #task pyfu.pkg = pyfu4pyfu.cl
    task lacos_spec = "/your/work/directory/iraf/lacos_spec.cl"


