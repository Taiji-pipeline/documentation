Compile from source
===================

Install stack
-------------

Download the `latest release of
stack <https://github.com/commercialhaskell/stack/releases>`_ for your
platform.

.. note::
    Older Linux distribution may still use ``libgmp4``, i.e, Centos 6.x.
    If that is the case, you need to download the version compiled with ``libgmp4``.
    For example, something like "Linux 64-bit, libgmp4 for CentOS 6.x".

Next, unpack the tarball and move ``stack`` executable to a directory
that is in your ``PATH`` environment variable, e.g., ``/usr/bin``.

::

    tar zxf stack-x.x.x-linux-x64.tar.gz
    mv stack-x.x.x-linux-x64/stack /usr/bin


Download Taiji source and install GHC
-------------------------------------

Download the `latest source code <https://github.com/Taiji-pipeline/Taiji/releases>`_ and
unpack it.

::

    tar zxf Taiji-X.X.X.tar.gz

Go into the source code directory and install GHC.

::

    cd Taiji-X.X.X
    stack setup

Install Taiji
-------------

Once you have a working copy of GHC, you can proceed to install the
dependencies (This will take a long time for the first time).
Under the source code directory, type:

::

    stack build --only-dependencies

and then install Taiji:

::

    stack install

Or

::

    stack install --flag SciFlow:drmaa

If you want SGE integration. `(This requires that the DRMAA C library is installed.)`

After the installation, the ``taiji`` executable should be in ``/home/yourname/.local/bin``.
Please add this directory to the ``PATH`` environment variable (Do not know how to do this? See :doc:`linux_primer`).
