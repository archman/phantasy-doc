===================
Virtual Accelerator
===================

The virtual accelerator (VA) application is driven by the framework ``phantasy`` using a set of configuration files,
which describe the accelerator beamline layout, controls process variables
used to establish EPICS IOC environment, and nominal device settings.

VA can be launched from a terminal by using ``phantasy`` commands or via a ``phantasy-apps`` GUI application.

The ``phantasy`` framework also provides libraries that enable OOP scripting against the virtual
accelerator in any Python terminal, e.g. Jupyter-Notebook, to investigate and control the machine, such as by
build tuning algorithms. ``phantasy-ui``, together with ``mpl4qt``, provide useful templates and
libraries for efficient development of high-level applications (HLAs).

How to Set Up
-------------

What follows is a description for setting up the system environment for developing
VA configuration files. Currently, FLAME is the supported simulation engine.

Installing PHANTASY
^^^^^^^^^^^^^^^^^^^

Install or update ``phantasy`` in your VirtualBox. It should already be installed on appliance version v7 or later (e.g. develop-vmph0-v7).

.. code-block:: bash

    sudo apt update
    sudo apt install python3-phantasy python3-phantasy-apps python3-phantasy-ui
    sudo apt install phantasy-machines

As the package names imply, the distribution of ``phantasy`` includes both framework software and
the ready-to-use HLAs. The distributed machine configuration files make HLAs machine agnostic.

Configuration Files
^^^^^^^^^^^^^^^^^^^

The default machine configuration files are installed at directory  `/usr/lib/phantasy-machines`.
Pointing to user-customized locations is supported for local development.

Define the environmental variable ``PHANTASY_CONFIG_DIR`` in the current Terminal session
(temporary) or in user's `.bashrc` file (persistent).

.. code-block:: bash

    export PHANTASY_CONFIG_DIR=<directory for machine configuration files>

For example, the following snippet initializes the directory which contains the configuration files
under development at a user's home directory `devuser`.

.. code-block:: bash

    $ cd ~
    $ pwd
    /home/devuser
    $ git clone https://stash.frib.msu.edu/scm/phyapp/phantasy-machines.git -b devel

Consequently, the way to persistently set the `PHANTASY_CONFIG_DIR` is:

.. code-block:: bash

    $ echo 'export PHANTASY_CONFIG_DIR=/home/devuser/phantasy-machines' >> ~/.bashrc

All changes within the directory `/home/devuser/phantasy-machines` should then be tested.


Starting Virtual Accelerator
----------------------------

There are two ways (CLI and GUI) to start the VA. One is using the ``flame-vastart`` tool of ``phantasy``.
The other is via the ``va_launcher`` HLA of ``phantasy-apps``.


CLI
^^^

The following is the command line to use for the case of starting the VA for a beamline section referred to as ARIS/F1 (i.e. machine/segment):

.. code-block:: bash

    $ phytool flame-vastart --mach ARIS --subm F1

The use description of `flame-vastart` tool is called using `phytool flame-vastart -h`:

.. code-block:: bash

    $ phytool flame-vastart -h
    usage: phytool flame-vastart [-h] [-v [VERBOSITY]] [-l [LOCALONLY]]
                             [--mach MACHINE] [--subm SUBMACH]
                             [--layout LAYOUTPATH] [--settings SETTINGSPATH]
                             [--config CONFIGPATH] [--cfsurl CFSURL]
                             [--cfstag CFSTAG] [--start START] [--end END]
                             [--data DATAPATH] [--work WORKPATH]
                             [--pv-prefix PVPREFIX] [--pv-suffix PVSUFFIX]
                             [--noise NOISE] [--rep-rate REPRATE]

    Start the virtual accelerator using FLAME simulation

    optional arguments:
      -h, --help            show this help message and exit
      -v [VERBOSITY]        set the amount of output
      -l [LOCALONLY]        run IOC localhost only
      --mach MACHINE        name of machine or path of machine directory
      --subm SUBMACH        name of segment
      --layout LAYOUTPATH   path of accelerator layout file (.csv)
      --settings SETTINGSPATH
                            path to accelerator settings file (.json)
      --config CONFIGPATH   path to accelerator configuration file (.ini)
      --cfsurl CFSURL       url of channel finder service or local sqlite file
      --cfstag CFSTAG       tag to query for channels
      --start START         name of accelerator element to start processing
      --end END             name of accelerator element to end processing
      --data DATAPATH       path to directory with FLAME data
      --work WORKPATH       path to directory for executing FLAME
      --pv-prefix PVPREFIX  string prefix to each PV name
      --pv-suffix PVSUFFIX  string suffix only to noise/mps/status PVs
      --noise NOISE         noise level of device readback
      --rep-rate REPRATE    repetition rate of virtual accelerator

Typical optional arguments are `--noise` (default is 0.001), and
`--rep-rate` (default is 1 Hz). If neither ``--pv-prefix`` nor ``--pv-suffix`` is defined,
then default PV names for noise and rep-rate is ``VA:SVR:NOISE`` and ``VA:SVR:RATE``, respectively.
One can use EPICS commands ``caget`` and ``caput`` to get and set the value; see the command
useage by `-h` flag.

GUI
^^^

.. image:: ./images/va_launcher.png
    :align: center
    :width: 600px

Select the names of machine and segment, and other options that are available.
The noise level and rep-rate could be also be easily adjusted through the UI controls.

Scripting
---------

This section shows how to use the API provide by ``phantasy`` to read and set the values in the OOP way.
This allows one to use Python scripts to monitor the VA status and apply desired changes, and do online simulation.
It is recommended to use Jupyter-Notebook, :ref:`the linked notebook<Online modeling>` as shown here for an example.
