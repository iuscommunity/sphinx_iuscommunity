:tocdepth: 2

.. _Fedora EPEL: https://fedoraproject.org/wiki/EPEL
.. _IUS Community Repo: http://dl.iuscommunity.org/pub/ius
.. _LaunchPad IUS Bug #453543: https://bugs.launchpad.net/ius/+bug/453543
.. _Yum Bug #296: http://yum.baseurl.org/ticket/296
.. _Red Hat Bug #529719: https://bugzilla.redhat.com/show_bug.cgi?id=529719


======================
IUS Client Usage Guide
======================

.. contents::
    :backlinks: none
    
Summary
=======

This document outlines the basic client side usage of IUS. For common questions
regarding IUS please see the :ref:`FAQs`.

End User Agreement
==================

 * http://dl.iuscommunity.org/pub/ius/IUS-COMMUNITY-EUA

.. _Information_on_IUS_Packages:

Information on IUS Packages
===========================

The following explains a little bit behind how IUS packages are organized. 

Package Categories
==================

The packaging model of IUS is broken up into two package categories:

Primary Packages:
-----------------

The packages represent our purpose in maintaining these repositories.
All primary packages will be maintained inline with upstream stable sources.
In short, these are the packages that we focus our time on most, and have the
highest priority. Almost always, with little exception, these packages
explicitly replace a package that already exists in RHEL. For example:
mysql50 and mysql51 both explicitly replace mysql as shipped with RHEL.

Secondary Supporting Packages:
------------------------------

Supporting packages are those that have been
added to enhance the usability and appeal of the Primary Packages.
These will most often be additional libraries such as PECL for PHP, or libraries
for Python. Supporting Packages might not replace anything in RHEL, but
sometimes do. In should be noted that IUS is not focused on adding extra
packages for RHEL. If we want to package something additional for stock
RHEL we will do so through `Fedora EPEL`_. Supporting Packages might not follow
upstream stable sources in the fashion that the Primary Packages do. If it's
stable, and works... we may not update these packages for a long time
(or until an update is requested).


Package Types
=============

IUS Packages will be one of two types: Conflicting or Parallel.

Conflict Replace Packages:
--------------------------

Almost all IUS packages will be conflicting. This means, the packages that they
replace must first be uninstalled before the IUS version is installed.
For example: mysql50 and mysql51 both conflict with mysql. They provide 'mysql'
but they do not obsolete mysql, therefore simply subscribing to the IUS Yum
Repositories will not upgrade anything automatically. If you attempt to install
mysql5X packages from IUS you will get Yum errors. That said, once you remove
the original packages (mysql, mysql-server, mysql-devel, etc) and install the
IUS counterparts everything will function the same.

Parallel Install Packages:
--------------------------

In some cases it makes sense to maintain Parallel Packages, meaning packages
that you can install side-by-side with their stock counter parts. A perfect
example of this would be Python, or in IUS python26. On RHEL and most other
linux distributions python is a critical piece that the system relies on.
Upgrading Python on RHEL can cause major problems including break the update
management system. Yes, there are ways around it... but they are all messy and
not recommended. By parallel installing Python 2.6 alongside the stock Python
2.4 the system remains intact and you can access both versions of Python.


Unfortunately this layout leads to confusion, as there is nothing to signify
which packages are conflicting/replacement packages and which can be installed
side-by-side. Maybe one day we'll figure out a better schema.

Configuration
=============

Client configuration can be automatically setup by installing the ius-release
package, which also requires the epel-release package. Both packages can be
found within the respective repo for your OS:

 * http://dl.iuscommunity.org/pub/ius/stable

The following is an example, please note you may need to adjust the URLs for
your system::

    root@linuxbox ~]# wget http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/x86_64/ius-release-1-2.ius.el5.noarch.rpm

    root@linuxbox ~]# wget http://dl.iuscommunity.org/pub/ius/stable/Redhat/5/x86_64/epel-release-1-1.ius.el5.noarch.rpm

    root@linuxbox ~]# rpm -Uvh ius-release*.rpm epel-release*.rpm

See What Packages Are Available
===============================

