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
### Add the UWHealth Root Certificate to cacert.pem
Copy `cacert.pem to a new location:
```
> mkdir %USERPROFILE%\certs
> copy cacert.pem %USERPROFILE%\certs\
```

Next, navigate to your new directory and open the file. Copy the UWHealth root certificate at the end of the file. You can get this certificate from Craig. 
* You can copy the certficiate to a new line if you want to, but make sure there are no blank lines between the bulk of the text and the appended certificate.
* Make sure their are no blank lines within the appended certificate.
* Make sure the file ends with a single blank line.

Finally, change the filename to `ca-bundle.crt`:
```
move %USERPROFILE%\certs\cacert.pem %USERPROFILE%\certs\ca-bundle.crt
```

### Enable SSL Verification for git
The following commands enable SSL verification for `git`, and set the certificate file path.
```
> git config --global http.sslVerify true
> git config --global http.sslCAInfo %USERPROFILE%\certs\ca-bundle.crt
```

### Enable SSL Verification for pip
The following commands set the certificate file path for `pip`, then check it.
```
> pip config set global.cert %USERPROFILE%\certs\ca-bundle.crt
> pip config list
```

### Enable SSL Verification for conda
The following commands set the certificate file path for `conda`, then check it.
```
> conda config --set ssl_verify %USERPROFILE%\certs\ca-bundle.crt
> conda config --show ssl_verify
```
