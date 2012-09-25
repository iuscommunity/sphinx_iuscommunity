:tocdepth: 2

.. _End User Agreement: http://dl.iuscommunity.org/pub/ius/IUS-COMMUNITY-EUA
.. _Launchpad IUS Bug #453543: http://bugs.launchpad.net/ius/+bug/453543
.. _Yum Bug #296: http://yum.baseurl.org/ticket/296
.. _Red Hat Bug #529719: https://bugzilla.redhat.com/show_bug.cgi?id=529719

.. _GettingStarted:

===============
Getting Started
===============

.. contents::
    :backlinks: none

Before using any packages from the IUS Community repositories, you should first
read and agree with the `End User Agreement`_.

All technical collaboration, bug tracking, blueprint planning, etc takes place
at LaunchPad/IUS.

General discussions happen within the IUS Community Members mailing list.
Join the team on Launchpad and you will be able to subscribe to the list.

Reporting Bugs and Feature Requests
===================================

Bugs and Feature Requests can be submitted via our Launchad tracking system.

Accessing our Yum Repositories
===============================

Our official yum repository is available at http://dl.iuscommunity.org/pub/ius.
Additional mirrors are available, and can be viewed here.

.. _Subscribing_to_the_IUS_Yum_Repository:

Subscribing to the IUS Yum Repository
=====================================

Subscribing to the IUS Community repositories is as easy as installing an RPM
package. Within the repo are two packages called 'epel-release', and
'ius-release'. The packages configure your system to use the Fedora EPEL and
IUS Community repositories, as well as installs the GPG keys necessary to
validate signed packages of both.

**Note:** IUS Packages replace stock RHEL packages, however they do not
obsolete them. Meaning, you can't just 'yum upgrade' and get our packages...
you need to first remove the stock RHEL package such as mysql, and replace it
with the IUS package such as mysql51. See :doc:`IUSClientUsageGuide` for full
examples of installing software from IUS.

Known Yum Dependency Resolution Issues
======================================

The IUS CoreDev Team is aware of an issue with older versions of Yum and how it
resolves dependencies when installing packages. For background on this matter
please see the upstream bug reports that we have submitted:

 * `Launchpad IUS Bug #453543`_
 * `Yum Bug #296`_
 * `Red Hat Bug #529719`_

Please be sure that you are using yum > 3.2.26, or RHEL's version 3.2.22-23.
