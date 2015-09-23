:tocdepth: 2

.. _ius.io: https://ius.io
.. _sphinx_iuscommunity Github repo: https://github.com/iuscommunity/sphinx_iuscommunity
.. _Rackspace Hosting: http://www.rackspace.com
.. _End User Agreement: https://dl.iuscommunity.org/pub/ius/IUS-COMMUNITY-EUA
.. _mailing lists: http://launchpad.net/~ius-community
.. _bug tracking system: http://bugs.launchpad.net/ius
.. _IUS CoreDev Team: http://launchpad.net/~ius-coredev
.. _bug tracking system: http://bugs.launchpad.net/ius
.. _answers section: http://answers.launchpad.net/ius

=====
About
=====

.. note:: As of September 24, 2015, `ius.io`_ has replaced this website.  This
          site will stay up for about 30 days for archive purposes then redirect to
          the new site.  The old site code be still be available at the
          `sphinx_iuscommunity Github repo`_.

.. note:: The IUS Community Project is aimed at providing up to date and       
           regularly maintained RPM packages for the latest upstream versions of
           PHP, Python, MySQL and other common software specifically for Redhat 
           Enterprise Linux.  IUS can be thought of as a better way to upgrade  
           RHEL, when you need to.

.. contents::
    :backlinks: none

History
=======

The IUS Community Project is a brain child of the RPM Development Team at
`Rackspace Hosting`_. Since 2006, we have provided and maintain packages for
the latest versions of PHP/MySQL and other common software on Red Hat
Enterprise Linux, because a lot of our customers strongly demand it. Internally
we maintain a number of package sets for an audience of thousands of production
servers. Until now, these packages have only been available internally to
Rackspace customers. After a while we started thinking: Why not make this
available publicly for everyone to benefit?

Updates for existing packages, as well as new packages, will be made available
to the community first during a 'testing' stage. We give it some time to bake
and allow IUS users to test the packages out and submit bug reports if
necessary. After testing, those packages will join the IUS 'stable' repository.
For new packages, we require the requester to do some testing and give feedback
before we will push a package into the stable repository. For updates, we allow
two weeks for feedback before pushing package into the stable repository.
Updates that contain fixes for CVEs will get pushed into the stable repository
sooner. We want to make sure that the the community has given some time to test
the update, but also want to push the update in a timely fashion.

IUS Community is run by full time Linux Engineers employed by Rackspace,
therefore making the project 'Sponsored by Rackspace'. That said, **IUS is not
a service of Rackspace**. That means: It's free! (And unsupported!) Please read
the `End User Agreement`_. Support for IUS users can be found within our
`mailing lists`_ and `bug tracking system`_.

The IUS Community Project is also  SafeRepo Aware, see
:doc:`TheSafeRepoInitiative`.

Community Contribution
======================

With the large scale use of our packages across the world, we are able to
provide valuable feedback to the upstream vendors regarding the latest stable
versions of their software. Any bugs reported for IUS Community Packages will
be fixed by the `IUS CoreDev Team`_ where possible, and the report and
suggested fix will also be pushed upstream to the software vendor. Having
a large community that is constantly using the latest versions of software from
the vendor (without having to manually update it) gets real time feedback to
those vendors, helping them solidify their product more rapidly.

By using IUS Community Packages, you are constantly helping upstream vendors to
make their products better. All you have to do is be vocal and report any
issues you might have after a package update to the IUS Community Repositories.

IUS in Production
=================

IUS Community Packages offer the latest upstream stable versions of software
built for Red Hat Enterprise Linux. There is an inherent risk when using
bleeding edge software, and as such users must understand that they are
forfeiting the stability of Red Hat packages in favor of getting updated
software. That said, we attempt to fulfill the demand for servers that
absolutely need the latest versions of software. We like to think of IUS as "a
better way to upgrade RHEL, if you really need to."

IUS has a strict packaging model that enables users to subscribe to our repo,
but only receive new packages and updates if you explicitly install one of our
packages sets. Therefore, you can enjoy the stability of Red Hat Enterprise
Linux for the majority of the system but also provide your developers with the
latest PHP, Python, MySQL etc if that is what their applications demand.

Project Goals
=============

The IUS Community Project is new, ever growing and changing.  That said,
currently we have the following goals in mind:

 * Provide quality RPMs for Red Hat Enterprise Linux (and clones)
 * Establish ourselves as a reliable means of upgrading certain parts of 
   RHEL
 * Offer more than one major version of software per repo/distro.
 
IUS packages the most popular branches of upstream software. For example, we
currently maintain packages for PHP 5.5, MySQL 5.6, and Python 3.4, all
within the same repository. This approach allows end users to have more control
over the software on their server, without the fuss of maintaining multiple
repos.

Contacting the IUS Core Development Team
========================================

If you are unable to communicate via our `bug tracking system`_, `mailing
lists`_, or `answers section`_, please send us an email to
coredev@iuscommunity.org.

We also hangout in #iuscommunity on irc.freenode.net.
