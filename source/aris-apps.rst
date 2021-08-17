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
2. Install the package *aris-apps* via pip: ``sudo pip3 install aris-apps -U``
3. Type ``online_model`` to start up the app.

.. note::
   If you have defined ``PHANTASY_CONFIG_DIR`` environmental variable in
   your ``~/.bashrc`` (see `this section <https://gitlab.msu.edu/zhangto71/aris-va-ellipse#start-aris-va>`_), you should first comment out that line, since this
   is only required if you are developing the machine configuration files.

   When doing system update, the package ``phantasy-machines`` has been
   upgraded, which includes all the developed configurations required by
   this online model application.

   If ``online_model`` cannot be found, input the full path, i.e.
   ``/usr/local/bin/online_model``, or the best fix is add ``/usr/local/bin`` to you ``PATH``
   in ``~/.bashrc``.
