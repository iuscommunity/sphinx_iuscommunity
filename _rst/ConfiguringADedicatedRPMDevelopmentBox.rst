:tocdepth: 2

.. _Configuring a Dedicated RPM Development Box:

===========================================
Configuring a Dedicated RPM Development Box
===========================================

.. contents::
    :backlinks: none
    
Introduction
============

This article serves to outline the steps necessary to configure a dedicated RPM
Development server. There are some thing to consider depending on how this system
is to be used:

 * **Minimal Install**. The majority of work will be done via the Fedora Mock
   utility, therefore you really don't need or want to have any unnecessary
   software or services running on the system that could effect performance or
   open up security holes.
 * **Locked Down**. If this system is to be used as the primary development server,
   it is also likely that you might want to sign RPM with a custom GPG key. The
   private GPG Key must be kept as secure as possible, therefore there should be
   no incoming access allowed to this server. SSH should really be the only
   incoming service, but should be locked down to specific management IPs rather
   than globally open.
 * **Limited User Access**. It is easy to simply hand out sudo access to all the
   users who need to access this system, however unless all of those users are
   trusted with your private GPG Key it is strongly recommended that user access
   be limited. If you are in an environment where a lot of users want/need
   access to this development server then it is recommended that you have yet
   another dedicated server strictly for signing RPMs with the private GPG Key.

System Requirements
-------------------

Building RPMs with Mock is primarily CPU intensive, especially for larger
packages that require a lot of source code compilation. That said, more CPU is
better but the following would be recommended minimum specs:

 * Redhat/CentOS 5+ or Fedora Core 10+ (Minimal Install)
 * 64bit OS Installation (required to build i386 and x86_64 packages)
 * 2Ghz CPU or better
 * 512M Memory or better
 * Several Gigs of free hard drive space. Each build performed by Mock create a
   chrooted install, and installs all necessary RPMs required to build the SRPM
   you are working with. This can range from a few hundred megs to over a gig
   depending. Mock does do caching which helps, however until files are removed
   the disk usage will grow with each build (for different targets).

For this article I am using a minimal install CentOS 5.4 installation.

Conventions and Vocabulary
--------------------------

All commands are prefixed with a generic shell, and appear in a code block i
such as::

    [user@mockbuild]$ echo "This is a regular user shell"
    
    [root@mockbuild]# echo "This is a root shell"

