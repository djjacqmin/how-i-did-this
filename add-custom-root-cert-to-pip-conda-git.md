# How to add a custom CA Root certificate to the CA Store used by pip, conda and git in Windows
## Problem Statement
When I try to use pip in Windows on my UWHealth computer, I get this message:
```
Could not fetch URL https://pypi.org/simple/pip/: There was a problem confirming the ssl certificate: HTTPSConnectionPool(host='pypi.org', port=443): Max retries exceeded with url: /simple/pip/ (Caused by SSLError(SSLError(1, '[SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:833)'),)) - skipping
```

When I try to use git in Windows on my UWHealth computer, I get this message:
```
fatal: unable to access 'https://github.com/djjacqmin/how-i-did-this.git/': SSL certificate problem: self signed certificate in certificate chain
```

## System Parameters
* Windows 10 Enterprise

## Solution
My solution is derived from [this Stack Overflow page](https://stackoverflow.com/questions/39356413/how-to-add-a-custom-ca-root-certificate-to-the-ca-store-used-by-pip-in-windows/52961564#52961564)

### Get an up-to-date Certificate Authorities bundle
The Stack Overflow post discusses how to get an up-to-date CA bundle online. I have chosen to use the cacert.pem file from the most up-to-date version of certifi in my Anaconda distribution. I found the certificate here:
```
C:\Anaconda3\pkgs\certifi-2019.3.9-py37_0\Lib\site-packages\certifi\cacert.pem
```


