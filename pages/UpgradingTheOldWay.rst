:tocdepth: 2

.. _ius.io: https://ius.io
.. _sphinx_iuscommunity Github repo: https://github.com/iuscommunity/sphinx_iuscommunity

.. _UpgradingTheOldWay:

=====================
Upgrading the Old Way
=====================

.. note:: As of September 24, 2015, `ius.io`_ has replaced this website.  This
          site will stay up for about 30 days for archive purposes then redirect to
          the new site.  The old site code will remain available at the
          `sphinx_iuscommunity Github repo`_.

The following outlines the old way to upgrade/downgrade IUS packages. These
processes still work, but have been replaced by the usage of the
'yum-replace-plugin': 

.. contents::
    :backlinks: none
    
Upgrading Stock RHEL Packages to IUS Packages
=============================================

Admittedly, upgrading from stock RHEL packages to IUS is not exactly an easy
transition, but trust us it's worth it in the long run. Because of the way IUS
packages are created, you can not simply do a 'yum upgrade <package>' because
the IUS packages actually conflict with the stock RHEL packages. What you really
need to do is first remove the stock RHEL packages, and then install the
corresponding IUS package you want. Lets use php on my stock RHEL5 box and first
see what packages we need to remove::

    [root@el5-i386 ~]# rpm -qa | grep php
    php-gd-5.1.6-23.2.el5_3
    php-cli-5.1.6-23.2.el5_3
    php-odbc-5.1.6-23.2.el5_3
    php-mbstring-5.1.6-23.2.el5_3
    php-pear-1.4.9-4.el5.1
    php-pdo-5.1.6-23.2.el5_3
    php-5.1.6-23.2.el5_3
    php-xml-5.1.6-23.2.el5_3
    php-common-5.1.6-23.2.el5_3
    php-ldap-5.1.6-23.2.el5_3
    php-mysql-5.1.6-23.2.el5_3
    php-imap-5.1.6-23.2.el5_3

*Note: php-pear is not part of the php package set. It is it's own package.*


Looking at this list I can see that I want to remove everything but 'php-pear'.
You can do this by 'rpm -e --nodeps...' but a cleaner, safer, more appropriate
way would be to use yum shell:

**Short version with no output**::

    [root@el5-i386 ~]# yum shell
    
    > remove php-gd php-cli php-odbc php-mbstring php-pdo php php-xml php-common php-ldap php-mysql php-imap
    Setting up Remove Process
    
    > install php52-gd php52-cli php52-odbc php52-mbstring php52-pdo php52 php52-xml php52-common php52-ldap php52-mysql php52-imap
    
    > transaction solve
    
    > transaction run

*NOTE: It is *very* important that you run 'transaction solve' to ensure all
dependencies are being met with the new packages. Removing packages with unmet
dependencies in Yum can lead to a nightmare of packages being removed.*

