:tocdepth: 2

.. _Fedora Packaging Guidelines: https://fedoraproject.org/wiki/Packaging:Guidelines
.. _Fedora EPEL: https://fedoraproject.org/wiki/EPEL
.. _Package Wish List: https://bugs.launchpad.net/ius/+bugs?field.tag=wishlist
.. _LaunchPad: https://launchpad.net/ius
.. _IUS Community Project: http://code.launchpad.net/ius

.. _IUSDeveloperGuide:

===================
IUS Developer Guide
===================

.. contents::
    :backlinks: none
    
Introduction
============

This article outlines the basics of RPM packaging guidelines for IUS. It is a
work in progress. If you need help on how to configure a local build system see
:ref:`Configuring a Dedicated RPM Development Box`.

Package Guidelines
==================

The following outlines standard guidelines that packaging developers should
follow when creating and working with packages for IUS. In general we attempt to
follow the `Fedora Packaging Guidelines`_ where possible, which should be a
starting point for anyone just getting into packaging.

Package Categories
==================

The packaging model of IUS is broken up into two package categories:

Primary Packages:
-----------------

The packages represent our purpose in maintaining these
repositories. All primary packages will be maintained inline with upstream
stable sources. In short, these are the packages that we focus our time on most,
and have the highest priority. Almost always, with little exception, these
packages explicitly replace a package that already exists in RHEL. For example:
mysql50 and mysql51 both explicitly replace mysql as shipped with RHEL.

Secondary Supporting Packages:
------------------------------

Supporting packages are those that have been
added to enhance the usability and appeal of the Primary Packages. These will
most often be additional libraries such as PECL for PHP, or libraries for
Python. Supporting Packages might not replace anything in RHEL, but sometimes
do. In should be noted that IUS is not focused on adding extra packages for
RHEL. If we want to package something additional for stock RHEL we will do so
through `Fedora EPEL`_. Supporting Packages might not follow upstream stable
sources in the fashion that the Primary Packages do. If it's stable, and
works... we may not update these packages for a long time (or until an update
is requested).

Package Types
=============

See the SafeRepo Initiative specification.

Addon Packages
--------------

IUS Does Not Add Packages to RHEL. IUS provides newer versions of existing
software. If you have a package that you want to add to RHEL, you should go
through `Fedora EPEL`_. The exception to this is Supporting Packages. For example,
php52 replaces stock php... however we might add php52-pecl-XXXX that might not
exist in RHEL but adds features for the use of php52. Another exception is m
ysql50-percona-highperf, which replaces mysql but is not just a newer version of
the same software, it is patched by Percona.

Replacement Packages
--------------------

This type of package Provides the same software that another Stock Package
(i.e. php53 provides php). The following rules apply:

Can not Obsolete a Stock Distro Package
Can not have the same name as another Stock Distro Package
Must Provide the software it is replacing and be [more or less] compatible with
other Stock Distro Packages that require it.
Must explicitly Conflict with a Stock Distro Package (via the spec).
For example the package php53 "Conflicts: php < 5.3", but it "Provides:
php = %{version}-%{release}".
Must not automatically install, upgrade, or replace Stock Distro Packages
when subscribing to the repo.

Parallel Installable Packages
-----------------------------