Take note of the user [user@, root@] and shell variable [#, $] for each command.
Some pieces of the article require root access, some are expected to be run as a
regular user account.

The hostname we use is 'mockbuild' to signify this machine we are working on.
If the pwd [present working directory] is important, we will first prefix any
commands with a 'cd' [change directory] statement to signify that you should
change to a specific directory. For example::

    [user@mockbuild]$ mkdir ~/buildroot/{RPMS,SRPMS,BUILD,SPECS,SOURCES} -p
    
    [user@mockbuild]$ cd ~/buildroot
    
    [user@mockbuild]$ ls -lah
    total 28K
    drwxrwxr-x 7 user user 4.0K Nov  4 20:57 .
    drwx------ 4 user user 4.0K Nov  4 20:57 ..
    drwxrwxr-x 2 user user 4.0K Nov  4 20:57 BUILD
    drwxrwxr-x 2 user user 4.0K Nov  4 20:57 RPMS
    drwxrwxr-x 2 user user 4.0K Nov  4 20:57 SOURCES
    drwxrwxr-x 2 user user 4.0K Nov  4 20:57 SPECS
    drwxrwxr-x 2 user user 4.0K Nov  4 20:57 SRPMS

The two users we primarily use are 'user' and 'root'.

Objectives
==========

The following are the objectives we plan to accomplish throughout this article:

 * Have a clean host system dedicated for local RPM Development
 * Secure the server from outside access
 * Optimize the OS Environment for Mock

OS Configuration
================

This article assumes you are using a minimal 64bit install of either
Redhat/CentOS EL5+ or Fedora Core 10+. The minimal install ensure that the
system is not bloated with unnecessary files or services. The following also
assumes that you have root access on the server.

Ensure The System is Up-To-Date
-------------------------------

You first want to ensure the system is up-to-date with any available patches.
This is easily done with the following::

    [root@mockbuild]# yum upgrade

Optionally, you may wish to ensure the system is configured for automatic
nightly updates [recommended]. This is easily configured via the yum-cron
package::

    [root@mockbuild]# yum install yum-cron

By default, yum-cron will run nightly and apply any updates that are available
for your system. If this is not preferred you can also set **CHECK_ONLY=yes** in the
configuration file **/etc/sysconfig/yum-cron**. This will check for updates,
but not apply them.

Ensure System is Secured
------------------------

As with any system you want to ensure that there is no unauthorized access,
especially when talking about signing packages with a GPG Key. The system that
has the private GPG key install but be as secure as possible. Ensuring proper
security is a bit outside of the scope of this article, however the most basic
measure would be to ensure the following:

 * Allow RELATED, and ESTABLISHED incoming traffic
 * Allow NEW, RELATED, and ESTABLISHED outgoing traffic
 * Allow incoming SSH connections from only specific IP Addresses or
   ranges(do not use DNS hostnames)
 * Reject or Drop all other traffic.

Configure 3rd Party Yum Repositories
------------------------------------

You may wish to have 3rd party yum repos setup as well, though keep in mind that
this is for the host system only. You have to add 3rd party repo configs to your
mock configuration files as well if you wish to build against packages in the
3rd party repo (more on that later). Because we want to use the Fedora Mock
utility, we want to install the Fedora EPEL repository::

    [root@mockbuild]# rpm -Uvh http://download.fedora.redhat.com/pub/epel/5/i386/epel-release-5-3.noarch.rpm

This will create the yum repositories for Fedora EPEL in
**/etc/yum.repos.d/epel.repo** as well as import the EPEL GPG Key.

Install Fedora Mock and Other Packages
--------------------------------------

The Fedora Mock utility is in EPEL, and can be installed via Yum::

    [root@mockbuild]# yum install mock.noarch rpm-build


Configure Global Environment Settings
-------------------------------------

There are a number of global settings we can make that will optimize the use of
this system for RPM Development with Mock.

**/etc/profile.d/mock.sh**:

We prefer to add an alias for the mock command that adds a unique extension to
the end of each build. This is critical on a shared system where you might have
multiple developers building against the same target. Add the following to
**/etc/profile.d/mock.sh** as well as any other global mock environment changes you
need::

    alias mock="mock --uniqueext=$USER"
    
And make the file executable::

    [root@mockbuild]# chmod +x /etc/profile.d/mock.sh

**/etc/skel**:

Setting up a default /etc/skel can help provide a decent 'starting point' for
users, and help encourage a common practice on working with files. The following
sets up /etc/skel.

Create the buildroot for packaging::

    [root@rpmbuild]# mkdir /etc/skel/packages
    
    [root@rpmbuild]# cd /etc/skel/packages
    
    [root@rpmbuild]# mkdir buildroot.clean/{RPMS,SRPMS,SPECS,BUILD,SOURCES} -p

Create the RPM Macros file to set rpmbuild defaults by adding the following to
**/etc/skel/.rpmmacros**::

    %_topdir %(pwd)
    %el5 1
    %rhel 5
    
*Note, these settings are for RHEL / CentOS 5 and would be different for Fedora
Core or others.*

We set each user's _topdir (default: /usr/src/redhat) to 'pwd' [present working
directory] as it makes working out of multiple build roots easier. Remember,
this system is dedicated to RPM Development! We will explain this a bit more in
detailed later in the article. Setting _topdir to 'pwd' is a preference and can
be modified based on each individuals needs.


Add / Modify Users
------------------

At this point you should have everything in place to setup users properly.

New Users::

    [root@mockbuild]# useradd <username>
    
    [root@mockbuild]# passwd <username>
    
    [root@mockbuild]# usermod -aG mock <username>

Existing Users::

    [root@mockbuild]# usermod -aG mock <username>
    
    [root@mockbuild]# sudo -u <username> cp -a /etc/skel/packages /home/<username>
    
    [root@mockbuild]# sudo -u <username> cp -a /etc/skel/.rpmmacros /home/<username>


Mock Configurations
===================

Mock comes with a number of default configurations for building against Fedora
Core and CentOS with Fedora EPEL. All target configuration files are in
**/etc/mock**.

Setting Global Defaults
-----------------------