**Long version with output**::

    [root@el5-i386 ~]# yum shell
    
    Loaded plugins: downloadonly, rhnplugin, security
    Setting up Yum Shell
    
    > remove php-gd php-cli php-odbc php-mbstring php-pdo php php-xml php-common php-ldap php-mysql php-imap
    Setting up Remove Process
    
    > install php52-gd php52-cli php52-odbc php52-mbstring php52-pdo php52 php52-xml php52-common php52-ldap php52-mysql php52-imap
    
    Excluding Packages in global exclude list
    Finished
    Setting up Install Process
    Parsing package install arguments
    > transaction solve
    --> Running transaction check
    ---> Package php52-ldap.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-common.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-xml.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php-cli.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php52.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php-mbstring.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php52-mysql.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-cli.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php-imap.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php-mysql.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php-odbc.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php52-gd.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-pdo.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-imap.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-odbc.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php52-mbstring.x86_64 0:5.2.11-1.ius.el5 set to be updated
    ---> Package php-gd.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php-pdo.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php-common.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php-xml.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    ---> Package php-ldap.x86_64 0:5.1.6-23.2.el5_3 set to be erased
    --> Finished Dependency Resolution
    Success resolving dependencies
    
    > transaction run
    
    --> Running transaction check
    --> Finished Dependency Resolution
    
    ====================================================================================================
     Package                   Arch              Version                     Repository            Size
    ====================================================================================================
    Installing:
     php52                     x86_64            5.2.11-1.ius.el5            ius                  1.4 M
     php52-cli                 x86_64            5.2.11-1.ius.el5            ius                  2.6 M
     php52-common              x86_64            5.2.11-1.ius.el5            ius                  246 k
     php52-gd                  x86_64            5.2.11-1.ius.el5            ius                  124 k
     php52-imap                x86_64            5.2.11-1.ius.el5            ius                   60 k
     php52-ldap                x86_64            5.2.11-1.ius.el5            ius                   41 k
     php52-mbstring            x86_64            5.2.11-1.ius.el5            ius                  1.1 M
     php52-mysql               x86_64            5.2.11-1.ius.el5            ius                   94 k
     php52-odbc                x86_64            5.2.11-1.ius.el5            ius                   59 k
     php52-pdo                 x86_64            5.2.11-1.ius.el5            ius                   73 k
     php52-xml                 x86_64            5.2.11-1.ius.el5            ius                  113 k
    Removing:
     php                       x86_64            5.1.6-23.2.el5_3            installed            3.0 M
     php-cli                   x86_64            5.1.6-23.2.el5_3            installed            5.3 M
     php-common                x86_64            5.1.6-23.2.el5_3            installed            397 k
     php-gd                    x86_64            5.1.6-23.2.el5_3            installed            333 k
     php-imap                  x86_64            5.1.6-23.2.el5_3            installed             98 k
     php-ldap                  x86_64            5.1.6-23.2.el5_3            installed             49 k
     php-mbstring              x86_64            5.1.6-23.2.el5_3            installed            1.8 M
     php-mysql                 x86_64            5.1.6-23.2.el5_3            installed            196 k
     php-odbc                  x86_64            5.1.6-23.2.el5_3            installed             88 k
     php-pdo                   x86_64            5.1.6-23.2.el5_3            installed            114 k
     php-xml                   x86_64            5.1.6-23.2.el5_3            installed            241 k
    
    Transaction Summary
    ====================================================================================================
    Install     11 Package(s)         
    Update       0 Package(s)         
    Remove      11 Package(s)         
    
    Total download size: 5.9 M
    Is this ok [y/N]: y
    
    Downloading Packages:
    (1/11): php52-ldap-5.2.11-1.ius.el5.x86_64.rpm                               |  41 kB     00:00     
    (2/11): php52-odbc-5.2.11-1.ius.el5.x86_64.rpm                               |  59 kB     00:00     
    (3/11): php52-imap-5.2.11-1.ius.el5.x86_64.rpm                               |  60 kB     00:00     
    (4/11): php52-pdo-5.2.11-1.ius.el5.x86_64.rpm                                |  73 kB     00:00     
    (5/11): php52-mysql-5.2.11-1.ius.el5.x86_64.rpm                              |  94 kB     00:00     
    (6/11): php52-xml-5.2.11-1.ius.el5.x86_64.rpm                                | 113 kB     00:00     
    (7/11): php52-gd-5.2.11-1.ius.el5.x86_64.rpm                                 | 124 kB     00:00     
    (8/11): php52-common-5.2.11-1.ius.el5.x86_64.rpm                             | 246 kB     00:00     
    (9/11): php52-mbstring-5.2.11-1.ius.el5.x86_64.rpm                           | 1.1 MB     00:00     
    (10/11): php52-5.2.11-1.ius.el5.x86_64.rpm                                   | 1.4 MB     00:00     
    (11/11): php52-cli-5.2.11-1.ius.el5.x86_64.rpm                               | 2.6 MB     00:00     
    ----------------------------------------------------------------------------------------------------
    Total                                                                49 kB/s | 5.9 MB     02:02     
    warning: rpmts_HdrFromFdno: Header V3 DSA signature: NOKEY, key ID 9cd4953f
    Importing GPG key 0x9CD4953F "IUS Community Project <coredev@iuscommunity.org>" from /etc/pki/rpm-gpg/IUS-COMMUNITY-GPG-KEY
    Is this ok [y/N]: y
    Running rpm_check_debug
    Running Transaction Test
    Finished Transaction Test
    Transaction Test Succeeded
    Running Transaction
      Installing     : php52-common                                    [ 1/22] 
      Installing     : php52-pdo                                       [ 2/22] 
      Installing     : php52-odbc                                      [ 3/22] 
      Installing     : php52-mysql                                     [ 4/22] 
      Installing     : php52                                           [ 5/22] 
      Installing     : php52-mbstring                                  [ 6/22] 
      Installing     : php52-ldap                                      [ 7/22] 
      Installing     : php52-cli                                       [ 8/22] 
      Installing     : php52-xml                                       [ 9/22] 
      Installing     : php52-imap                                      [10/22] 
      Installing     : php52-gd                                        [11/22] 
      Erasing        : php                                             [12/22] 
      Erasing        : php-gd                                          [13/22] 
      Erasing        : php-xml                                         [14/22] 
      Erasing        : php-cli                                         [15/22] 
      Erasing        : php-common                                      [16/22] 
      Erasing        : php-pdo                                         [17/22] 
      Erasing        : php-mysql                                       [18/22] 
      Erasing        : php-odbc                                        [19/22] 
      Erasing        : php-imap                                        [20/22] 
      Erasing        : php-mbstring                                    [21/22] 
      Erasing        : php-ldap                                        [22/22] 
    
    Removed: php.x86_64 0:5.1.6-23.2.el5_3 php-cli.x86_64 0:5.1.6-23.2.el5_3 php-common.x86_64 0:5.1.6-23.2.el5_3 php-gd.x86_64
    0:5.1.6-23.2.el5_3 php-imap.x86_64 0:5.1.6-23.2.el5_3 php-ldap.x86_64 0:5.1.6-23.2.el5_3 php-mbstring.x86_64 0:5.1.6-23.2.el5_3
    php-mysql.x86_64 0:5.1.6-23.2.el5_3 php-odbc.x86_64 0:5.1.6-23.2.el5_3 php-pdo.x86_64 0:5.1.6-23.2.el5_3 php-xml.x86_64 0:5.1.6-23.2.el5_3
    Installed: php52.x86_64 0:5.2.11-1.ius.el5 php52-cli.x86_64 0:5.2.11-1.ius.el5 php52-common.x86_64 0:5.2.11-1.ius.el5 php52-gd.x86_64
    0:5.2.11-1.ius.el5 php52-imap.x86_64 0:5.2.11-1.ius.el5 php52-ldap.x86_64 0:5.2.11-1.ius.el5 php52-mbstring.x86_64 0:5.2.11-1.ius.el5
    php52-mysql.x86_64 0:5.2.11-1.ius.el5 php52-odbc.x86_64 0:5.2.11-1.ius.el5 php52-pdo.x86_64 0:5.2.11-1.ius.el5 php52-xml.x86_64 0:5.2.11-1.ius.el5
    Finished Transaction
    > Leaving Shell