You can easily see what software is available to you by doing a 'yum search'
or by going to the `IUS Community Repo`_ directly. Keep in mind that packages will
be added to IUS regularly so check back often::

    [root@el5-i386 ~]# yum list | grep -w \.ius\.
             
    mod_python26-debuginfo.i386            3.3.1-10.ius.el5       ius-testing       
    mysql50.i386                           5.0.83-2.ius.el5       ius-testing       
    mysql50-bench.i386                     5.0.83-2.ius.el5       ius-testing       
    mysql50-debuginfo.i386                 5.0.83-2.ius.el5       ius-testing       
    mysql50-devel.i386                     5.0.83-2.ius.el5       ius-testing       
    mysql50-server.i386                    5.0.83-2.ius.el5       ius-testing       
    mysql51.i386                           5.1.36-2.ius.el5       ius-testing       
    mysql51-bench.i386                     5.1.36-2.ius.el5       ius-testing       
    mysql51-debuginfo.i386                 5.1.36-2.ius.el5       ius-testing       
    mysql51-devel.i386                     5.1.36-2.ius.el5       ius-testing       
    mysql51-plugins-archive.i386           5.1.36-2.ius.el5       ius-testing       
    mysql51-plugins-blackhole.i386         5.1.36-2.ius.el5       ius-testing       
    mysql51-plugins-example.i386           5.1.36-2.ius.el5       ius-testing       
    mysql51-plugins-federated.i386         5.1.36-2.ius.el5       ius-testing       
    mysql51-server.i386                    5.1.36-2.ius.el5       ius-testing       
    php52.i386                             5.2.10-1.2.ius.el5     ius-testing         
    php52-cli.i386                         5.2.10-1.2.ius.el5     ius-testing
    php52-common.i386                      5.2.10-1.2.ius.el5     ius-testing         
    php52-gd.i386                          5.2.10-1.2.ius.el5     ius-testing       
    php52-imap.i386                        5.2.10-1.2.ius.el5     ius-testing       
    php52-ldap.i386                        5.2.10-1.2.ius.el5     ius-testing       
    php52-mbstring.i386                    5.2.10-1.2.ius.el5     ius-testing       
    php52-mysql.i386                       5.2.10-1.2.ius.el5     ius-testing       
    php52-odbc.i386                        5.2.10-1.2.ius.el5     ius-testing       
    php52-pdo.i386                         5.2.10-1.2.ius.el5     ius-testing       
    php52-xml.i386                         5.2.10-1.2.ius.el5     ius-testing
    php52-bcmath.i386                      5.2.10-1.2.ius.el5     ius-testing       
    php52-dba.i386                         5.2.10-1.2.ius.el5     ius-testing       
    php52-debuginfo.i386                   5.2.10-1.2.ius.el5     ius-testing       
    php52-devel.i386                       5.2.10-1.2.ius.el5     ius-testing       
    php52-mcrypt.i386                      5.2.10-1.2.ius.el5     ius-testing       
    php52-mssql.i386                       5.2.10-1.2.ius.el5     ius-testing       
    php52-ncurses.i386                     5.2.10-1.2.ius.el5     ius-testing       
    php52-pgsql.i386                       5.2.10-1.2.ius.el5     ius-testing       
    php52-snmp.i386                        5.2.10-1.2.ius.el5     ius-testing       
    php52-soap.i386                        5.2.10-1.2.ius.el5     ius-testing       
    php52-tidy.i386                        5.2.10-1.2.ius.el5     ius-testing       
    php52-xmlrpc.i386                      5.2.10-1.2.ius.el5     ius-testing       
    php53.i386                             5.3.0-1.ius.el5        ius-testing       
    php53-bcmath.i386                      5.3.0-1.ius.el5        ius-testing       
    php53-cli.i386                         5.3.0-1.ius.el5        ius-testing       
    php53-common.i386                      5.3.0-1.ius.el5        ius-testing       
    php53-dba.i386                         5.3.0-1.ius.el5        ius-testing       
    php53-debuginfo.i386                   5.3.0-1.ius.el5        ius-testing       
    php53-devel.i386                       5.3.0-1.ius.el5        ius-testing       
    php53-gd.i386                          5.3.0-1.ius.el5        ius-testing       
    php53-imap.i386                        5.3.0-1.ius.el5        ius-testing       
    php53-ldap.i386                        5.3.0-1.ius.el5        ius-testing       
    php53-mbstring.i386                    5.3.0-1.ius.el5        ius-testing       
    php53-mcrypt.i386                      5.3.0-1.ius.el5        ius-testing       
    php53-mssql.i386                       5.3.0-1.ius.el5        ius-testing       
    php53-mysql.i386                       5.3.0-1.ius.el5        ius-testing       
    php53-odbc.i386                        5.3.0-1.ius.el5        ius-testing       
    php53-pdo.i386                         5.3.0-1.ius.el5        ius-testing       
    php53-pgsql.i386                       5.3.0-1.ius.el5        ius-testing       
    php53-snmp.i386                        5.3.0-1.ius.el5        ius-testing       
    php53-soap.i386                        5.3.0-1.ius.el5        ius-testing       
    php53-tidy.i386                        5.3.0-1.ius.el5        ius-testing       
    php53-xml.i386                         5.3.0-1.ius.el5        ius-testing       
    php53-xmlrpc.i386                      5.3.0-1.ius.el5        ius-testing    
    python26-debuginfo.i386                2.6-4.5.ius.el5        ius-testing
    python26-devel.i386                    2.6-4.5.ius.el5        ius-testing
    python26-libs.i386                     2.6-4.5.ius.el5        ius-testing
    python26-setuptools.noarch             0.6c9-1.1.ius.el5      ius-testing
    python26-test.i386                     2.6-4.5.ius.el5        ius-testing
    python26-tools.i386                    2.6-4.5.ius.el5        ius-testing