Global defaults are in **/etc/mock/site-defaults.cfg**, of which each individual
target config can override. For the majority of Mock users, the defaults are
preferred and shouldn't really need to be modified.

Stock Build Targets
-------------------

As mentioned, mock comes with a number of stock build targets. As an example,
lets look at the mock config for Fedora Core 10 x86_64.

**/etc/mock/fedora-10-x86_64.cfg**::

    config_opts['root'] = 'fedora-10-x86_64'
    config_opts['target_arch'] = 'x86_64'
    config_opts['chroot_setup_cmd'] = 'groupinstall buildsys-build'
    config_opts['dist'] = 'fc10'  # only useful for --resultdir variable subst
    
    config_opts['yum.conf'] = """
    [main]
    cachedir=/var/cache/yum
    debuglevel=1
    reposdir=/dev/null
    logfile=/var/log/yum.log
    retries=20
    obsoletes=1
    gpgcheck=0
    assumeyes=1
    # grub/syslinux on x86_64 need glibc-devel.i386 which pulls in glibc.i386, need to exclude all
    # .i?86 packages except these.
    #exclude=[0-9A-Za-fh-z]*.i?86 g[0-9A-Za-km-z]*.i?86 gl[0-9A-Za-hj-z]*.i?86 gli[0-9A-Zac-z]*.i?86 glib[0-9A-Za-bd-z]*.i?86
    # The above is not needed anymore with yum multilib policy of "best" which is the default in Fedora.
    
    # repos
    
    [fedora]
    name=fedora
    mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=fedora-10&arch=x86_64
    failovermethod=priority
    
    [updates-released]
    name=updates
    mirrorlist=http://mirrors.fedoraproject.org/mirrorlist?repo=updates-released-f10&arch=x86_64
    failovermethod=priority
    
    [local]
    name=local
    baseurl=http://koji.fedoraproject.org/static-repos/dist-fc10-build-current/x86_64/
    cost=2000
    enabled=0
    """

Essentially, the config simply sets a number of unique configuration options
including the Yum config to use for the build. It is recommended to keep the
stock config files as-is so that you have a basis for 'building against a stock
distro' as apposed to custom targets that might include additional 3rd party
repositories.

Custom Build Targets
--------------------

Custom build targets can be created by simply copying a stock config and adding
the changes and additional 3rd party repositories that you might want to build
against. As an example, I will create a custom build target that builds against
CentOS 5 x86_64 + Fedora EPEL 5 + IUS 5 repositories.

