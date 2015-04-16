:tocdepth: 2

.. _answers section: http://answers.launchpad.net/ius
.. _testing repository: http://dl.iuscommunity.org/pub/ius/testing
.. _bug tracking system: http://bugs.launchpad.net/ius
.. _Fedora Koji: http://fedoraproject.org/wiki/Koji

.. _FAQs:

====
FAQs
====

The FAQs on this page are the most common, official questions we wanted to
answer. However, if you don’t find your question here, please check out our
`answers section`_. There you can view all questions that have been
answered, as well as ask your own.

.. contents::
    :backlinks: none

General
=======

What is the IUS Community Project?
----------------------------------

The IUS Community Project is an effort to package rpms of the latest stable
versions of the most commonly requested software on Red Hat Enterprise Linux
and CentOS. IUS provides a better way to upgrade PHP/MySQL/Python/Etc on RHEL
or CentOS. The project is run by professional Linux Engineers that are
primarily focused on RPM Development in the web hosting industry.

What is it NOT?
---------------

For one, IUS is not a service of Rackspace but rather is Sponsored by
Rackspace. Additionally, IUS is not the same as Fedora EPEL or similar repos.
EPEL is geared towards adding packages to RHEL, and has strict guidelines that
none of their packages replace anything in RHEL. IUS on the other hand is
explicitly geared towards providing packages that do replace existing software
in RHEL. Essentially, we are offering a proper way to upgrade software on RHEL
when you really need it the latest upstream versions of software.

What Does IUS Stand For?
------------------------

The acronym IUS stands for **Inline with Upstream Stable**. We also stand for
world peace, and kittens.

What is the History of the IUS Community Project
------------------------------------------------

See the :doc:`About` page.

Why can't I just do a 'yum upgrade' and update my RPMs?
-------------------------------------------------------

See :ref:`Why are IUS Packages named differently`.

.. _Why are IUS Packages named differently:

Why are IUS Packages named differently
--------------------------------------

There is great debate around the naming conventions used for IUS Packages. For
example the equivalent of 'mysql' in IUS is mysql50, or mysql51, etc. The
reason for this is simple, if the name of the package was the same as the stock
package then as soon as you subscribed to the IUS Repositories your system
would update nearly every package in the repo. This is what we call 'bad
ju-ju'. The majority of IUS users want to upgrade something specific like
mysql, php, etc.; not everything. Additionally, the naming convention also
allows us to offer multiple branches of software via the same repository as
well. Meaning, you subscribe once and are always one install away from
installing the latest that IUS has to offer.

The alternative is to maintain multiple repositories for every package set. So
rather than having all packages in one repository, you would have a repository
for php52, php53, mysql50, mysql51, python26, etc… not too mention all the
supporting packages that might not warrant their own repository like
php-pear18, or mysqlclient15.

Another major reason is for compatibility (or clarity) when mixing with other
repos. for example when subscribing a RHEL box to EPEL, which has a number of
php-pecl-XXX packages. All php-pecl packages in EPEL are built against stock
php-5.1.6. If I introduced another repository with php-5.2.x in it and the
packages were still named 'php' that would be quite confusing, not to mentioned
incompatible since the php-pecl packages in EPEL wouldn't work. Having an
alternative name makes it clear that php52 is not php.

Long story short, from a packaging stand point, we have decided that this is
the best way to maintain packages for IUS based on our package sets. Ease of
installation might feel a bit janky or cumbersome, but if you look at it from
the stance of long term use and supportability of IUS it really isn't that bad. 

Is IUS SafeRepo Aware?
----------------------

It sure is! For more info on what this means, see the :doc:`TheSafeRepoInitiative`.

Is IUS Right For Me?
====================

Ask yourself these questions:

Am I a Rackspace Customer?
--------------------------

IUS is kind of like the 'Fedora of Rackspace packages'. We've been offering
similar services to Rackspace Hosting customers since late 2006. Please ask
your AM or Support Team what packages and services are offered, rather than
using IUS in your production. That said, for systems outside of Rackspace or
for testing/dev environments IUS gives you a chance to try out the latest
versions of software before it makes its way to Rackspace production. 

Is there anyway to make my application work with stock RHEL packages?
---------------------------------------------------------------------

If it is possible, stick with stock Red Hat packages. Thoroughly consider why
you think you need upgraded versions that IUS provides and make a wise and
educated decision. 

Can I afford the risk?
----------------------