Upgrading Stock RHEL Packages to IUS Packages
=============================================

The IUS repository has a package called 'yum-plugin-replace'. This package is
*not* required by the 'ius-release' package, but can be installed via::

    $ sudo yum install yum-plugin-replace

The replace plugin was written specifically for IUS to assist in upgrading from
stock packages to IUS packageXY style packages.

If for some reason these processes and the yum-plugin-replace do not work
correctly, you can also try :ref:`UpgradingTheOldWay`.

Using 'php' as an example, we are going to show how to upgrade from stock RHEL
packages to the IUS counterparts::

    [root@linuxbox ~]# rpm -qa | grep php
    php-pear-1.4.9-6.el5
    php-common-5.1.6-27.el5
    php-cli-5.1.6-27.el5
    php-devel-5.1.6-27.el5
    php-5.1.6-27.el5
    
    [root@linuxbox ~]# yum replace php --replace-with php53
    Loaded plugins: replace
    Excluding Packages in global exclude list
    Finished
    Replacing packages takes time, please be patient...
    
    WARNING: Unable to resolve all providers: ['config(php-common)', 'dbase.so()(64bit)', 'php-dbase', 'php-mime_magic', 'php-pcntl']
    
    This may be normal depending on the package.  Continue? [y/N] y
    
    Removed:
      php.x86_64 0:5.1.6-27.el5        php-cli.x86_64 0:5.1.6-27.el5  php-common.x86_64 0:5.1.6-27.el5 
      php-devel.x86_64 0:5.1.6-27.el5  php-pear.noarch 1:1.4.9-6.el5 
    
    Installed:
      php53.x86_64 0:5.3.2-6.ius.el5                   php53-cli.x86_64 0:5.3.2-6.ius.el5              
      php53-common.x86_64 0:5.3.2-6.ius.el5            php53-devel.x86_64 0:5.3.2-6.ius.el5            
      php53-pear.noarch 1:1.8.1-4.ius.el5              php53-pspell.x86_64 0:5.3.2-6.ius.el5           
    
    Complete!

As you can see there is a WARNING that the 'replace' operation was unable to
resolve all providers. This means that the 'php53' package doesn't provide
everything that the 'php' packages did. This is normal, and should be expected
when upgrading major versions of software. At times this will also be because of
something missing in the newer packages. For example, dbase was removed from
php53 core ... however 'config(php-common)' should likely be added to the php53
packages and is simply just an rpm spec change that needs to happen. The
yum-plugin-replace is new, and therefore small issues like this will be resolved
in the near future as they are discovered.

You will notice that the 'replace' plugin determines all the required sub
packages that are required to resolve the deps provided by the stock versions
package set. Additionally, the plugin will attempt to install any external
packages that might need to be replaced as well. For example, the 'php-pear'
package is not part of the 'php' package set. Therefore, it needs to be replaced
by 'php53-pear' ... another example would be with any PECL sub packages that
might be installed (assuming the php53-pecl-xxxxxx package is available in IUS).

