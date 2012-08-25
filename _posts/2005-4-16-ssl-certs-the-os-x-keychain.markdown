---
layout: post
title: !binary |-
  U1NMIENlcnRzICYgdGhlIE9TIFggS2V5Y2hhaW4=
---
Given that Apple is releasing <a href="http://www.apple.com/macosx/tiger">Mac OS X 10.4 Tiger</a> in less than two weeks, their upgraded Mail 2 may fix the troubling process of <a href="http://www.sfsmith.com/blog/archives/000108.html">adding a self-signed certificate authority in Mail</a> to implicitly trust your SSL-enabled email server.

Here's the shell method to download an SSL server's certificate and then add it to the Keychain (in OS X 10.3.8):

1. Get the certificate from the SSL-enabled service:

    `openssl s_client -connect` **your.server.com:port** `| sed -n "/BEGIN CERT/,/END CERT/p" > ~/Desktop/temp.pem`

2. Add the certificate to your X509 Keychain:

    `sudo /usr/bin/certtool i ~/Desktop/temp.pem v k=/System/Library/Keychains/X509Anchors`

3. Restart Mail (or any app) that needs to be aware of the new Keychain item.


I use [& highly recommend] <a href="http://csoft.net">CubeSoft's OpenBSD-based hosting</a> & <a href="http://textdrive.com">TextDrive's FreeBSD-based hosting</a>, both of which support IMAP/SSL & SMTP/SSL. Although, CubeSoft's IMAP/SSL certificate is self-signed, which lead me to figure out this solution.
