---
description: Exploiting Web Caches
---

# 🗄️ Web Cache Poisoning

Cache Design Flaws

* Param Miner
  * GET-Params, Cookies, Headers
* Can use any custom get param as cachebuster to avoid poisoning root
* Cache-Control header reveals max age until cache reset
* Even if found on utm\_content check generic query params and if headers (i.e origin) are part of the key.
* X-Cache shows when "hit"/set
* Make sure to check for XSS
  * Includes checking loaded pages and 404s
  * Some DOM Open Redirects may not work in intercept
    * Visit URL if vulnerable input is taken from there

#### Cache Implementation Flaws

1. Identify Cache Oracle
   * Page/Endpoint that gives info on cache behavior such as full URL and a query param
   * Check docs on cache i.e `Pragma: akamai-x-get-cache-key`
2. Probe Key Handling
   * Check if cache transforms the cache-key
3. Identify Exploitable Gadget
   * XSS and Open Redirects

**Unkeyed Port**

* Host Header Port
  * check if port has to be numeric

**Unkeyed Query String**

* Cache-keys commonly stripped from request line
  * Does NOT mean they can't be part of the exploitable gadet
  * Put Cachebuster in headers
  * Add dynamic cache busters and include cachebuster in headers options in Param Miner
  * Check if request line treated differently by cache i.e GET // in Apache

**Cache Parameter Cloaking**

* Hiding parameters by abusing how the cache parses the URL
  * `/?example=1?execluded-example=2`
  * Ruby: `GET /?keyed_param=abc&excluded_param=123;keyed_param=bad-stuff-here`
  * Rail Param Cloaking option in Param Miner
  * utm\_content and other utm\_ params can be useful

**fat GET**

* Same as Parameter cloaking only the malicious value is sent in the body instead of having multiple GET params
* Can try to override method i.e `X-HTTP-Method-Override`

```
GET /function=setName HTTP/1.1
UNKEYED:
HEADERS:

function=alert(document.cookie)	
```

**Dynamic Content in Resource Imports**

```
GET /style.css?excluded_param=alert(1)%0A{}*{color:red;} HTTP/1.1

HTTP/1.1 200 OK
Content-Type: text/html
…
This request was blocked due to…alert(1){}*{color:red;}
```

**Normalized Cache Keys**

* Can be used to bypass URL encoding in XSS
