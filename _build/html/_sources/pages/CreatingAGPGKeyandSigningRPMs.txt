:tocdepth: 2

.. _ius.io: https://ius.io
.. _sphinx_iuscommunity Github repo: https://github.com/iuscommunity/sphinx_iuscommunity

===================================
Creating A GPG Key and Signing RPMs
===================================

.. note:: As of September 24, 2015, `ius.io`_ has replaced this website.  This
          site will stay up for about 30 days for archive purposes then redirect to
          the new site.  The old site code will remain available at the
          `sphinx_iuscommunity Github repo`_.

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

*Note: Replace the last three accordingly. This should be a generic name/email
but should exist should anyone send email to the address.*

You will be prompted to enter a passphrase. Make this a strong, cryptic,
passphrase and store it somewhere so you never forget it. Next you will be
asked to generate a lot of random bits.... type on the keyboard, move the mouse
(console), etc... until it completes (it takes a while). A handy way I've found
to generate entropy is to just run a grep on '/' like::

    [you@linuxbox ~]$ grep -ri 'something bogus' /

*Note: probably not preferred to do the above in production. Run that for a
while until the GPG key generation completes. On successful completion, the
command spits out something like the following*::

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
    
You should change 'MYORG' to something else more appropriate for your
organization. The Key-ID (454CBFAB) in this example is obtained from the output
in the previous example. You now have your public and private keys. The keys
look like::

    [you@linuxbox ~]$ cat MYORG-GPG-KEY*   
    
    -----BEGIN PGP PRIVATE KEY BLOCK-----
    Version: GnuPG v1.4.5 (GNU/Linux)
    
    lQHhBEs71qsRBAChJ9cAdA4zn46ANnjmd4EKHc3j7s0gsdh/FKHtVo6CkX81Pp0I
    OMWpUPp9m7gfbsvRDPCaznT3/BXFKB9ICKNRh3gLL2jGyzazUrh+WosWy7VQLzlx
    um2ve+iTnTv7kt3n7Jsm1cPj9pE6G9mfuLyZZONLjDtIPf382Lf1W6KzNwCgnoGL
    vy5kGo8Wqx6aBBK+gffgx50D/i3nfA5YA+2UvkpVUSjBWmqhXAZMRfNpfDzDd/v+
    xuo2L0VbKF/l8OaL51fP+OSgZPLYrss9Z7Qco2GIapdPD8wJsjK4HY6mt+8EwOG2
    k9wRY+3uWtbuxJFBwA23s3L9S5WtGutP6AqCxspRVN/wRVtwT6olOzGYDEROY4Vn
    pDsnA/4ytLCvwGS27N6GlbumKXTaUWIXWkOVUW8KevkpVc5a1uitUpBUX+Z8mBdc
    3qvSnrNsutOgmqaFYGRwHRGkv9kniNCQDFAj3h40hWXZpGjvUXRsC6cw0TGXh0Nm
    fvai8VhRVolLV2yhUiT7M5nYeKW27ysVUQq0DCFW4j5VjvMaEv4DAwJFo0ixdOak
    iGBra3mmX610A7XfkGeRKkqkTozWGEoJoQXCjGxqMte6TUbJ9At+DvW4uirq59OA
    kIEb/LQrSm9obiBEb2UgKFJQTSBEZXZlbG9wZXIpIDxqZG9lQGV4YW1wbGUuY29t
    PohgBBMRAgAgBQJLO9arAhsDBgsJCAcDAgQVAggDBBYCAwECHgECF4AACgkQ8I6y
    7kVMv6uCnQCeLw4Xi1xEaqi5+1hNGPfpUbuvqmwAn33XfvOWsyXelF6mSLPcZCPq
    m+27nQJjBEs71rMQCACIWoPREd46vuHIxJAwwFmrvyBWFnerS9FsDmHctkQmc/9J
    YzKs2FjrAMgSfhKoGiJCJuVhRrKfBzWWY55z0wnlFHUGZrJUrqnNfKjO6zXlSfrQ
    u8+g2kN1KPtoPQNzkpllNdhfkOz9/axa/+xr5SxqTjqx/D6hmiXw1OMuagGxQoqS
    zEmbv5BUUScPFm9KzUsu/2NIUO01WZxYPwaCfYUxDojLpvoNvHloue6NcKDOsLfT
    T7q0/A5MTcSt6Qr1paHa45cW2TLVe1jK8SmtGsMeU3pRTXj5HPN8TbCviLR6t9Uz
    myFBBORf0i7+jethEc/A+ARgXeutDDN97NFI3DEzAAMFB/91fjswPYAX83twAIFk
    zXXQYg3hKhlHP49D79U+SEpMtPSdXIPJ8CZAuc62KYUUBK/ERmmM986Mfai9OMVE
    rV+O2cZmyg/HZ5BaDLm5nY47ZoezTInI3n83BMJF8ET30uW4NUZa0Su+KjrjE+jA
    8i7RvHYjRrffxxyILd28GE5M/uVJKd8f+lq42QCiHx5zir0SqSVHOU2MMd028do7
    niNTRcoHpqNsiCfIALdICCXGeQDK+XRhFCQcIU9wYObKfJJLv4TLPgRaCFZ7+1Zj
    UMZaK34wivBc250t1rurA9eF59xTflCRA4P/fGKeXOj9vEQzQjz0mhXTfMuR87FP
    UnAI/gMDAkWjSLF05qSIYFqaObOxESIozAW3jB87HcEWYJ/9BSTkHCy+iL0N9321
    RXxvMkvLNgy9TvvA003Q6/qhnXwjULXdTgW0k8hadiQVWy59IuA+Tq2ISQQYEQIA
    CQUCSzvWswIbDAAKCRDwjrLuRUy/q5kiAJwJWfDhc+U3cQddOrdcdoBrlVh1/ACf
    YERkTP/fz8oJqTr2/CBfhLhsTi0=
    =C/iH
    -----END PGP PRIVATE KEY BLOCK-----
    
    -----BEGIN PGP PUBLIC KEY BLOCK-----
    Version: GnuPG v1.4.5 (GNU/Linux)
    
    mQGiBEs71qsRBAChJ9cAdA4zn46ANnjmd4EKHc3j7s0gsdh/FKHtVo6CkX81Pp0I
    OMWpUPp9m7gfbsvRDPCaznT3/BXFKB9ICKNRh3gLL2jGyzazUrh+WosWy7VQLzlx
    um2ve+iTnTv7kt3n7Jsm1cPj9pE6G9mfuLyZZONLjDtIPf382Lf1W6KzNwCgnoGL
    vy5kGo8Wqx6aBBK+gffgx50D/i3nfA5YA+2UvkpVUSjBWmqhXAZMRfNpfDzDd/v+
    xuo2L0VbKF/l8OaL51fP+OSgZPLYrss9Z7Qco2GIapdPD8wJsjK4HY6mt+8EwOG2
    k9wRY+3uWtbuxJFBwA23s3L9S5WtGutP6AqCxspRVN/wRVtwT6olOzGYDEROY4Vn
    pDsnA/4ytLCvwGS27N6GlbumKXTaUWIXWkOVUW8KevkpVc5a1uitUpBUX+Z8mBdc
    3qvSnrNsutOgmqaFYGRwHRGkv9kniNCQDFAj3h40hWXZpGjvUXRsC6cw0TGXh0Nm
    fvai8VhRVolLV2yhUiT7M5nYeKW27ysVUQq0DCFW4j5VjvMaErQrSm9obiBEb2Ug
    KFJQTSBEZXZlbG9wZXIpIDxqZG9lQGV4YW1wbGUuY29tPohgBBMRAgAgBQJLO9ar
    AhsDBgsJCAcDAgQVAggDBBYCAwECHgECF4AACgkQ8I6y7kVMv6uCnQCeLw4Xi1xE
    aqi5+1hNGPfpUbuvqmwAn33XfvOWsyXelF6mSLPcZCPqm+27uQINBEs71rMQCACI
    WoPREd46vuHIxJAwwFmrvyBWFnerS9FsDmHctkQmc/9JYzKs2FjrAMgSfhKoGiJC
    JuVhRrKfBzWWY55z0wnlFHUGZrJUrqnNfKjO6zXlSfrQu8+g2kN1KPtoPQNzkpll
    NdhfkOz9/axa/+xr5SxqTjqx/D6hmiXw1OMuagGxQoqSzEmbv5BUUScPFm9KzUsu
    /2NIUO01WZxYPwaCfYUxDojLpvoNvHloue6NcKDOsLfTT7q0/A5MTcSt6Qr1paHa
    45cW2TLVe1jK8SmtGsMeU3pRTXj5HPN8TbCviLR6t9UzmyFBBORf0i7+jethEc/A
    +ARgXeutDDN97NFI3DEzAAMFB/91fjswPYAX83twAIFkzXXQYg3hKhlHP49D79U+
    SEpMtPSdXIPJ8CZAuc62KYUUBK/ERmmM986Mfai9OMVErV+O2cZmyg/HZ5BaDLm5
    nY47ZoezTInI3n83BMJF8ET30uW4NUZa0Su+KjrjE+jA8i7RvHYjRrffxxyILd28
    GE5M/uVJKd8f+lq42QCiHx5zir0SqSVHOU2MMd028do7niNTRcoHpqNsiCfIALdI
    CCXGeQDK+XRhFCQcIU9wYObKfJJLv4TLPgRaCFZ7+1ZjUMZaK34wivBc250t1rur
    A9eF59xTflCRA4P/fGKeXOj9vEQzQjz0mhXTfMuR87FPUnAIiEkEGBECAAkFAks7
    1rMCGwwACgkQ8I6y7kVMv6uZIgCfbpZ8kaevKp55GIGoOWuVo/b/Zc8An1Q3TG8G
    a9p0vD3QA6XzbZEnOJ0C
    =Xmla
    -----END PGP PUBLIC KEY BLOCK-----