The following is the full output from the command::

    [root@linuxbox ~]# yum replace php --replace-with php53
    Loaded plugins: replace
    Excluding Packages in global exclude list
    Finished
    Replacing packages takes time, please be patient...
    
    WARNING: Unable to resolve all providers: ['config(php-common)', 'dbase.so()(64bit)', 'php-dbase', 'php-mime_magic', 'php-pcntl']
    
    This may be normal depending on the package.  Continue? [y/N] y
    Resolving Dependencies
    --> Running transaction check
    ---> Package php.x86_64 0:5.1.6-27.el5 set to be erased
    ---> Package php-cli.x86_64 0:5.1.6-27.el5 set to be erased
    ---> Package php-common.x86_64 0:5.1.6-27.el5 set to be erased
    ---> Package php-devel.x86_64 0:5.1.6-27.el5 set to be erased
    ---> Package php-pear.noarch 1:1.4.9-6.el5 set to be erased
    ---> Package php53.x86_64 0:5.3.2-6.ius.el5 set to be updated
    ---> Package php53-cli.x86_64 0:5.3.2-6.ius.el5 set to be updated
    ---> Package php53-common.x86_64 0:5.3.2-6.ius.el5 set to be updated
    ---> Package php53-devel.x86_64 0:5.3.2-6.ius.el5 set to be updated
    ---> Package php53-pear.noarch 1:1.8.1-4.ius.el5 set to be updated
    ---> Package php53-pspell.x86_64 0:5.3.2-6.ius.el5 set to be updated
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ====================================================================================================
     Package                 Arch              Version                       Repository            Size
    ====================================================================================================
    Installing:
     php53                   x86_64            5.3.2-6.ius.el5               ius                  2.0 M
     php53-cli               x86_64            5.3.2-6.ius.el5               ius                  3.1 M
     php53-common            x86_64            5.3.2-6.ius.el5               ius                  557 k
     php53-devel             x86_64            5.3.2-6.ius.el5               ius                  595 k
     php53-pear              noarch            1:1.8.1-4.ius.el5             ius                  420 k
     php53-pspell            x86_64            5.3.2-6.ius.el5               ius                   22 k
    Removing:
     php                     x86_64            5.1.6-27.el5                  installed            6.2 M
     php-cli                 x86_64            5.1.6-27.el5                  installed            5.3 M
     php-common              x86_64            5.1.6-27.el5                  installed            397 k
     php-devel               x86_64            5.1.6-27.el5                  installed            2.5 M
     php-pear                noarch            1:1.4.9-6.el5                 installed            1.8 M
    
    Transaction Summary
    ====================================================================================================
    Install       6 Package(s)
    Upgrade       0 Package(s)
    Remove        5 Package(s)
    Reinstall     0 Package(s)
    Downgrade     0 Package(s)
    
    Total download size: 6.6 M
    Is this ok [y/N]: y
    Downloading Packages:
    (1/6): php53-pspell-5.3.2-6.ius.el5.x86_64.rpm                               |  22 kB     00:00     
    (2/6): php53-pear-1.8.1-4.ius.el5.noarch.rpm                                 | 420 kB     00:00     
    (3/6): php53-common-5.3.2-6.ius.el5.x86_64.rpm                               | 557 kB     00:00     
    (4/6): php53-devel-5.3.2-6.ius.el5.x86_64.rpm                                | 595 kB     00:00     
    (5/6): php53-5.3.2-6.ius.el5.x86_64.rpm                                      | 2.0 MB     00:00     
    (6/6): php53-cli-5.3.2-6.ius.el5.x86_64.rpm                                  | 3.1 MB     00:00     
    ----------------------------------------------------------------------------------------------------
    Total                                                                11 MB/s | 6.6 MB     00:00     
    Running rpm_check_debug
    Running Transaction Test
    Finished Transaction Test
    Transaction Test Succeeded
    Running Transaction
      Installing     : php53-cli                                                                   1/11 
      Installing     : php53-common                                                                2/11 
      Installing     : php53                                                                       3/11 
      Installing     : php53-devel                                                                 4/11 
      Installing     : php53-pspell                                                                5/11 
      Installing     : php53-pear                                                                  6/11 
      Erasing        : php-common                                                                  7/11 
      Erasing        : php-cli                                                                     8/11 
      Erasing        : php                                                                         9/11 
      Erasing        : php-devel                                                                  10/11 
      Erasing        : php-pear                                                                   11/11 
    
    Removed:
      php.x86_64 0:5.1.6-27.el5        php-cli.x86_64 0:5.1.6-27.el5  php-common.x86_64 0:5.1.6-27.el5 
      php-devel.x86_64 0:5.1.6-27.el5  php-pear.noarch 1:1.4.9-6.el5 
    
    Installed:
      php53.x86_64 0:5.3.2-6.ius.el5                   php53-cli.x86_64 0:5.3.2-6.ius.el5              
      php53-common.x86_64 0:5.3.2-6.ius.el5            php53-devel.x86_64 0:5.3.2-6.ius.el5            
      php53-pear.noarch 1:1.8.1-4.ius.el5              php53-pspell.x86_64 0:5.3.2-6.ius.el5           
    
    Complete!