IUS might introduce bugs/stability issues because it is bleeding edge versions
of software. There is no way around that… so it is a give and take situation.
You get the latest and greatest… but you also get to be one of the first people
to find bugs before everyone else. 

Am I upgrading software because I failed a PCI Security Scan?
-------------------------------------------------------------

Before jumping the gun and upgrading your packages because some 'Security
Professional' or software told you too, don't. Atleast not without further
consideration. Red Hat uses a packaging model called 'back porting' where the
security fixes are back-ported to older versions of software. Therefore, the
PCI scan may fail but in fact the software is patched for the known
vulnerability.

Always check the errata for the package in question, ensure you are updated to
the latest, and verify if or if not that security vulnerability has been
patched. You can read more about how Red Hat back-ports security fixes here:

http://www.redhat.com/security/updates/backporting/?sc_cid=3093

Does Red Hat Support the Use of IUS Packages?
---------------------------------------------

Ha! No.

The idea behind IUS is to provide and maintain packages for RHEL (and other
distros) that are inline with the latest versions of upstream software. So, if
you upgrade say, php to our php52 packages and then try to obtain support from
Redhat regarding PHP you will likely not get it.

There are two main reasons that most people want to use IUS: The first being
that they absolutely need newer versions of software that RHEL does not
provide, and the other being for people that need to do testing of upstream
versions. Think of IUS as a better way to upgrade RHEL, if you really need to. 

Do You Recommend IUS Over Stock RHEL Packages
---------------------------------------------

No, not necessarily. We do not want to give the impression that our packages
are better than those found in RHEL. Red Hat has teams of developers working
tirelessly to ensure the stability and reliability of their packages. IUS does
not have the resources to even begin to compete with the stability and quality
of the official packages maintained by Red Hat. Plus, we are also following the
latest and greatest sources from upstream directly which inevitably opens up a
higher level of risk that those packages will introduce bugs.

IUS is recommended for users that absolutely need newer versions of specific
software than RHEL can provide. IUS is not intended for users that want to
update their entire OS. You don't want to use IUS packages just because it
'feels good' or is 'cool' to have newer software. Though, don't get me wrong,
IUS is pretty cool.

IUS solves the problem of: "How do I upgrade software on my RHEL machine, and
keep that software updated and maintained for bugs/security issues?" You can
look all over the internets and see dozens of 'how-to' articles where people
explain how they upgraded PHP/MySQL/etc. on their RHEL server. But how many do
you find that explain how they keep that version of PHP/MySQL/etc. updated and
patched? Not many, until now. 

Does IUS Cost Anything?
-----------------------

Nope, but we do ask that if you've found IUS useful that you drop us a line and
say hi or blog about it to get the word out for us. 

Development and Packaging Questions
===================================

How Can I Contribute to IUS
---------------------------

Currently the best way to contribute is by providing feedback of testing and
stable packages. Our main goal is to provide stable packages using the latest
upstream sources which can be difficult. By users reporting any issues/bugs
they come across right away, we can get those fixed and updated packages pushed
out asap. That said, please also see :doc:`ContributingToIUS` on the wiki. 

How Do I Install Packages From Testing?
---------------------------------------

When packages are tagged as 'testing' in our buildfarm they are added to the
`testing repository`_. If you have installed the ius-release package then
you already have this repo configured in yum, but disabled by default. Simply
issue the following command for the package(s) you want to install from
testing::

    [root@linuxbox ~]# yum install PACKAGE_NAME --enablerepo=ius-testing

How Can I Become a Packager?
----------------------------

Currently we are not opening up access for packagers outside of the IUS Core
Development Team, which is currently the Engineers at Rackspace that are
leading IUS. This is primarily due to the fact that our buildsystem is not
publicly available. It is something that we are going to open up later on once
IUS is fully vetted and all processes and documentation have been figured out,
and of course once we get a public build system setup. For the time being we
graciously request that you submit patches or SRPMS via the `bug tracking system`_.
See our :doc:`IUSDeveloperGuide` on the wiki for info on branching the Bazaar repos and
creating merge requests. 

What Packaging System Do You Use?
---------------------------------

We built a system called MonkeyFarm that functions much like `Fedora
Koji`_. We make no attempt to compete with Koji in that department, however
there were other needs for a 'build system' outside of RPM packaging that lead
us to continue work on our own. Additionally, the preceptor to MonkeyFarm was
RPE (The Rackspace Packaging Environment) which was started before Koji was
released publicly (and before we knew about it).
