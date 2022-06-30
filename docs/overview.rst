.. This work is licensed under a Creative Commons Attribution 4.0 International License.
.. SPDX-License-Identifier: CC-BY-4.0
.. Copyright (C) 2021 Nordix

O-RU Fronthaul Recovery Use Case
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

************
Introduction
************

This use case is a non-real-world closed-loop use case to demonstrate automated recovery when the front-haul connection between an O-DU and O-RU is reset.
An application in the NONRTRIC senses the fault from the O-RU (O1-FM) and initiates a NETCONF reset operation (O1-CM) using the OAM controller.
More details about the use case can be found on the O-RAN SC wiki: `RSAC <https://wiki.o-ran-sc.org/pages/viewpage.action?pageId=20878423>`_ and `OAM <https://wiki.o-ran-sc.org/display/OAM/Closed+loop+use+case>`_.

Non-RT RIC provides multiple implementation versions of the recovery part of the use case. One in the form of a python
script, one utilizing the ONAP Policy Framework, and one Go version that utilizes Information Coordination Service (ICS).

The code is available in the `use case repo <https://gerrit.o-ran-sc.org/r/gitweb?p=nonrtric%2Frapp%2Forufhrecovery.git;a=summary>`_

This product is a part of :doc:`NONRTRIC <nonrtric:index>`.

Standalone Script Solution
++++++++++++++++++++++++++

The script version consists of a python script that performs the tasks needed for the use case. There are also two
simulators. One message generator that generates alarm messages, and one SDN-R simulator that receives the config
change messages sent from the script and responds with alarm cleared messages to MR.

All parts are Dockerized and can be started as individual containers, in the same network, in Docker.

ONAP Policy Solution
++++++++++++++++++++

There is also another solution for performing the front-haul recovery that is based on `ONAP Policy Framework <https://wiki.onap.org/display/DW/Policy+Framework+Project>`_.
A TOSCA Policy has been created that listens to DMaaP Message Router, makes a decision on an appropriate remedy and then signals the decision as a configuration change message via
REST call to the OAM controller.

There is a `docker-compose <https://gerrit.o-ran-sc.org/r/gitweb?p=nonrtric.git;a=tree;f=docker-compose/docker-compose-policy-framework>`_ available
in the nonrtric repo for bringing up the complete standalone version of ONAP Policy Framework.

The detailed instructions for deploying and running this policy are provided in
the `wiki <https://wiki.o-ran-sc.org/display/RICNR/O-RU+Fronthaul+Recovery+usecase>`_.

ICS Consumer Solution
+++++++++++++++++++++

The ICS Consumer solution is implemented in Go and instead of polling MR itself, it registers as a consumer of the "STD_Fault_Messages" job in ICS.