The **Public Key** is the only portion that should ever be made public. The
Private Key must be kept secure, preferably on a system that does not allow
outside access from the public internet. It is also recommended to make a hard
copy (burned cd/usb key) of the Public/Private key pair and keep that somewheere
secure like a Safe Deposit Box or similar. Anyone who obtains your
Private/Public key pair can sign RPMs as if they were signed by you and
potentially perform malicious acts to your users.

You will need to make the public key available, so lets assume that you have
uploaded this key to your web server (generally in the /pub folder or root of
your repository). For this document we will assume that the MYORG-GPG-KEY is
available at the following URL:

 * http://www.example.com/pub/MYORG-GPG-KEY 


Importing a Private Key
=======================

If you wish to import your Private key to another server, or to restore from
backup, the following imports your key::

    [you@linuxbox ~]$ gpg --import MYORG-GPG-KEY.private 
    gpg: key 454CBFAB: secret key imported
    gpg: key 454CBFAB: public key "John Doe (RPM Developer) <jdoe@example.com>" imported
    gpg: Total number processed: 1
    gpg:               imported: 1
    gpg:       secret keys read: 1
    gpg:   secret keys imported: 1
    
    [you@linuxbox ~]$ gpg --list-secret-keys
    /home/you/.gnupg/secring.gpg
    ----------------------------
    sec   1024D/454CBFAB 2009-12-30
    uid                  John Doe (RPM Developer) <jdoe@example.com>
    ssb   2048g/767143C3 2009-12-30

