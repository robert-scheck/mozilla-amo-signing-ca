Information related to Mozilla AMO Production Signing Service
=============================================================

Root certificate authorities in Mozilla Firefox source code:

  * [C=US, O=Mozilla Corporation, OU=Mozilla AMO Production Signing Service, CN=root-ca-production-amo](7dd43faa.0) ([upstream commit](https://github.com/mozilla/gecko-dev/commit/7b5d12ad46e0c48500458432b69b9330e311babe))
  * [C=US, ST=CA, L=Mountain View, O=Addons Test Signing, CN=test.addons.signing.root.ca](9d0dad87.0) ([upstream commit](https://github.com/mozilla/gecko-dev/commit/7b5d12ad46e0c48500458432b69b9330e311babe))

Intermediate CAs signed by Mozilla AMO Production Signing Service root CA:

  * [C=US, O=Mozilla Corporation, OU=Mozilla AMO Production Signing Service, CN=production-signing-ca.addons.mozilla.org](51b97126.0) (used until at least [2017-06-15](https://github.com/mozilla/gecko-dev/commit/28c0b4915fda9f6a93a18047974f17653eb1f24b))
  * [C=US, O=Mozilla Corporation, OU=Mozilla AMO Production Signing Service, CN=signingca1.addons.mozilla.org](fe942ae0.0) (used until 2019-05-04)
  * [C=US, O=Mozilla Corporation, OU=Mozilla AMO Production Signing Service, CN=signingca1.addons.mozilla.org](fe942ae0.1) (used since 2019-05-04)

Some findings:

  * [CN=root-ca-production-amo](7dd43faa.0) seems to be hard-coded in source code
  * [CN=root-ca-production-amo](7dd43faa.0) has no CRL
  * [CN=root-ca-production-amo](7dd43faa.0) expires before [CN=signingca1.addons.mozilla.org](fe942ae0.1) (2025-03-14 < 2025-04-04, possible "Armagadd-on 3.0" at 2025-03-14)
  * [CN=signingca1.addons.mozilla.org](fe942ae0.1) has [non-reachable CRL](http://addons.mozilla.org/ca/crl.pem) (as of 2019-05-04)

```
$ wget http://addons.mozilla.org/ca/crl.pem 2>&1 | egrep "^(Location|HTTP|20)"
HTTP request sent, awaiting response... 301 Moved Permanently
Location: https://addons.mozilla.org/ca/crl.pem [following]
HTTP request sent, awaiting response... 301 Moved Permanently
Location: /ca/firefox/crl.pem [following]
HTTP request sent, awaiting response... 301 Moved Permanently
Location: /ca/firefox/crl.pem/ [following]
HTTP request sent, awaiting response... 404 Not Found
2019-05-05 03:07:42 ERROR 404: Not Found.
$ 
```

View certificates of Firefox Add-ons:

  * `unzip <add-on>.xpi`
  * `openssl pkcs7 -in META-INF/mozilla.rsa -inform DER -print | less`
  * `openssl pkcs7 -in META-INF/mozilla.rsa -inform DER -outform PEM -print_certs -out cert.pem`

See also:

  * https://blog.mozilla.org/addons/2015/02/10/extension-signing-safer-experience/
  * https://wiki.mozilla.org/Add-ons/Firefox57
  * https://wiki.mozilla.org/Add-ons/Extension_Signing
  * https://developer.mozilla.org/en-US/docs/Archive/Add-ons/Signing_an_XPI
  * https://bugzilla.mozilla.org/show_bug.cgi?id=1070155
  * https://wiki.mozilla.org/AMO/SigningService/API
  * https://bugzilla.mozilla.org/show_bug.cgi?id=1548973
  * https://blog.mozilla.org/addons/2019/05/04/update-regarding-add-ons-in-firefox/
  * https://www.di-mgt.com.au/how-mozilla-signs-addons.html
