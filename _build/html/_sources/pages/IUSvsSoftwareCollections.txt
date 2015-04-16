:tocdepth: 2

.. _2013-09-12: http://developerblog.redhat.com/2013/09/12/rhscl1-ga/
.. _Software Collections: https://www.softwarecollections.org
.. _Filesystem Hierarchy Standard: http://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard
.. _Product Life Cycle: https://access.redhat.com/support/policy/updates/rhscl/
.. _RHEL 5 MySQL 5.5 migration guide: https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/5/html/Deployment_Guide/ch-Migrating_from_MySQL_5.0_to_MySQL_5.5.html

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
  as soon as possible.
* The SCL `Product Life Cycle`_ states that new packages will be released
  every 18 months.  These release are snapshots with backported security fixes,
  and are supported for a period of three years.

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
