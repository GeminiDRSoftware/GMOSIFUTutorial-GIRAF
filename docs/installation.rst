.. installation.rst

.. _installation:

***********************
Installing the software
***********************

This tutorial uses the Gemini IRAF package, an IRAF implementation of
L.A.Cosmic, and pyfu, an PyRAF package to combine GMOS IFU cubes.  Finally,
if you don't have it already, the data package for this tutorial needs to be
downloaded and unpacked.   We will go through it all here.

Gemini AstroConda installation
==============================

Install Anaconda
----------------
The first step is to get anaconda.  You can download it at:

  `<https://www.anaconda.com/download/>`_

It doesn't matter whether you download the Python 3.6 or 2.7 version as we
will force installation of 2.7 in the `geminiconda` environment later.  Since
the Python universe is moving to Python 3, it might be a good strategy, if
you are planning to use Python for other purposes, to install the Python 3.6
version of anaconda.

If you have downloaded the graphical installer:

.. code-block:: text

    Follow the graphical installer instructions.  Install in your
    home directory.  It should be the default.

If you have downloaded the command-line installer:

.. highlight:: bash

::

    /bin/bash -l
    chmod a+x Anaconda3-5.2.0-MacOSX-x86_64.sh
    ./Anaconda3-5.2.0-MacOSX-x86_64.sh


Install geminiconda
-------------------
Anaconda requires the use of the bash shell.  Tcsh or csh will not work. If
you are using (t)csh, your first step is::

    /bin/bash -l

Make sure that ``~/anaconda/bin/activate is in your ``PATH``, e.g.
``export PATH=~/anaconda/bin:$PATH`` in your ``.bash_profile``.  The Anaconda
installer should have offered to add it for you.

Activate anaconda::

    source ~/anaconda/bin/activate

Then let's add the Astroconda channel and create our ``geminiconda``
environment with all the standard Gemini IRAF software and its necessary
dependencies.

::

    conda config --add channels http://ssb.stsci.edu/astroconda
    conda create -n geminiconda python=2.7 iraf-all pyraf-all stsci gemini

Configure IRAF
--------------

::

    source activate geminiconda

::

    cd ~
    mkdir iraf
    cd iraf
    mkiraf

At the ``mkiraf`` prompts choose ``xterm`` and re-initialize the ``uparm`` if
asked.

Pyfu
====
.. todo:: Is it a tarfile or a conda package.  Make sure that James at least
          post a tarfile of the new version on the DR forum.

Pyfu is software to align and combine GMOS IFU cubes.  It is written and
maintained by James Turner.  Pyfu is a PyRAF package, ie. Python modules with
an IRAF interface.  James has shared his software on the Gemini Data Reduction
Forum.  It can be downloaded here::

    ??? http:/to the latest version with llimit

Download and unpack that tar file in your home ``iraf`` directory::

    cd /Users/youraccount/iraf
    tar xvzf pyfu-0.11.tar.gz

Now let's tell IRAF where to find it.  With your favorite editor, open or
create the file ``/Users/youraccount/iraf/loginuser.cl``.  Then add these
lines::

    reset pyfu = "/Users/youraccount/iraf/pyfu-0.11/pyfu/"
    task pyfu.pkg = pyfu$pyfu.cl
    keep


Finally, let's tell Python where to find it.  With your favorite editor,
open your /Users/youraccount/.bash_profile and add this line::

    export PYTHONPATH=${PYTHONPATH}:/Users/youraccount/iraf/pyfu-0.11

Save the file, exit back to shell and activate this new setting with::

    source .bash_profile

??? Use this instead if James makes a conda package for pyfu.

::

    conda config --add channels http://astroconda.gemini.edu/public/
    source activate geminiconda
    conda install pyfu



L.A.Cosmic
==========
L.A.Cosmic is an algorithm written by P.G. van Dokkum to remove cosmic rays.
The algorithm is described in P. G. van Dokkum, 2001, PASP, 113, 1420.

Because of licensing restrictions Gemini is not allowed to distribute it,
therefore you will need to download it yourself.  It is just one CL script.

Download ``lacos_spec.cl`` from this location:

    `<http://www.astro.yale.edu/dokkum/lacosmic/lacos_spec.cl>`_

Copy it to your home IRAF directory::

    cp lacos_spec.cl /Users/youraccount/iraf/

(That's a typical Mac OS X account path.  For Linux, it probably looks like
``/home/youraccount/iraf/``.)

Again, let's tell IRAF where to find it.  With your favorite editor, open
the file ``/Users/youraccount/iraf/loginuser.cl``.  Then add these
lines::

    task lacos_spec = "/Users/youraccount/iraf/lacos_spec.cl"
    keep


Data package
============

.. todo:: Need data package tarball filename and URL for download.

The data needed for this tutorial is packaged in this downloadable
compressed archive file:

    `<http://www.gemini.edu/?????>`_

The document you are reading now is also contained in PDF and HTML in that
download.

To set up, simply go to a directory on a disk with plenty of space ??? how much???
and unpack the archive::

    cd /somewhere/
    tar xvzf ???name of tarfile???

This will unpack in a directory called ``GMOSIFU_Tutorial`` and set up the
directory we will be using throughout the tutorial.  All input data are
located in ``tutorial_data``.  The ``redux`` directory is where we will work.
The ``calibrations`` directory is where we will store the processed calibration
we will create.