This type of package is very much like a Replacement Package, however it is
meant to be installed side-by-side with the Stock Distro Package that it would
otherwise replace. Some distros sometimes use this technique to introduce newer
software while not interrupting the system and software that require the older
version of it. Python for example is a system critical piece of software.
Upgrading it will always cause issues, however by parallel installing a newer
version of python you have the best of both words. Users/Applications that
require a newer version of that software can explicitly call the alternate
location while other software continues to work fine. The following rules apply:

 * Can not Obsolete a Stock Distro Package
 * Can not have the same name as another Stock Distro Package
 * Can not Provide the software it is installing next to. For example,
   python26 does not "Provide: python" because this might confuse other Stock
   Distro Packages to think that it can find python and its libraries in the
   stock location (it can't).
 * Must not automatically install, upgrade, or replace Stock Distro Packages
   when subscribing to the repo.
 * Executable binaries must be renamed with the major version number. I.e.
   '/usr/bin/python' -> '/usr/bin/python2.6'.
 * An identifier can/should be added to the release, such as Release:
   1.ius.parallel%{?dist}.
 * All directories must have alternate paths. I.e. '/var/lib/mysql' ->
   '/var/lib/mysql51'.
   
Additional Packaging Notes
==========================

IUS Packages Provide/Conflict - Never Obsolete
----------------------------------------------

Packages in the IUS repository never obsolete a RHEL package directly. Meaning,
if I subscribe to an IUS repo nothing will update automatically. See :ref:`The
SafeRepo Initiative` for more on this. However, I can remove a RHEL package and
replace it with an IUS package that provides the same package. This is
accomplished through a few steps that need to be added to the spec file of the
IUS package.

Lets take the 'php52' package for example. This package 'Provides: php', but
does not 'Obsolete: php'. If we obsoleted 'php' then as soon as we subscribed a
system to IUS yum would attempt to update 'php' with the 'php52' counterparts.
This is not desired. The following directives in the spec help perform this::
    
    %define basever 5.2
    %define real_name php
    %define name php52
    
    ... snip ...
    
    Provides:  %{real_name} = %{version}-%{release}
    Conflicts: %{real_name} < %{basever}
    Conflicts: php51
    
.. note::
    This has to be done for all subpackages accordingly as well.

Assuming that we have other packages such as an older 'php51' for other branches
of PHP we want our 'php52' package to conflict with those packages. We also want
our 'php52' package to conflict with 'php < 5.2'. Because we are building
specifically for the purpose of upgrading to a newer branch of software, it is
safe to assume that RHEL will never upgrade PHP to 5.2 (using RHEL 5 in the
example).

Note: The exception to obsoletes is when an IUS package obsoletes another IUS
package... for example, php53-pear might have obsoleted php-pear18.

A Note on External Sub Packages
-------------------------------

The above example works well for 'base' packages. For example, 'php' is the base
package... it's real name is 'php' (which is also the source name) and the IUS
name is 'php52' or 'php53'. That said, it is not as clean of an example for
external sub packages such as 'php-eaccellerator' which is packaged for 'php5x'
but not together with the base package. So for example you might do::

    %global php_basever 5.2
    
    Name: php52-eaccelerator
     
    ... snip ...
    
    Requires: php52 >= %{php_basever}
    Provides: php-eaccelerator = %{version}-%{release}
    
You'll notice that we do not 'Conflict: php-eaccelerator < %{base_ver}' because
in this context we are not upgrading php-eaccelerator to another major branch,
we updated php to another major branch. Since the base package 'php52' already
handled the conflict with 'php < %{basever}' and php52-eaccelerator requires
'php52' it is safe to assume the hard conflicts will be handled.

IUS Packages Follow Fedora Package Collection
---------------------------------------------

Because Fedora is upstream to RHEL, IUS follows changes that the Fedora
maintainers make upstream. Meaning, anytime we make updates to a package we
pull the latest Fedora SRPM as well and implement any patches/changes/etc that
are relevant. We do not replace our SRPM with the Fedora SRPM... but rather
manually go through the latest Fedora spec and make any relavant changes that
haven't been made yet.

.. _Package_Wish_List:

Package Wish List
=================

IUS users are encouraged to submit packages to the `Package Wish List`_ when they
want something added to IUS. This is a good place to start for new contributors
who want to help with packaging bug might not have any packages in mind.

.. _Managing_Updates:

Managing Updates
================

See :ref:`Managing Updates`.

Converting Existing Packages For IUS
====================================

In general, when creating a new IUS package you will start with the SRPM of the
software from the latest version in RHEL, or more preferably Fedora (since we
are building the latest sources from upstream) and build up from there. We
want our packages to follow Redhat/Fedora standards as much as possible to
ensure seamless upgrades from stock RHEL to IUS packages.

The following points are examples, but not limited to the changes that will need
to be made to the spec to build for IUS.

Rename The Spec File
--------------------

The spec file should match the new name of the package.
Using php as an example::

    you@linuxbox buildroot]$ mv SPECS/php.spec SPECS/php52.spec

Add The IUS Release Tag
-----------------------

IUS packages are designated by a '.ius' tag in the release. For community
packages, this should just be '.ius'. Enterprise packages will likely be tagged
with '.ius.ent' or simply '.rs' as they are now::

    Release: 1.ius%{?dist}

Define The Base Version, and Real Name
--------------------------------------

The base version is important as its used further down in the spec and makes
things clean. Additionally, we need to reference the 'real name' of the package
through out the spec. The following should be added to the top of the spec
before the Preamble::

    %define basever 5.2
    %define real_name php
    %define name php52
    
.. note::
    We are using php as an example. Replace names accordingly.

