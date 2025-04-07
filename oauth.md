---
description: Portswigger Practitioner Lab notes for OAuth exploits
---

# 🅾️ OAuth

**OAUTH Scan Extension - make sure to check passive results**

**Check /.well-known/\* directory**

* Check under oauth server/domain, not the web server
* jwks.json
* openid-configuration
* oauth-authorization-server

**Redirect URI**

* Check for Open Redirects ANYWHERE in page
* Check that the Redirect\_uri is vulnerable to open redirectA

**State Param**

* No State may mean CSRF
* Make sure to drop request that uses the "oauth-linking" code

**Check the authenticate endpoint for params that ID the user i.e Email**

**Check Bearer token in API call for /userinfo**

* Auth code grant type

**OpenID Connect**

* extension of OAuth
* Always check if supported with openid Scope value and id\_token response type
* Response\_type id\_token added
  * JWT
* uses claims to store userinfo in dictionary-like keys

**Unprotected dynamic client registration**

* /register should require authentication
* Can lead to vulns in the POST to /register
  * SSRFs in URI params
  * Can use logo\_uri to fetch info
    * Info will be revealed in Burp Traffic when retrieving logo info
    * Testable with Burp Collab