And now, you should have a working install of PHP 5.3 on RHEL5::

    [root@linuxbox ~]# php -v
    PHP 5.3.2 (cli) (built: Jun 24 2010 17:22:02) 
    Copyright (c) 1997-2010 The PHP Group
    Zend Engine v2.3.0, Copyright (c) 1998-2010 Zend Technologies
    
But don't forget to check and restart Apache::

    [root@el5-i386 ~]# httpd -t
    Syntax OK
    
    [root@el5-i386 ~]# /etc/init.d/httpd restart
    Stopping httpd:                                            [  OK  ]
    Starting httpd:
    
As the plugin suggest one piece of software is being replaced by another, for
example you can not replace mysql with mysql55 if mysql is not initially
installed::

    # yum replace mysql --replace-with mysql55
    Loaded plugins: fastestmirror, replace
    Loading mirror speeds from cached hostfile
     * base: centos-distro.cavecreek.net
     * epel: fedora-epel.mirror.lstn.net
     * extras: centos.mirror.lstn.net
     * ius: pancks.sothatswhy.org.uk
     * updates: mirror.raystedman.net
    Replacing packages takes time, please be patient...
    Error: Package 'mysql' is not installed.
    
One of the main reasons you may run in to this is with Enterprise Linux 6.

Enterprise Linux 6 comes pre installed with mysql-libs as it is required by
Postfix, but does not come with mysql. The simplest solution in these cases
would be to first install mysql from base Redhat::

    # yum install mysql
    Loaded plugins: fastestmirror, replace
    Loading mirror speeds from cached hostfile
     * base: centos-distro.cavecreek.net
     * epel: fedora-epel.mirror.lstn.net
     * extras: centos.mirror.lstn.net
     * ius: pancks.sothatswhy.org.uk
     * updates: mirror.raystedman.net
    Setting up Install Process
    Resolving Dependencies
    --> Running transaction check
    ---> Package mysql.i686 0:5.1.52-1.el6_0.1 set to be updated
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ====================================================================================================
     Package                 Arch            Version                      Repository            Size
    ====================================================================================================
    Installing:
     mysql                   i686             5.1.52-1.el6_0.1             updates               898 k
    
    Transaction Summary
    ====================================================================================================
    Install       1 Package(s)
    Upgrade       0 Package(s)
    
    Total download size: 898 k
    Installed size: 2.3 M
    Is this ok [y/N]: y
    Downloading Packages:
    mysql-5.1.52-1.el6_0.1.i686.rpm                                                    | 898 kB     00:06     
    Running rpm_check_debug
    Running Transaction Test
    Transaction Test Succeeded
    Running Transaction
    Warning: RPMDB altered outside of yum.
      Installing     : mysql-5.1.52-1.el6_0.1.i686                                      1/1 
    
    Installed:
      mysql.i686 0:5.1.52-1.el6_0.1                                                                                                                                                   
    
    Complete!