Add the Provides/Conflicts for the Base Package


Provides:  %{real_name} = %{version}-%{release}
Conflicts: %{real_name} < %{basever}
Conflicts: php51

.. note::
    The 'Conflicts: php51' should list any ius packages previous to this
    one (php52 in our example). It does not need to list ius packages newer than it.

Update All Sub Packages
-----------------------

All sub packages need to both provide the real name of the sub package, as well
as require the new name of the package. Take the php52-devel package for
example::

    %package devel
    Group: Development/Libraries
    Summary: Files needed for building PHP extensions.
    Requires: %{name} = %{version}-%{release}, autoconf, automake
    Provides: %{real_name}-devel = %{version}-%{release}
    Conflicts: %{real_name}-devel < %{base_ver}
    
    %description devel
    The php-devel package contains the files needed for building PHP
    extensions. If you need to compile your own PHP extensions, you will
    need to install this package.
    
.. note::
    We need to provide the '%{real_name}' of the sub package to resolve
    any dependencies in the system that are looking for the stock version of the
    software.

Modify The %setup Line
----------------------

We need to tell %setup to use the real name of our software here::

    %setup -q -n %{real_name}-%{version}

A Note On BuildRequires/Requires
--------------------------------

This can sound a bit confusing, however it should be noted that IUS packages
should not explicitly BuildRequire/Require a stock RHEL packages that another
IUS package replaces. This can cause dependency hell during the build process
with Mock because yum calls for 'XXX' package and if nothing is already
installed you might get a conflict between all the packages that provide 'XXX'
package. For example.. if a php52-pecl-pear package has a 'BuildRequires:
php-pear' yum will freak out because php-pear requires php-cli ... and even
though php52-cli provides php-cli, php52 isn't installed yet... so yum tries to
install php along with php-cli .... and things just explode. For that example,
we now have 'php52-pear' which isn't really needed since php-pear works fine...
but this allows us explicitly make sure that php52 gets installed at build time
and not php.


Contributing Packages or Changes
================================

Currently we do not have a public build farm setup. We are debating whether to
move everything to a dedicate Koji instance, or continue development on our
existing build system and setup a public instance of that. In the mean time
developers that want to contribute can simply branch our bazaar repos from which
we can merge from and submit the builds to the build farm.

Branching Bazaar Repos
----------------------

Each package has its own branch hosted on our `LaunchPad`_ project page. You can
create your own branch, make changes, and then request a merge. Once merges have
been approved your changes will appear in the next package release.

Assuming you have a `LaunchPad`_ account, the following is an example of merging
from the official ius branch to make your changes::

    you@linux ]$ mkdir ius
    
    you@linux ]$ cd ius
    
    you@linux ]$ bzr launchpad-login <your_launchpad_login>
    
    you@linux ]$ bzr init
    
    you@linux ]$ bzr branch lp:~ius-coredev/ius/php52
    Branched 7 revision(s).   
    
    you@linux ]$ cd php52
    
    you@linux ]$ ls -lah
    total 32K
    drwxrwxr-x 8 you you 4.0K Oct 13 15:52 .
    drwxrwxr-x 4 you you 4.0K Oct 13 15:50 ..
    drwxrwxr-x 2 you you 4.0K Oct 13 15:52 BUILD
    drwxrwxr-x 6 you you 4.0K Oct 13 15:52 .bzr
    drwxrwxr-x 2 you you 4.0K Oct 13 15:52 RPMS
    drwxrwxr-x 2 you you 4.0K Oct 13 15:52 SOURCES
    drwxrwxr-x 2 you you 4.0K Oct 13 15:52 SPECS
    drwxrwxr-x 2 you you 4.0K Oct 13 15:52 SRPMS


Modifying Packages
------------------

After making changes, you want to make sure that they build (and you probably
want to test installing and using the RPMs as well). It is recommended that
packagers use the Fedora Mock utility for building as this ensures builds are
clean and all dependencies are resolved. After modifying, an example build might
look like (don't forget to up the release)::

    you@linux ]$ rpmbuild -bs SPECS/php52.spec --nodeps
    Wrote: /home/you/ius/php52/SRPMS/php52-5.2.11-2.ius.src.rpm
    
    you@linux ]$ mock -r ius-5-x86_64 rebuild SRPMS/php52-5.2.11-2.ius.src.rpm
    INFO: mock.py version 0.9.14 starting...
    State Changed: init plugins
    State Changed: start
    INFO: Start(SRPMS/php52-5.2.11-2.ius.src.rpm)  Config(ius-5-x86_64)
    State Changed: lock buildroot
    State Changed: clean
    State Changed: init
    State Changed: lock buildroot
    Mock Version: 0.9.14
    State Changed: running yum
    State Changed: setup
    State Changed: build
    INFO: Done(SRPMS/php52-5.2.11-2.ius.src.rpm) Config(ius-5-x86_64) 13 minutes 10 seconds
    INFO: Results and/or logs in: /var/lib/mock/ius-5-x86_64-you/result