**/etc/mock/centos-5-x86_64-epel-ius.cfg**::

    config_opts['root'] = 'centos-5-x86_64-epel-ius'
    config_opts['target_arch'] = 'x86_64'
    config_opts['chroot_setup_cmd'] = 'install buildsys-build'
    config_opts['dist'] = 'el5'  # only useful for --resultdir variable subst
    
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
    # grub/syslinux on x86_64 need glibc-devel.i386 which pulls in glibc.i386, need to exclude all
    # .i?86 packages except these.
    exclude=[0-9A-Za-fh-z]*.i?86 g[0-9A-Za-km-z]*.i?86 gl[0-9A-Za-hj-z]*.i?86 gli[0-9A-Zac-z]*.i?86 glib[0-9A-Za-bd-z]*.i?86
    
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
    baseurl=http://koji.fedoraproject.org/static-repos/dist-5E-epel-build-current/x86_64/
    cost=2000
    enabled=0
    
    [ius]
    name=ius
    mirrorlist=http://dl.iuscommunity.org/mirrorlist?repo=ius-el5&arch=$basearch
    """

A Sample Build with Mock
------------------------

The following steps outline a few basic commands for building and working with
mock. Keep in mind that every build should be out of its own build root.
Meaning, you don't want to build multiple packages out of the same buildroot as
that can lead to file clobbering and a lot of confusion. For this example I am
going to pull down an existing source RPM from EPEL, and rebuild it with Mock:

Setup the unique, dedicated build root for this package::

    [user@mockbuild]$ cd ~/packages
    
    [user@mockbuild]$ cp -a buildroot.clean libmcrypt
    
    [user@mockbuild]$ cd libmcrypt
    
    [user@mockbuild]$ ls -lah
    drwxr-xr-x 7 user user 4.0K Nov  4 23:14 .
    drwxr-xr-x 4 user user 4.0K Nov  5 00:55 ..
    drwxr-xr-x 2 user user 4.0K Nov  4 23:14 BUILD
    drwxr-xr-x 2 user user 4.0K Nov  4 23:14 RPMS
    drwxr-xr-x 2 user user 4.0K Nov  4 23:14 SOURCES
    drwxr-xr-x 2 user user 4.0K Nov  4 23:14 SPECS
    drwxr-xr-x 2 user user 4.0K Nov  4 23:14 SRPMS

Install the original source rpm::

    [user@mockbuild]$ rpm -Uvh http://mirror.rackspace.com/epel/5Server/SRPMS/libmcrypt-2.5.7-5.el5.src.rpm
    
    [user@mockbuild]$ ls -lah SPECS/
    total 12K
    drwxr-xr-x 2 user user 4.0K Nov  5 00:57 .
    drwxr-xr-x 7 user user 4.0K Nov  4 23:14 ..
    -rw-rw-r-- 1 user user 2.0K Oct  8  2006 libmcrypt.spec


Make changes, rebuild the source rpm, and build with mock::

    [user@mockbuild]$ rpmbuild -bs SPECS/libmcrypt.spec --nodeps
    Wrote: /home/user/packages/libmcrypt/SRPMS/libmcrypt-2.5.7-5.src.rpm
    
    
    [user@mockbuild]$ mock -r centos-5-x86_64-epel-ius rebuild SRPMS/libmcrypt-2.5.7-5.src.rpm
    INFO: mock.py version 0.9.14 starting...
    State Changed: init plugins
    State Changed: start
    INFO: Start(SRPMS/libmcrypt-2.5.7-5.src.rpm)  Config(centos-5-x86_64-epel-ius)
    State Changed: lock buildroot
    State Changed: clean
    State Changed: init
    State Changed: lock buildroot
    Mock Version: 0.9.14
    INFO: Mock Version: 0.9.14
    INFO: enabled root cache
    INFO: enabled yum cache
    State Changed: cleaning yum metadata
    INFO: enabled ccache
    State Changed: running yum
    State Changed: creating cache
    State Changed: setup
    State Changed: build
    INFO: Done(SRPMS/libmcrypt-2.5.7-5.src.rpm) Config(centos-5-x86_64-epel-ius) 5 minutes 26 seconds
    INFO: Results and/or logs in: /var/lib/mock/centos-5-x86_64-epel-ius-user/result

As you can see we made changes to the spec, and then just rebuilt the source rpm
in our local root. From there, we build our source rpm against the destination
target [centos-5-x86_64-epel-ius]. Take notice that our '<username>' is tagged
on to the end of the target work directory, making it unique to our user so
other users building against the same target won't clobber our build.

To view your results, see the result dir of the build::
    
    [you@mockbuild]$ ls -lah /var/lib/mock/centos-5-x86_64-epel-ius-user/result/
    total 1.3M
    drwxrwsr-x 2 user   mock 4.0K Nov  5 01:08 .
    drwxrwsr-x 4 root   mock 4.0K Nov  5 01:02 ..
    -rw-rw-r-- 1 user   mock 300K Nov  5 01:12 build.log
    -rw-r--r-- 1 user   mock 516K Nov  5 01:12 libmcrypt-2.5.7-5.el5.src.rpm
    -rw-r--r-- 1 user   mock 114K Nov  5 01:12 libmcrypt-2.5.7-5.el5.x86_64.rpm
    -rw-r--r-- 1 user   mock 176K Nov  5 01:12 libmcrypt-debuginfo-2.5.7-5.el5.x86_64.rpm
    -rw-r--r-- 1 user   mock 103K Nov  5 01:12 libmcrypt-devel-2.5.7-5.el5.x86_64.rpm
    -rw-rw-r-- 1 user   mock  55K Nov  5 01:12 root.log
    -rw-rw-r-- 1 user   mock  570 Nov  5 01:12 state.log

Should the build fail, you can view the 'build.log' in the result dir for the
output of the rpmbuild command that mock ran. Modify your spec, rebuild the
source rpm again, and then rebuild with mock once again. However, this time you
want to skip the initial setup of the chroot so be sure to add the --no-clean
flag to avoid cleaning the existing chroot and starting from scratch (it will be
much faster). 