Signing RPMs With Your GPG Key
==============================

Now that we have the GPG keys configured, you can sign RPMS with it. Before
doing so, you of course need the RPM(s) to sign and also to add the following to
the signing user's ~/.rpmmacros file:

**~/.rpmmacros**::

    %_signature gpg 
    %_gpg_name John Doe (RPM Development) <jdoe@example.com>   


Please note that the '%_gpg_name' must match exactly how it is listed via the
key. If you forget this information, you can list the GPG keys on your system::

    [you@linuxbox ~]$ gpg --list-keys  
    
    /home/you/.gnupg/secring.gpg
    ----------------------------
    sec   1024D/454CBFAB 2009-12-30
    uid                  John Doe (RPM Developer) <jdoe@example.com>
    ssb   2048g/767143C3 2009-12-30


*Note: Any users wishing to sign RPMs with this GPG Key need to install the
Private GPG Key for signing. For that reason you may wish to restrict access to
the GPG key to a shared users that everyone else sudo's to in order to sign RPMs
limiting how spread out the Private portion of the key is, and keeping it more
secure.*

Once everything is setup, signing RPMs is as easy as::

    [you@linuxbox ~]$ rpm --resign /path/to/rpms/*.rpm


Looking at the info of a package [rpm -qip /path/to/signed/package.rpm]
you can see our signature::

    Signature   : DSA/SHA1, Wed Dec 30 17:57:49 2009, Key ID f7ba18a0454cbfab

Notice that the second half of the Key ID is our key '454CBFAB'.


Installing a Public GPG Key
===========================

In order to restrict Yum to only install signed RPMs you must set 'gpgcheck=1'
in the yum configuration for the repo, and have the Public GPG Key installed::

    [you@linuxbox ~]$ rpm --import http://example.com/pub/MYORG-GPG-KEY

*Or wherever you made it publicly available at.*