Then replace with mysql55 from IUS::

    # yum replace mysql --replace-with mysql55
    Loaded plugins: fastestmirror, replace
    Loading mirror speeds from cached hostfile
     * base: centos-distro.cavecreek.net
     * epel: mirror.utexas.edu
     * extras: centos.mirror.lstn.net
     * ius: pancks.sothatswhy.org.uk
     * updates: mirror.raystedman.net
    Replacing packages takes time, please be patient...
    
    WARNING: Unable to resolve all providers: ['config(mysql-libs)', 'libmysqlclient.so.16', 'libmysqlclient.so.16(libmysqlclient_16)',
    'libmysqlclient_r.so.16', 'libmysqlclient_r.so.16(libmysqlclient_16)', 'mysql-libs(x86-32)', 'mysql(x86-32)']
    
    This may be normal depending on the package.  Continue? [y/N] y
    Resolving Dependencies
    --> Running transaction check
    ---> Package mysql.i686 0:5.1.52-1.el6_0.1 set to be erased
    ---> Package mysql-libs.i686 0:5.1.52-1.el6_0.1 set to be erased
    --> Processing Dependency: libmysqlclient.so.16 for package: 2:postfix-2.6.6-2.el6.i686
    --> Processing Dependency: libmysqlclient.so.16 for package: perl-DBD-MySQL-4.013-3.el6.i686
    --> Processing Dependency: libmysqlclient.so.16(libmysqlclient_16) for package: 2:postfix-2.6.6-2.el6.i686
    --> Processing Dependency: libmysqlclient.so.16(libmysqlclient_16) for package: perl-DBD-MySQL-4.013-3.el6.i686
    ---> Package mysql55.i686 0:5.5.15-2.ius.el6 set to be updated
    --> Processing Dependency: mysqlclient16 for package: mysql55-5.5.15-2.ius.el6.i686
    ---> Package mysql55-libs.i686 0:5.5.15-2.ius.el6 set to be updated
    --> Running transaction check
    ---> Package mysqlclient16.i686 0:5.1.56-1.ius.el6 set to be updated
    ---> Package perl-DBD-MySQL.i686 0:4.013-3.el6 set to be erased
    ---> Package postfix.i686 2:2.6.6-2.el6 set to be erased
    --> Processing Dependency: /usr/sbin/sendmail for package: cronie-1.4.4-2.el6.i686
    --> Running transaction check
    ---> Package cronie.i686 0:1.4.4-2.el6 set to be erased
    --> Processing Dependency: cronie = 1.4.4-2.el6 for package: cronie-anacron-1.4.4-2.el6.i686
    --> Running transaction check
    ---> Package cronie-anacron.i686 0:1.4.4-2.el6 set to be erased
    --> Processing Dependency: /etc/cron.d for package: crontabs-1.10-32.1.el6.noarch
    --> Restarting Dependency Resolution with new changes.
    --> Running transaction check
    ---> Package crontabs.noarch 0:1.10-32.1.el6 set to be erased
    --> Finished Dependency Resolution
    --> Running transaction check
    ---> Package cronie.i686 0:1.4.4-2.el6 set to be erased
    ---> Package cronie-anacron.i686 0:1.4.4-2.el6 set to be erased
    ---> Package crontabs.noarch 0:1.10-32.1.el6 set to be erased
    ---> Package perl-DBD-MySQL.i686 0:4.013-3.el6 set to be erased
    ---> Package postfix.i686 2:2.6.6-2.el6 set to be erased
    --> Finished Dependency Resolution
    
    Dependencies Resolved
    
    ====================================================================================================
     Package                 Arch            Version                      Repository            Size
    ====================================================================================================
    Installing:
     mysql55                 i686             5.5.15-2.ius.el6             ius                  5.8 M
     mysql55-libs            i686             5.5.15-2.ius.el6             ius                  773 k
    Removing:
     mysql                   i686             5.1.52-1.el6_0.1             @updates             2.3 M
     mysql-libs              i686             5.1.52-1.el6_0.1             @updates             3.9 M
    Installing for dependencies:
     mysqlclient16           i686             5.1.56-1.ius.el6             ius                  4.0 M
    
    Transaction Summary
    ====================================================================================================
    Install       3 Package(s)
    Upgrade       0 Package(s)
    Remove        2 Package(s)
    Reinstall     0 Package(s)
    Downgrade     0 Package(s)
    
    Total download size: 11 M
    Is this ok [y/N]: y
    Downloading Packages:
    (1/3): mysql55-5.5.15-2.ius.el6.i686.rpm                                                          | 5.8 MB     00:02     
    (2/3): mysql55-libs-5.5.15-2.ius.el6.i686.rpm                                                     | 773 kB     00:00     
    (3/3): mysqlclient16-5.1.56-1.ius.el6.i686.rpm                                                    | 4.0 MB     00:01     
    ------------------------------------------------------------------------------------------------------
    Total                                                                                             2.7 MB/s |  11 MB     00:03     
    Running rpm_check_debug
    Running Transaction Test
    Transaction Test Succeeded
    Running Transaction
      Installing     : mysql55-libs-5.5.15-2.ius.el6.i686                                             1/5 
      Installing     : mysqlclient16-5.1.56-1.ius.el6.i686                                            2/5 
      Installing     : mysql55-5.5.15-2.ius.el6.i686                                                  3/5 
      Erasing        : mysql-5.1.52-1.el6_0.1.i686                                                    4/5 
      Erasing        : mysql-libs-5.1.52-1.el6_0.1.i686                                               5/5 
    
    Removed:
      mysql.i686 0:5.1.52-1.el6_0.1                                                         mysql-libs.i686 0:5.1.52-1.el6_0.1                                                        
    
    Installed:
      mysql55.i686 0:5.5.15-2.ius.el6                                                       mysql55-libs.i686 0:5.5.15-2.ius.el6                                                      
    
    Dependency Installed:
      mysqlclient16.i686 0:5.1.56-1.ius.el6                                                                                                                                           
    
    Complete!
    
