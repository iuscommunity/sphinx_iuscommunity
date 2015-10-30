:tocdepth: 2

.. _ius.io: https://ius.io
.. _sphinx_iuscommunity Github repo: https://github.com/iuscommunity/sphinx_iuscommunity

.. _Managing Updates:

================
Managing Updates
================

.. note:: As of September 24, 2015, `ius.io`_ has replaced this website.  This
          site will stay up for about 30 days for archive purposes then redirect to
          the new site.  The old site code will remain available at the
          `sphinx_iuscommunity Github repo`_.

.. contents::
    :backlinks: none
    
What Should be Updated
======================

Upsream Source Updates
----------------------

One of the goals of IUS is to remain inline with upstream stable sources. That
said, anytime a new source update is available an update is general acceptable.
That said, the developer has full right to skip a source update under the
following conditions:

 * The update is not applicable to the project. I.E. A fix was applied for a
   specific operating system that IUS doesn't support.
 * The update does not have any security fixes. Any security fixes needs to be
   applied asap. Alternatively security and major bug fixes can be backported if
   preferred.
 * The update significantly breaks backward compatibility (which should not
   happen in a minor release update... so if it does be sure to wag your finger
   at the upstream devs and let them know what the business is). 

Back-Up-Porting from RHEL and Fedora
------------------------------------

It is very common that backports to RHEL packages and updates in the current
version of Fedora might apply IUS packages. We call this :ref:`back-up-porting`.
This is sometimes required to get security fixes applied before the next upstream
stable source release. Doing so takes a more intimate knowledge of the source of
your package and should not be done without caution and thorough testing.

We also follow changes in Fedora to remain inline with what users expect. For
example, adding a new subpackage for PHP or changing a default config setting.
Changes like this should applied to IUS packages where possible and compatible
with previous versions.


Update Frequency
================

Some upstream devs don't really have any concept of release management and just
push out new updates whenever it is convenient. Though IUS is following upstream
stable, we also kind of want to take the precautions to keep our releases as
stable as possible. A good rule of thumb is to let a number of changes/fixes/etc
acumulate and pushing out one larger update. Updates should be done no more than
once every two weeks [preferably once a month] for the same package, unless a
security or major bug fix is required.

Our goal as packagers should not be zero day releases [to stable] for new
upstream stable sources. Getting a zero day release out to testing is great,
and by all means please do. But be sure that update gets proper testing before
making it to stable. 
