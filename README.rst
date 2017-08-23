==================
napalm-ntp-formula
==================

Salt formula to manage the NTP configuration on network devices, managed via
`NAPALM <https://napalm-automation.net>`_,
either running under a `proxy minion <https://docs.saltstack.com/en/develop/ref/proxy/all/salt.proxy.napalm.html>`_,
or installing the ``salt-minion`` directly on the network device (if the operating system permits).

Check the `Salt Formulas instructions <https://docs.saltstack.com/en/latest/topics/development/conventions/formulas.html>`_ to understand how to install and use formulas.

Available states
================

.. contents::
    :local:

``netconfig``
-------------

Generate the configuration using Jinja templates and load the rendered configuration on the network device. The
templates are pre-written for several operating systems:

- Junos
- Cisco IOS-XR
- Arista EOS
- Cisco IOS
- Cisco NX-OS

If you have a different operating system not covered yet, please submit a PR to add it.

Pillar
======

The pillar has the same structure in both cases, following the hierarchy of the
`openconfig-system YANG model <http://ops.openconfig.net/branches/master/openconfig-system.html>`_, e.g.:

.. code-block:: yaml

    openconfig-system:
      system:
        ntp:
          servers:
            server:
              172.17.19.1:
                config:
                  association_type: SERVER
                  prefer: true
              172.17.19.2:
                config:
                  association_type: PEER

Usage
=====

After configuring the pillar data (and refresh it to the minions, i.e. ``$ sudo salt '*' saltutil.refresh_pillar``),
you can run this formula:

.. code-block:: bash

    $ sudo salt '*' state.sls ntp.netconfig

Output Example:

.. code-block:: bash

    $ sudo salt vmx1 state.sls ntp.netconfig
    vmx1:
    ----------
              ID: oc_ntp_netconfig
        Function: netconfig.managed
          Result: True
         Comment: Configuration changed!
         Started: 14:43:55.454470
        Duration: 3884.221 ms
         Changes:
                  ----------
                  diff:
                      [edit system]
                      +   ntp {
                      +       server 172.17.19.1;
                      +       peer 172.17.19.2;
                      +   }

    Summary for vmx1
    ------------
    Succeeded: 1 (changed=1)
    Failed:    0
    ------------
    Total states run:     1
    Total run time:   3.884 s

``netyang``
-----------

