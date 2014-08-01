:tocdepth: 2

.. _2013-09-12: http://developerblog.redhat.com/2013/09/12/rhscl1-ga/
.. _Software Collections: https://www.softwarecollections.org
.. _Terminology: https://iuscommunity.org/pages/TheSafeRepoInitiative.html#terminology
.. _The SafeRepo Initiative: https://iuscommunity.org/pages/TheSafeRepoInitiative.html
.. _replacement: https://iuscommunity.org/pages/TheSafeRepoInitiative.html#replacement-packages
.. _parallel installable: https://iuscommunity.org/pages/TheSafeRepoInitiative.html#parallel-installable-packages
.. _Filesystem Hierarchy Standard: http://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard
.. _addon: https://iuscommunity.org/pages/TheSafeRepoInitiative.html#addon-packages
.. _Life Cycle: https://access.redhat.com/support/policy/updates/rhscl/

============================
IUS vs. Software Collections
============================

.. contents::
    :backlinks: none

Introduction
============

On `2013-09-12`_, Red Hat released a product called `Software Collections`_,
also know as SCL.  The front page has this statement::

    Software Collections give you power to build, install, and use multiple
    versions of software on the same system, without affecting system-wide
    installed packages.

This may sound like a similar goal as IUS, but there are important differences in execution.

Terminology
===========

Please refer to the `Terminology`_ section of `The SafeRepo Initiative`_.

Types of Packages
=================

* IUS provides `replacement`_ and `parallel installable`_ packages.
* SCL provides `addon`_ packages only.

File Locations
==============

* Files from IUS packages are installed to standard `Filesystem Hierarchy
  Standard`_ locations (/usr/bin, /usr/lib, etc).
* Files from SCL packages are installed under /opt/rh.

Usage
=====

* Usage of IUS packages are the same or very close to stock packages::

    php -v
    service mysqld start
    mysql -V
    python2.7 -V


* Usage of SCL packages are very different from stock packages::

    scl enable php54 "php -v"
    service mysql55-mysqld start
    scl enable mysql55 "mysql -V"
    scl enable python27 "python -V"

Releases
========

* IUS packages are updated in line with the upstream software project.  This
  means that once upstream releases a new version, we will begin building RPMs
  as soon as possible.
* The SCL `Life Cycle`_ states that new packages will be released every 18
  months.  These release are snapshots with backported security fixes, and are
  supported for a period of three years.
