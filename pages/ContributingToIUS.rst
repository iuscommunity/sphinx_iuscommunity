:tocdepth: 2

.. _IUS Community Team: http://launchpad.net/~ius-community
.. _bug tracking system: http://bugs.launchpad.net/ius                  
.. _IUS CoreDev Team: http://launchpad.net/~ius-coredev
.. _Blue Prints: http://blueprints.launchpad.net/ius
.. _Automated Testing with Jenkins CI: http://blueprints.launchpad.net/ius/+spec/automated-testing-with-hudson
.. _IUS Triage Team: http://launchpad.net/~ius-triage

===================
Contributing to IUS
===================

.. contents::
    :backlinks: none
    
Summary
=======

This article outlines how you can join the IUS Team(s) and get involved with
the project. There are many opportunities for all level of technical
backgrounds.

General Project Participation
=============================

The easiest, and most effective way to contribute to the IUS Community is to
simply participate in the project. All contributors should join the `IUS
Community Team`_ in LaunchPad [LP] as well as join the mailing list for the
team.  This is where end-users and community members are encouraged to
communicate.

The `IUS CoreDev Team`_ will frequently post 'blueprints' on LP when they have
an idea regarding projects or tasks that need to happen at some point.
Contributors can frequently review the `Blue Prints`_ page on LP to see if
there are any projects or tasks that they can square away.

The `IUS CoreDev Team`_ hangs out in #iuscommunity on freenode.net (IRC). Its
always tooo quiet in there. Dropping in to say hi, or to simply say
"Hey, thanks for adding packageXY!" is always appreciated. (We get lonely too)

Public Relations
================

IUS started in mid-2009. We have seen, and continue to see significant growth.
That said, we believe IUS is one of the best resources available for upgrading
software on RHEL... so we want to ensure that the project continues to reach
new markets and more users. By googling around for 'PHP on RHEL' or 'MySQL 5.1
on RHEL' or similar... you will find pages and pages of howtos and blogs about
end-users bending over backwards to upgrade their software. Often times this
includes source installs, or just one-off recompiles of SRPMS with no real
intention of maintaining that package for the long haul. There are still so
many people that haven't found IUS, but the need too! If you come across these
posts, contact the author and introduce them to 'a better way to upgrade RHEL'!
Other than blogs you can often find similar posts on sites like
http://serverfault.com where users are in desperate need of being introduced to
IUS.

Speaking of blogs, the PR team can really help bring new users to the project
by blogging themselves. Write a howto on an experience you had with IUS, or how
IUS solved a problem for you.

RPM Packaging
=============

RPM Packaging is the heart of the project. There are some limitations to
packaging at the moment simply because our 'build farm' is internal, and not
publicly available. For those wanting to participate in packaging, please see
the :doc:`IUSDeveloperGuide`. A good place to start is by checking out the `bug
tracking system`_ to report bugs or submit patches or the
:ref:`Package_Wish_List` for packages that you might want to build and submit
to the project. 

**Note**: We are currently in the process of coding the next version of our
build system which will be released under an Open Source license. Once we have
an instance of that setup, contributors will be able to submit and manage
builds on their own.

Testing and Q/A
===============

Almost all updates sit in 'testing' for around 2 weeks before being pushed to
stable. An email is sent out to 'ius-community' on Sundays (weekly) with a list
of all packages in testing. You can also see what is in testing by simply
running 'yum check-update --enablerepo=ius-testing' on a box subscribed to IUS.

Testers are encouraged to review this list weekly, and perform standard
installations from ius-testing to verify the updates are smooth. Providing
feedback to the project (good or bad) regarding an update that is in testing is
very helpful for us to know things are running well. If you see an update in
testing, and test it... you can provide feedback via any open bugs for that
milestone which shouldn't be hard to find:

 * Go to: https://code.launchpad.net/ius
 * Find the 'Series' for the package in question
 * Find the milestone for this release under "Milestones and Releases"
 * Here you should be able to find any bugs associated with the update.
   There is often a 'bug' related to the release (packageXY Source Version
   Update, etc) which would be a good place to report bugs. If not, and no
   other bugs listed relate to the issue you saw... feel free to create a bug
   and link it to this milestone.

Contributors can also help with testing by enabling the 'ius-testing'
repository on any of their non-production machines that might be running IUS
packages...  and submitting bugs if anything breaks during nightly updates.

The Testing Team is also responsible for development of testing tools, scripts,
and automation. See: `Automated Testing with Jenkins CI`_

IUS Triage Team
===============

Contributors can join the `IUS Triage Team`_ on LP and join the mailing list
there.  Triage is an important part of ensuring the project makes forward
progress and that issues don't sit forever unresolved.

 * Engaging end users on ius-community@lists.launchpad.net
 * Reviewing new issues (bugs) submitted to LP
    * Researching if necessary
    * Posting follow up information that might help developers resolve the issue
    * Making the initial response to end user (possibly requesting more information)
    * Routing the issue to the right developer
    * Linking the issue to relating 'Series' and Bzr branches
    * Attaching links to any related external bugs
    * Setting category/status/severity appropriately
 * Following up with idle LP issues and working to get them resolved/closed.
 * Tracking upstream. Ensuring issues are created [or exist] in LP when new
   source versions are available upstream for IUS package updates.
 * Subscribing upstream 'announce' mailing lists to the ius-triage list to
   ensure updates are handled promptly.