For more information, and an example IUS Mock config, see the Building Packages
with Fedora Mock section. Should you're build, and testing be successful you
then want to commit your changes.

Committing Changes To Launch Pad
--------------------------------

Under the `IUS Community Project`_ branches, click 'Register a Branch'.

 * Name: <package_name>
 * Type: Hosted
 * Status: Development
 * Register Branch
 
This creates a branch like lp:~you/ius/php52. You want to commit changes locally
first and include a detailed log of the changes you made. Then, for the IUS
CoreDev Team to be able to merge your changes in you need to commit to the
`LaunchPad`_ branch under your accound::

    you@linux ]$ bzr add SOURCES/php-5.2.11-mysourcechange.patch
    
    you@linux ]$ bzr commit -m 'Adding patch to fix something in the source.'
    
    you@linux ]$ bzr push lp:~you/ius/php52 --use-existing-dir
    Created new branch.  

Once your branch is complete, go back to your Branches page for your user, click
the branch and then click 'Propose for merging into another branch'. At this
point you want to choose the branch for the package. Note, this is the target
branch (where the proposed changes need to be applied).


Building Packages with Fedora Mock
==================================

The Fedora Mock utility is the preferred way of building packages locally.

Installing Mock
---------------

The mock package can be installed from Fedora, and Fedora EPEL repositories::

    root@linux ~]# yum install mock.noarch
    
    root@linux ~]# usermod -aG mock <username>
    
.. note::
    All users building with mock need to be added to the mock system group
    (as I did above for <username>).


Example IUS Mock Config
-----------------------

Copy the following to /etc/mock/ius-5-x86_64.cfg::

    config_opts['root'] = 'epel-5-x86_64'
    config_opts['target_arch'] = 'x86_64'
    config_opts['legal_host_arches'] = ('x86_64',)
    config_opts['chroot_setup_cmd'] = 'install buildsys-build'
    config_opts['dist'] = 'el5'  # only useful for --resultdir variable subst
    config_opts['macros']['%__arch_install_post'] = '%{nil}'
    
    config_opts['yum.conf'] = """
    [main]
    cachedir=/var/cache/yum
    debuglevel=1
    logfile=/var/log/yum.log
    reposdir=/dev/null
    retries=20
    obsoletes=1
    gpgcheck=0
    assumeyes=1
    syslog_ident=mock
    syslog_device=
    
    # repos
    
    [core]
    name=base
    mirrorlist=http://mirrorlist.centos.org/?release=5&arch=x86_64&repo=os
    
    [update]
    name=updates
    mirrorlist=http://mirrorlist.centos.org/?release=5&arch=x86_64&repo=updates
    
    [groups]
    name=groups
    baseurl=http://buildsys.fedoraproject.org/buildgroups/rhel5/x86_64/
    
    [extras]
    name=epel
    mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=epel-5&arch=x86_64
    
    [testing]
    name=epel-testing
    enabled=0
    mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=testing-epel5&arch=x86_64
    
    [local]
    name=local
    baseurl=http://kojipkgs.fedoraproject.org/repos/dist-5E-epel-build/latest/x86_64/
    cost=2000
    enabled=0
    
    [epel-debug]
    name=epel-debug
    mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=epel-debug-5&arch=x86_64
    failovermethod=priority
    enabled=0
    
    [ius]
    name=ius
    mirrorlist=http://dmirr.iuscommunity.org/mirrorlist?repo=ius-el5&arch=$basearch
    """

Building With Mock
------------------
::

    you@linux buildroot]$ vi SPECS/mypackage.spec
    
    you@linux buildroot]$ rpmbuild -bs SPECS mypackages.spec
    
    you@linux buildroot]$ mock -r ius-5-x86_64 rebuild SRPMS/mypackage-0.1-1.ius.src.rpm
    
Results will be in /var/lib/mock/ius-5-x86_64.

