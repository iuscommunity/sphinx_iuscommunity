:tocdepth: 2

.. _2013-09-12: http://developerblog.redhat.com/2013/09/12/rhscl1-ga/
.. _Software Collections: https://www.softwarecollections.org
.. _Filesystem Hierarchy Standard: http://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard
.. _Product Life Cycle: https://access.redhat.com/support/policy/updates/rhscl/
.. _RHEL 5 MySQL 5.5 migration guide: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/ch-Migrating_from_MySQL_5.0_to_MySQL_5.5.html
.. _softwarecollections.org: https://www.softwarecollections.org
.. _mailing list post: https://www.redhat.com/archives/sclorg/2014-November/msg00005.html

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

Here is our mission statement for the IUS Community project::

    The IUS Community Project is aimed at providing up to date and regularly
    maintained RPM packages for the latest upstream versions of PHP, Python,
    MySQL and other common software specifically for Red Hat Enterprise Linux.
    IUS can be thought of as a better way to upgrade RHEL, when you need to.

Both projects seek to solve the same problem, but there are several important
differences in execution.

Types of Packages
=================

* IUS provides :ref:`Replacement_Packages` and :ref:`Parallel_Installable_Packages`.
* SCL provides :ref:`Addon_Packages`.

File Locations
==============

* Files from IUS packages are installed to standard `Filesystem Hierarchy
  Standard`_ locations (/usr/bin, /usr/lib, etc).
* Files from SCL packages are installed under /opt.

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
  as soon as possible.  It also means that once software is declared "End of
  Life" (EOL) by the upstream project, we remove it from our stable
  repositories.
* The `Product Life Cycle`_ for Red Hat Software Collections (RHSCL) states
  that "Production Collections" are supported with security updates for three
  years.  Major updates are grouped together and released as a new collection.
* Software Collections from `softwarecollections.org`_ are community driven and have no
  guarantees.  Please see this `mailing list post`_.


FAQ
===

Q. What is the difference between mysql55-server and mysql55-mysql-server?

A. The package mysql55-server is from the IUS repo.  The package
   mysql55-mysql-server is an SCL RPM that Red Hat chose to add to the base EL5
   repos.  Please refer to the `RHEL 5 MySQL 5.5 migration guide`_ for more
   information.

More Info
=========

* https://www.softwarecollections.org
* https://iuscommunity.org/pages/TheSafeRepoInitiative.html
* https://www.redhat.com/about/news/archive/2013/6/red-hat-software-collections-1.0-beta-now-available
* http://developerblog.redhat.com/2013/09/12/rhscl1-ga/
* https://access.redhat.com/site/documentation/en-US/Red_Hat_Developer_Toolset/1/html/Software_Collections_Guide/
* https://access.redhat.com/support/policy/updates/rhscl/
