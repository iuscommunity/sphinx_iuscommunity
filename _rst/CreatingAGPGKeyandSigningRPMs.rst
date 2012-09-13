:tocdepth: 2

===================================
Creating A GPG Key and Signing RPMs
===================================


.. contents::
    :backlinks: none

Summary
=======

This howto outlines how to create a GPG Key for signing RPMs. It uses the
**gnupg** package and gpg utilities (notice that gnupg2 will not work with
these steps).

Creating a Public/Private GPG Key Pair 
======================================

The following commands create a Private/Public GPG Key Pair (Note you should
actually login console/ssh with this user, not sudo to it)::

    [you@linuxbox ~]$ gpg --gen-key

You will be asked a series of questions, answer as follows (answers in italic):

* Please select what kind of key you want: (1) DSA and ElGamal (default)
* What keysize do you want? 2048
* Key is valid for? 0 (key does not expire)
* Real name: John Doe
* Email Address: jdoe@example.com
* Comment: RPM Development

Note: Replace the last three accordingly. This should be a generic name/email
but should exist should anyone send email to the address.

You will be prompted to enter a passphrase. Make this a strong, cryptic,
passphrase and store it somewhere so you never forget it. Next you will be
asked to generate a lot of random bits.... type on the keyboard, move the mouse
(console), etc... until it completes (it takes a while). A handy way I've found
to generate entropy is to just run a grep on '/' like::

    [you@linuxbox ~]$ grep -ri 'something bogus' /

Note: probably not preferred to do the above in production. Run that for a
while until the GPG key generation completes. On successful completion, the
command spits out something like the following::

    pub   1024D/454CBFAB 2009-12-30
          Key fingerprint = 2D38 3000 DD7D 68C0 45D3  76C0 F08E B2EE 454C BFAB
    uid                  John Doe (RPM Developer) <jdoe@example.com>
    sub   2048g/767143C3 2009-12-30

Take notice of the first portion, specifically **454CBFAB** as this is the id of
the public key. 

Exporting Private/Public Keys for Safe Keeping
==============================================

You must export the keys so that you can save them to a secure location (and so
they can be imported in the future). To list the keys you have installed::

    [you@linuxbox ~]$ gpg --list-secret-keys
    /home/you/.gnupg/secring.gpg
    ----------------------------
    sec   1024D/454CBFAB 2009-12-30
    uid                  John Doe (RPM Developer) <jdoe@example.com>
    ssb   2048g/767143C3 2009-12-30

Exporting the Public GPG Key
============================

In order to use the GPG Key with RHN, you will need to make the Public portion
available in a Public GPG Key. The following commands make this happen::

    [you@linuxbox ~]$ gpg --export-secret-key -a 454CBFAB > MYORG-GPG-KEY.private 
    [you@linuxbox ~]$ gpg --export -a 454CBFAB > MYORG-GPG-KEY.public