Downgrading from IUS Packages to Stock RHEL Packages
====================================================

Please note that downgrading using the yum 'replace' plugin is slightly
experimental, and may not work for all package sets. If you have issues,
please use the old way.

Downgrading is really the same process but backwards. The 'replace' plugin for
yum also works for downgrading (but will produce many more missing providers)::

    [root@linuxbox ~]# yum replace php53 --replace-with php
    Loaded plugins: replace
    Excluding Packages in global exclude list
    Finished
    Replacing packages takes time, please be patient...
    
    WARNING: Unable to resolve all providers: ['php53-cgi', 'php53-pcntl', 'php53-readline', 'php53-cli', 'config(php53-common)', 'curl.so()(64bit)', 'fileinfo.so()(64bit)', 'json.so()(64bit)', 'phar.so()(64bit)', 'php(api)', 'php(zend-abi)', 'php-json', 'php-pecl(Fileinfo)', 'php-pecl(json)', 'php-pecl(phar)', 'php-pecl(zip)', 'php-pecl-Fileinfo', 'php-pecl-json', 'php-pecl-phar', 'php-pecl-zip', 'php-zip', 'php53(api)', 'php53(zend-abi)', 'php53-api', 'php53-bz2', 'php53-calendar', 'php53-ctype', 'php53-curl', 'php53-date', 'php53-exif', 'php53-ftp', 'php53-gettext', 'php53-gmp', 'php53-hash', 'php53-iconv', 'php53-json', 'php53-libxml', 'php53-openssl', 'php53-pcre', 'php53-pecl(Fileinfo)', 'php53-pecl(json)', 'php53-pecl(phar)', 'php53-pecl(zip)', 'php53-pecl-Fileinfo', 'php53-pecl-json', 'php53-pecl-phar', 'php53-pecl-zip', 'php53-posix', 'php53-reflection', 'php53-session', 'php53-shmop', 'php53-simplexml', 'php53-sockets', 'php53-spl', 'php53-sysvmsg', 'php53-sysvsem', 'php53-sysvshm', 'php53-tokenizer', 'php53-wddx', 'php53-zend-abi', 'php53-zip', 'php53-zlib', 'zip.so()(64bit)', 'php53-common', 'config(php53-devel)', 'php53-devel', 'config(php53-pspell)', 'pspell.so()(64bit)', 'php53-pspell']
    
    This may be normal depending on the package.  Continue? [y/N] y
    
    
    Removed:
      php53.x86_64 0:5.3.2-6.ius.el5                   php53-cli.x86_64 0:5.3.2-6.ius.el5              
      php53-common.x86_64 0:5.3.2-6.ius.el5            php53-devel.x86_64 0:5.3.2-6.ius.el5            
      php53-pear.noarch 1:1.8.1-4.ius.el5              php53-pspell.x86_64 0:5.3.2-6.ius.el5           
    
    Installed:
      php.x86_64 0:5.1.6-27.el5        php-cli.x86_64 0:5.1.6-27.el5  php-common.x86_64 0:5.1.6-27.el5 
      php-devel.x86_64 0:5.1.6-27.el5  php-pear.noarch 1:1.4.9-6.el5 
    
    Complete!
    
And of course we once again have stock PHP for EL5::

    [root@el5-i386 ~]# php -v
    PHP 5.1.6 (cli) (built: Feb 26 2009 07:01:10) 
    Copyright (c) 1997-2006 The PHP Group
    Zend Engine v2.1.0, Copyright (c) 1998-2006 Zend Technologies
    
Known Yum Dependency Resolution Issues
======================================

The IUS CoreDev Team is aware of an issue with the previous versions of Yum and
how it resolves dependencies when installing packages. For background on this
matter please see the upstream bug reports that we have submitted:

 * `LaunchPad IUS Bug #453543`_
 * `Yum Bug #296`_
 * `Red Hat Bug #529719`_

As of Yum 3.2.26 (backported: 3.2.22-23) this is no longer a problem.

We had previously implemented an optional and temporary workaround by
backporting the original patch that we submitted to a yum3 package in the IUS
EL 5 repositories. If you had used this yum3 package, please revert to the
stock version of yum in RHEL 5.5/6.0::

    # yum install yum-utils
    
    # yumdownloader yum
    
    # rpm -e --nodeps yum3
    
    # rpm -Uvh yum-*.rpm

