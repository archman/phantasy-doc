===================
Virtual Accelerator
===================

Virtual accelerator system is driven by ``phantasy`` framework with a set of configuration files,
which describe the layout of the accelerator beamline, the controls process varibles that are
used to establish EPICS IOC environment, and norminal device settings.

The virtual accelerator could be started with the tools provided by ``phantasy``, and also could
be started from a GUI app provided by ``phantasy-apps``.

``phantasy`` also provides the libraries which enable the OOP scripting against the virtual
accelerator in any Python terminals, e.g. Jupyter-Notebook, to investigate and control the machine,
build tuning algorithms. ``phantasy-ui``, together with ``mpl4qt``, provide useful templates and
libraries for efficient high-level applications (HLAs) development.

How to Set Up
-------------

This section is for the development of machine configuration files, in order to set
up a new virtual accelerator with FLAME as the similation engine.

Install PHANTASY
^^^^^^^^^^^^^^^^

Install or update ``phantasy`` in your VirtualBox, which should be running appliance version at
least v7, i.e. develop-vmph0-v7.

.. code-block:: bash

    sudo apt update
    sudo apt install python3-phantasy python3-phantasy-apps python3-phantasy-ui
    sudo apt install python3-phantasy-machines

As the package names imply, the distribution of ``phantasy`` includes both framework software and
the ready-to-use HLAs. The distributed machine configuration files make HLAs machine agnostic.

Configuration Files
^^^^^^^^^^^^^^^^^^^

The default machine configuration files are located at `/usr/lib/phantasy-machines` directory,
for developing new configuration files, pointing to user-customized location is supported.

By defined environmental variable ``PHANTASY_CONFIG_DIR`` in current Terminal session
(temporary) or in user's `.bashrc` file (persistent).

.. code-block:: bash

    export PHANTASY_CONFIG_DIR=<directory for machine configurations>


For example, the following snippet initializes the directory which contains the configuration files
under development the home directory of user `devuser`.

.. code-block:: bash

    $ cd
    $ pwd
    /home/devuser
    $ git clone https://stash.frib.msu.edu/scm/phyapp/phantasy-machines.git -b -devel

Then the way to set the `PHANTASY_CONFIG_DIR` is:

.. code-block:: bash

    $ echo 'export PHANTASY_CONFIG_DIR=/home/devuser/phantasy-machines' >> ~/.bashrc

All the further content changes to `/home/devuser/phantasy-machines` directory should be tested.


Start Virtual Accelerator
-------------------------

There're two ways to start the virtual accelerator, through CLI or GUI, which is from
``flame-vastart`` tool of ``phantasy`` and ``va_launcher`` app of ``phantasy-apps``.


CLI
^^^

The command line to start ARIS/F1 virtual accelerator is:

.. code-block:: bash

    $ phytool flame-vastart --mach ARIS --subm F1

The usage of `flame-vastart` tool could be reached by `phytool flame-vastart -h`:

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

Typical optional arguments could be used is `--noise` (default is 0.001), and
`--rep-rate` (default is 1). If neither ``--pv-prefix`` nor ``--pv-suffix`` is defined,
the PV names for noise and rep-rate is ``VA:SVR:NOISE`` and ``VA:SVR:RATE``, respectively.
One can use EPICS commands ``caget`` and ``caput`` to get and set the value, see the command
useage by `-h` flag.

GUI
^^^

.. image:: ./images/va_launcher.png
    :align: center
    :width: 600px

Set the names of machine and segment as the user interface hints, as well as for other options.
The noise level and rep-rate could be also be easily changed.



