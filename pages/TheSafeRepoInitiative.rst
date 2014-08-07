:tocdepth: 2

.. _The SafeRepo Initiative:

=======================
The SafeRepo Initiative
=======================

.. contents::
    :backlinks: none

Introduction
============

The SafeRepo Initiative is a ‘proposal’ [if you will] for a standard in 3rd
party Yum repositories. Currently there is none. It is more of an idea, rather
than an official specification. The idea of safe repositories came about while
building out the IUS Community Project, and as such this proposal is lead by
the IUS Core Development Team. We strive to follow this policy whenever
possible.  There is one notable exception, see
:doc:`IUSvsSoftwareCollections`.

Terminology
===========

* **Distro** or **Stock Distro**: Refers to a Linux distribution as it comes from the
  vendor, such as Redhat, CentOS, Fedora, Etc.
* **Repo** or **3rd Party Repo**: Refers to any secondary yum repository that a Linux
  system can subscribe to in order to install additional packages that the Stock
  Distro doesn’t provide by default.
* **3rd Party Package**: Refers to an rpm package that a 3rd Party Repo provides.
* **Stock Package**: Refers to an rpm package that a Stock Distro provides in
  default repositories.

The Current Problem with 3rd Party Repos
========================================

3rd party yum repositories introduce a level of unknown compatibility, as well
as unknown usage. One repo might offer 3rd Party Packages that are meant to
replace existing Stock Packages within a Stock Distro, and others might
explicitly only add packages to a distribution. However there are many that you
just don’t know what they will do when you subscribe, and often they simply
override Stock Packages from the Distro which is unsafe in a production
environment. 

Some examples of safe repositories:
-----------------------------------

* Fedora EPEL: Extra Packages for Enterprise Linux is a repository geared towards
  adding packages to RHEL, but have strict guidelines that packages will never
  replace or conflict with existing RHEL packages. Therefore, subscribing to EPEL
  is safe in that nothing will happen unless you explicitly install a package
  from the repo. 

* IUS Community: The IUS Community Project provides packages for RHEL that follow
  the latest upstream stable source versions of specific software such as PHP,
  MySQL, Python, Etc. IUS is a safe repo because it uses alternate package names
  that provide the same software (for example php52 provides php) but do not
  obsolete. IUS packages explicitly conflict with stock packages, therefore
  preventing anything from IUS automatically installing on/over RHEL. 

So what is an ‘unsafe repository’? 
-----------------------------------

* Any 3rd Party Repo offering 3rd Party Packages that obsolete or directly
  replace Stock Packages because they have the same package name. For example, if
  a repository has ‘php-5.2.11′ and you subscribe to it, your server will
  automatically upgrade PHP to that version. 

* Any 3rd Party Repo with packages that explicitly Obsolete Stock Distro
  Packages. 

Making The Community Aware
==========================

System administrators and end users need to know if a 3rd Party Repo they are
subscribing a server to is safe. By choosing to follow the development
practices of the SafeRepo Initiative tells your users that you value
standardization and the assurance that using your custom package repository
isn’t going to bork their otherwise clean/stock Linux distribution.

SafeRepo is not a certification, rather a push for awareness. If you are
interested in making your repositories safer, see the specification below. Once
you feel your repo meets the requirements, add a notice to your site that says
something to the affect of “This site is SafeRepo Aware!”. Be sure to link it
back to us so they know what it means. Additionally, If you’d like to be added
to our list of known safe repositories just send us an email to
coredev@iuscommunity.org. 

The SafeRepo Specification
==========================

The following outlines the basic steps and requirements necessary to make your
3rd party yum repo SafeRepo Aware. Package requirements to provide a SafeRepo
are simple, though differ based on the type of package they are. The following
are general package types as they relate to creating a safe repository of 3rd
party packages:

* Addon Packages
* Replacement Packages
* Parallel Installable Packages 

.. _Addon_Packages:

Addon Packages
==============

This type of package provides software that does not currently exist in the
Stock Distro. The Fedora EPEL repositories are perfect examples of strictly
addon packages. The following rules apply:

* Can not Obsolete a Stock Distro Package
* Can not have the same name as another Stock Distro Package
* Does not Conflict with a Stock Distro Package. If it does, it is a
  Replacement Packages.
* Must not automatically install, upgrade, or replace Stock Distro Packages
  when subscribing to the repo. Should the Stock Distro later add the package,
  the 3rd Party Repo should then remove the package from its repo. 

.. _Replacement_Packages:

Replacement Packages
====================

This type of package Provides the same software that another Stock Package
(i.e. php53 provides php). The IUS Community Project is an example of a 3rd
Party Repo that provides Replacement Packages. The following rules apply:

* Can not Obsolete a Stock Distro Package
* Can not have the same name as another Stock Distro Package
* Must Provide the software it is replacing and be [more or less] compatible
  with other Stock Distro Packages that require it.
* Must explicitly Conflict with a Stock Distro Package (via the spec). For
  example the package php53 “Conflicts: php < 5.3″, but it “Provides: php =
  %{version}”. 

Must not automatically install, upgrade, or replace Stock Distro Packages when
subscribing to the repo. 

.. _Parallel_Installable_Packages:

Parallel Installable Packages
=============================

This type of package is very much like a Replacement Package, however it is
meant to be installed side-by-side with the Stock Distro Package that it would
otherwise replace. Some distros sometimes use this technique to introduce newer
software while not interrupting the system and software that require the older
version of it. Python for example is a system critical piece of software.
Upgrading it will always cause issues, however by parallel installing a newer
version of python you have the best of both words. Users/Applications that
require a newer version of that software can explicitly call the alternate
location while other software continues to work fine. The following rules
apply:

* Can not Obsolete a Stock Distro Package:
* Can not have the same name as another Stock Distro Package
* Can not Provide the software it is installing next to. For example,
  python26 does not “Provide: python” because this might confuse other Stock
  Distro Packages to think that it can find python and its libraries in the stock
  location (it can’t).
* Must not automatically install, upgrade, or replace Stock Distro Packages
  when subscribing to the repo.
* Executable binaries must be renamed with the major version number. I.e.
  ‘/usr/bin/python’ -> ‘/usr/bin/python2.7′.
* An identifier can/should be added to the release, such as Release:
  1.ius.parallel%{?dist}.
* All directories must have alternate paths. I.e. ‘/usr/lib/python2.6’ ->
  ‘/usr/lib/python2.7′.
