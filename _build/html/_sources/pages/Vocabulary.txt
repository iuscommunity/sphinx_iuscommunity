:tocdepth: 2

.. _backporting: http://www.redhat.com/security/updates/backporting/?sc_cid=3093

================================
IUS Community Project Vocabulary
================================

.. contents::
    :backlinks: none
    
Summary
=======

This dictionary lists some vocabulary that might not be common, or that has
specific meaning within the project.


Language
========

.. _back-up-porting:

back-up-porting
---------------

Similar to Red Hat's process of `backporting`_, the IUS Community Project also
performs back-up-porting. This is the process of taking a backport [i.e. a
patch] from a Red Hat update, and then up-porting it to a newer version of the
same package in IUS.

Often times back-up-porting does not apply cleanly usually because the diff
patches were against a different version of the source. This all depends on how
far you've diverged from the Red Hat version... as well as if the file(s) in
question have changed since then.

It should be noted that just because Red Hat backports something, it doesn't
mean IUS packages need that update. To be sure you can verify the CVE or similar
security tracking number to determine if it applies the IUS version of the
source. Also, back-up-ported patches are generally only required and applied
during the interim before the next upstream source release. Almost always, these
patches are applied upstream and need to be removed during the next source
update (only if it's been previously applied). 