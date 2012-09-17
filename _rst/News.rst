============
News Updates
============

Scheduled Maintenance for July 13, 2011
=======================================

*Jul 12, 2011*

Hello,

We will have a scheduled maintenance during the hours of 12:00PM - 4:00PM CST on
Wed July 13, 2011. The details of this maintenance were outlined in our last
post, however have changed slightly (for the better). Our plan for the the
maintenance is as follows:

 * Add RHEL 6 repositories to dMirr (our mirrorlist service)
 * Remove old 5.x point release repos (never published to dMirr)

Previously we had mentioned that if you access the repository via a 'baseurl'
rather than 'mirrorlist' that your config will no longer work. This is not
necessarily true at this point as we are no longer changing the directly
structure of the repo (aside from removing the 5.x point releases).

Please note, we have never announced or published anything about the 5.x point
release repos as they were added for other reasons but we've decided not to go
down that path. For the majority of users who simply installed 'ius-release' and
used the standard 'mirrorlist' config, you should have no impact after the
maintenance.

Please also note that during this maintenance there will at times be brief
service interruptions as we restart services, and move files around. We will do
our best to minimize this.

If for some reason something is needed from the repository during the
maintenance and our primary repo is not available, you can also manually pull
packages from one of our downstream mirror sites:

 * http://www.applesauceman.com/ius/
 * http://mirrors.ircam.fr/pub/ius/
 * http://mirror.its.dal.ca/ius/
 * http://pancks.sothatswhy.org.uk/pub/ius/
 * http://ftp.astral.ro/mirrors/iuscommunity.org/
 * http://download.srv.ro/pub/ius/
 * http://mirror.rackspace.com/ius/
 * http://mirror.rackspace.co.uk/ius/
 * http://mirror.rackspace.hk/ius/
 * http://ftp.rediris.es/mirror/IUS_Community/


If you have any questions, please don't hesitate to contact us at
coredev@iuscommunity.org or jump in #iuscommunity on freenode.net where we'll
be.

Thanks!

-derks

Upcoming Changes to the IUS Yum Repository
==========================================

*Jun 28, 2011*

In the near future (not scheduled yet), we will be switching over to a new Yum
repository layout. This is due to the change in build syste as well as to clean
up a few things that were never officially released. The following should be
noted:

 * Most users should not notice the change
 * If you access our repos via a 'baseurl' rather than 'mirrorlist' your client
   config will no longer work. The directory layout is changing slightly so hard
   coded paths will break.
 * If you access any of the 5.x point release repos your client config will no
   longer work. We will no longer publish point release repos once the change
   over is complete.

We are announcing this now so that if there are any questions or concerns we
have time to respond to them before moving forward. We expect to schedule these
changes sometime over the next week or two. Please contact us if you feel this
may negatively affect you.

-derks