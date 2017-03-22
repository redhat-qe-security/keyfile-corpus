Collection of files with various keys stored in in different formats and using
different encryption algorithms.

The names should be mostly self-explanatory, below are some more tricky parts:

 - default - value is using the ASN.1 default value and as such is omitted
   from DER encoding
 - salt - length of salt used for the algorithm, in bytes
 - iter - number of iterations that the PRF needs to be run, actual number
 - keyLen - for PBKDF2: length of the key to output, in bytes
 - prf - for PBKDF2: pseudo-random function, default is hmacWithSHA1
 - bf-cbc - Blowfish cipher in CBC mode
 - IV - length of initialisation vector used, in bytes
 - none - when a specific entry in file does not use encryption, it's specified
   as none
 - pass - the parameter is the name of the password used for both integrity
   (MAC) and encryption
 - pass-mac - name of password used for integrity check
 - pass-cipher - name of password used for encryption
 - ber(inf) - when the files use the BER indefinite form encoding for some
   items in the structure

Other encryption method names correspond with the short OID names assigned to
them.

Default settings
================
Different applications and libraries create files in different formats and
with different settings. The following are some examples.

OpenSSL
-------

OpenSSL `pkcs12` command by default will create a PKCS#12 file with settings
like the ones used in
rsa(2048,sha256),cert(pbeWithSHAAnd40BitRC2-CBC,salt(8),iter(2048)),key(pbeWithSHAAnd3-KeyTripleDES-CBC,salt(8),iter(2048)),mac(sha1,salt(8),iter(2048)),pass(ascii).p12
file, if it was compiled with RC2 support. If OpenSSL was compiled without
RC2 support, it will create a file like
rsa(2048,sha256),cert&key(pbeWithSHAAnd3-KeyTripleDES-CBC,salt(8),iter(2048)),mac(sha1,salt(8),iter(2048)),pass(ascii).p12
.

OpenSSL versions before 1.1.0, when PBES2 encryption is specified by user, will
always use the default hmac for PBKDF2 (i.e. hmacWithSHA1). Later versions
(i.e. 1.1.0 and 1.1.1) always use hmacWithSHA256 for PBKDF2 in PKCS#12 files.

GnuTLS
------

GnuTLS `certtool` command by default will create a PKCS#12 file with settings
like the ones used in
'rsa(2048,sha256),cert(pbeWithSHAAnd3-KeyTripleDES-CBC,salt(8),iter(5318)),key(pbeWithSHAAnd3-KeyTripleDES-CBC,salt(8),iter(5204)),mac(sha1,salt(8),iter(10240),pass(ascii).p12'

If the cipher is specified (`aes-128`), it will create a PKCS#12 file with
settings like the ones used in
rsa(2048,sha256),cert(PBES2(PBKDF2(salt(18),iter(5127),keyLen(default),prf(default)),aes-128-cbc(IV(16)))),key(PBES2(PBKDF2(salt(16),iter(5301),keyLen(default),prf(default)),aes-128-cbc(IV(16)))),mac(sha1,salt(8),iter(10240)),pass(ascii).p12

NSS
---

NSS `pk12util` command creates PKCS#12 files with the BER indefinite form
encoding, and other settings as used in
rsa(2048,sha256),key(pbeWithSHAAnd3-KeyTripleDES-CBC,salt(16),iter(2000)),cert(pbeWithSHAAnd40BitRC2-CBC,salt(16),iter(2000)),mac(sha1,salt(16),iter(2000)),pass(ascii),ber(inf).p12
and
rsa(2048,sha256),key(pbeWithSHAAnd3-KeyTripleDES-CBC,salt(16),iter(2000)),cert(pbeWithSHAAnd40BitRC2-CBC,salt(16),iter(2000)),mac(sha1,salt(16),iter(2000)),pass(unicode,nss-3.28.3-1.1.fc24),ber(inf).p12

Note, order reversal (first key then certificate) reprsents the internal
PKCS#12 PDU order.
