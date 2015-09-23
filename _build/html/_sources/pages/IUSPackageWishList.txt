:tocdepth: 2

.. _ius.io: https://ius.io
.. _sphinx_iuscommunity Github repo: https://github.com/iuscommunity/sphinx_iuscommunity
.. _IUS Package Wish List: https://bugs.launchpad.net/ius/+bugs?field.tag=wishlist
.. _Advanced Search: https://bugs.launchpad.net/ius/+bugs?advanced=1
.. _LaunchPad project: http://bugs.launchpad.net/ius

=====================
IUS Package Wish List
=====================

.. note:: As of September 24, 2015, `ius.io`_ has replaced this website.  This
          site will stay up for about 30 days for archive purposes then redirect to
          the new site.  The old site code be still be available at the
          `sphinx_iuscommunity Github repo`_.

.. contents::
    :backlinks: none
    
The List
========

 * `IUS Package Wish List`_

Requesting New Package Additions
================================

New packages are added to IUS all the time. To request a new package addition,
first do an `Advanced Search`_ to ensure the package hasn't already been requested
and rejected in the past. To perform an advanced search select all status', add
your package name to the search box, and add 'wishlist' to the tags section.
If the package has not been requested in the past, create a bug in our `LaunchPad
project`_ with the following information.

Subject::

    WL: <package_name> <major_version>

Body::

    DESCRIPTION: <package_description>
    REASON: Why you want the package in 100 words or less
    
Extra Options::

    Tags: wishlist

It is important to add the tag 'wishlist' so that it gets listed under the `IUS
Package Wish List`_.

Example::

    WL: ruby 1.9

    ----------------------------------------------------------------------
    DESCRIPTION: Ruby is the interpreted scripting language for quick and easy
    object-oriented programming.  It has many features to process text
    files and to do system management tasks (as in Perl).  It is simple,
    straight-forward, and extensible.
    
    REASON: Our company relies heavily on ruby 1.9.x and are looking to
    standardize all of our production applications on it.
    ----------------------------------------------------------------------