Common Examples for Red Hat Enterprise Linux 4
==============================================

It is possible that you still have a RHEL4 box. Yes, we know... it is sad.
But don't worry, we still have a few packages available for you. 

Configuring EPEL and IUS on RHEL/CentOS 4
=========================================
::

    [root@esx02-bjd-el4-64 ~]# wget http://dl.iuscommunity.org/pub/ius/stable/Redhat/4/i386/epel-release-1-1.ius.el4.noarch.rpm
    
    [root@esx02-bjd-el4-64 ~]# wget http://dl.iuscommunity.org/pub/ius/stable/Redhat/4/i386/ius-release-1-2.ius.el4.noarch.rpm
    
    [root@esx02-bjd-el4-64 ~]# rpm -Uvh ius-release*.rpm epel-release*.rpm
    
    [root@esx02-bjd-el4-64 ~]# rpm --import /etc/pki/rpm-gpg/IUS-COMMUNITY-GPG-KEY 
    
    [root@esx02-bjd-el4-64 ~]# rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL
    
Upgrade Stock MySQL 4.1 to IUS MySQL 5.0 on RHEL/CentOS 4
=========================================================

This is quick and dirty, but so is RHEL 4... so deal::

    # backup your data
    
    [root@esx02-bjd-el4-64 ~]# mysqldump -A > all_databases.sql
    
    
    
    # determine which packages to remove/replace
    
    [root@esx02-bjd-el4-64 ~]# rpm -qa | grep mysql
    mysqlclient10-3.23.58-4.RHEL4.1
    mysql-server-4.1.22-2.el4
    php-mysql-4.3.9-3.26
    libdbi-dbd-mysql-0.6.5-10.RHEL4.1
    mysql-devel-4.1.22-2.el4
    mysql-4.1.22-2.el4
    
    
    
    # perform the upgrade
    
    [root@esx02-bjd-el4-64 ~]# /etc/init.d/mysqld stop
    Stopping MySQL:                                            [  OK  ]
    
    [root@esx02-bjd-el4-64 ~]# rpm -e mysql mysql-server mysql-devel --nodeps
    warning: /var/log/mysqld.log saved as /var/log/mysqld.log.rpmsave
    warning: /etc/my.cnf saved as /etc/my.cnf.rpmsave
    
    [root@esx02-bjd-el4-64 ~]# /etc/init.d/mysqld start
    Starting MySQL:                                            [  OK  ]
    
    [root@esx02-bjd-el4-64 ~]# mysql_upgrade -t /tmp
    
    
    # basque in the glory of a successful upgrade
    
    [root@esx02-bjd-el4-64 ~]# mysql -V
    mysql  Ver 14.12 Distrib 5.0.85, for redhat-linux-gnu (x86_64) using readline 5.1
    
Upgrading Stock PHP 4.3 to IUS PHP 5.2 on RHEL/CentOS 4
=======================================================

Again, quick and dirty::

    # determine which packages to remove/replace
    
    [root@esx02-bjd-el4-64 ~]# rpm -qa | grep php
    php-mbstring-4.3.9-3.26
    php-gd-4.3.9-3.26
    php-ldap-4.3.9-3.26
    php-mysql-4.3.9-3.26
    php-pgsql-4.3.9-3.26
    php-pear-4.3.9-3.26
    php-imap-4.3.9-3.26
    php-odbc-4.3.9-3.26
    php-4.3.9-3.26
    
    
    # do the upgrade
    
    [root@esx02-bjd-el4-64 ~]# rpm -e php-mbstring php-gd php-ldap php-mysql php-pgsql php-imap php-odbc php --nodeps
    warning: /etc/php.ini saved as /etc/php.ini.rpmsave
    
    [root@esx02-bjd-el4-64 ~]# up2date -u php52-mbstring php52-gd php52-ldap php52-mysql php52-pgsql php52-imap php52-odbc php52 
    
    [root@esx02-bjd-el4-64 ~]# php -v
    PHP 5.2.11 (cli) (built: Oct  1 2009 19:19:44) 
    Copyright (c) 1997-2009 The PHP Group
    Zend Engine v2.2.0, Copyright (c) 1998-2009 Zend Technologies
    
    [root@esx02-bjd-el4-64 ~]# httpd -t
    Syntax OK
    
    [root@esx02-bjd-el4-64 ~]# /etc/init.d/httpd restart
    Stopping httpd:                                            [  OK  ]
    Starting httpd:                                            [  OK  ]
