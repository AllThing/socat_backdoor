# The Socat backdoor

This repo contains some research I'm currently doing on the Socat backdoor.

On February 1st 2016, a security advisory was posted to Openwall by a [Socat](http://www.dest-unreach.org/socat/) developer: [Socat security advisory 7 - Created new 2048bit DH modulus](http://www.openwall.com/lists/oss-security/2016/02/01/4)

> In the OpenSSL address implementation the hard coded 1024 bit DH p parameter was not prime. The effective cryptographic strength of a key exchange using these parameters was weaker than the one one could get by using a prime p. Moreover, since there is no indication of how these parameters were chosen, the existence of a trapdoor that makes possible for an eavesdropper to recover the shared secret from a key exchange that uses them cannot be ruled out.  
> A new prime modulus p parameter has been generated by Socat developer using OpenSSL dhparam command.  
> In addition the new parameter is 2048 bit long.

This is a pretty weird message with a [Juniper](http://forums.juniper.net/t5/Security-Incident-Response/Important-Announcement-about-ScreenOS/ba-p/285554) feeling to it. 

[Socat's README](http://www.dest-unreach.org/socat/doc/README) tells us that you can use their free software to setup an encrypted tunnel for data transfer between two peers.

Looking at the commit logs you can see that they used a 512 bits Diffie-Hellman modulus until last year (2015) january when [it was replaced with a 1024 bits one](http://repo.or.cz/socat.git/commitdiff/281d1bd6515c2f0f8984fc168fb3d3b91c20bdc0).

> Socat did not work in FIPS mode because 1024 instead of 512 bit DH prime is required. Thanks to **Zhigang Wang** for reporting and sending a patch.

The person who pushed the commit is *Gerhard Rieger* who is the same person who fixed it a year later. In the comment he refers to Zhigang Wang, an Oracle employee at the time who has yet to comment on his mistake.

But what is the mistake? The new 1024 bits DH modulus **was not a prime**.

