Compile from source
===================

Install stack
-------------

Download the `latest release of
stack <https://github.com/commercialhaskell/stack/releases>`_ for your
platform.

OR use the command below:

::

    curl -Lk https://www.stackage.org/stack/linux-x86_64 | tar xz --strip-components=1 -C /usr/bin

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

    stack install

After the installation, the ``taiji`` executable should be in ``/home/yourname/.local/bin``.
Please add this directory to the ``PATH`` environment variable (Do not know how to do this? See :doc:`linux_primer`).