And now, you should have a working install of PHP 5.2 on RHEL5::

    [root@el5-i386 ~]# php -v
    PHP 5.2.10 (cli) (built: Jul 28 2009 14:54:01) 
    Copyright (c) 1997-2009 The PHP Group
    Zend Engine v2.2.0, Copyright (c) 1998-2009 Zend Technologies

But don't forget to check and restart Apache::
    
    [root@el5-i386 ~]# httpd -t
    Syntax OK
    
    [root@el5-i386 ~]# /etc/init.d/httpd restart
    Stopping httpd:                                            [  OK  ]
    Starting httpd:                                            [  OK  ]


Downgrading from IUS Packages to Stock RHEL Packages
====================================================

Downgrading is really the same process but backwards. You first want to
determine which packages you want/need to remove::

    [root@el5-i386 ~]# rpm -qa | grep php52
    php52-common-5.2.10-1.2.ius.el5
    php52-mbstring-5.2.10-1.2.ius.el5
    php52-xml-5.2.10-1.2.ius.el5
    php52-cli-5.2.10-1.2.ius.el5
    php52-mysql-5.2.10-1.2.ius.el5
    php52-ldap-5.2.10-1.2.ius.el5
    php52-pdo-5.2.10-1.2.ius.el5
    php52-odbc-5.2.10-1.2.ius.el5
    php52-5.2.10-1.2.ius.el5
    php52-gd-5.2.10-1.2.ius.el5
    php52-imap-5.2.10-1.2.ius.el5

You then need to remove, and replace with stock packages::

    [root@el5-i386 ~]# yum shell
    
    > remove php52-gd php52-cli php52-odbc php52-mbstring php52-pdo php52 php52-xml php52-common php52-ldap php52-mysql php52-imap
    
    > install php-gd php-cli php-odbc php-mbstring php-pdo php php-xml php-common php-ldap php-mysql php-imap
    Setting up Remove Process
    
    > transaction solve
    
    > transaction run


And of course we once again have stock PHP for EL5::

    [root@el5-i386 ~]# php -v
    PHP 5.1.6 (cli) (built: Feb 26 2009 07:01:10) 
    Copyright (c) 1997-2006 The PHP Group
    Zend Engine v2.1.0, Copyright (c) 1998-2006 Zend Technologies
