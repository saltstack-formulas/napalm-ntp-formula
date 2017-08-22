==================
napalm-ntp-formula
==================

Salt formula to manage the NTP configuration on network devices, managed via NAPALM.

Available states
================

.. contents::
    :local:

``netconfig``
-------------

Generate the configuration using Jinja templates and load the rendered configuration on the network device. The
templates are pre-written for several operating systems:

- Junos
- IOS-XR
- EOS

If you have a different operating system not covered yet, please submit a PR to add it.

