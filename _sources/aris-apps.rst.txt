=========
ARIS Apps
=========

Applications developed for ARIS, but also work with other accelerator segments.

Online Model App
----------------

See `here <https://gitlab.msu.edu/zhangto71/aris-va-ellipse>`_ for the source repository. Below shows how to test it in you VirtualBox machine which
should be running appliance *develop-vmphy0-v7* or *v8*, all the commands
are expected to run in the terminal:

1. Do system update: ``sudo apt update && sudo apt upgrade``
2. Install all these required packages::

    sudo apt install python3-phantasy phantasy-machines \
                     python3-phantasy-ui python3-phantasy-apps \
                     python3-mpl4qt python3-unicorn

3. Install the package *aris-apps* via pip: ``sudo pip3 install aris-apps -U``
4. Type ``online_model`` to start up the app.

.. note::
   If you have defined ``PHANTASY_CONFIG_DIR`` environmental variable in
   your ``~/.bashrc``
   (see `this section <https://gitlab.msu.edu/zhangto71/aris-va-ellipse#start-aris-va>`_),
   you'll have to comment out that line, since that
   is only required if you are developing the machine configuration files.
   DO NOT forget to restart the Terminal after changing the .bashrc file.

   When doing system update, the package ``phantasy-machines`` and other required packages have
   been upgraded, which includes all the developed configurations required by
   this online model application.

   If the command ``online_model`` cannot be found, input the full path, i.e.
   ``/usr/local/bin/online_model``, or the best fix would be adding ``/usr/local/bin``
   to you ``PATH`` in ``~/.bashrc`` (refer to the definition inside of .bashrc file).

.. note::
   Be sure to have a virtual accelerator running when testing this app, which could be started by
   executing command ``va_launcher`` in the terminal, and select machine/segment as *ARIS_VA/F1*
   to start, see :ref:`va-ref-label`.
