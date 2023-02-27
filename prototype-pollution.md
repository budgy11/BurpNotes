---
description: Prototype pollution notes for the first couple labs
---

# ⛽ Prototype Pollution

* [https://portswigger.net/burp/documentation/desktop/tools/dom-invader/prototype-pollution#scanning-for-prototype-pollution-gadgets](https://portswigger.net/burp/documentation/desktop/tools/dom-invader/prototype-pollution#scanning-for-prototype-pollution-gadgets) - DOM Invader
* Find source that allows adding properties to global prototype
  * `website.com/?__proto__[foo}=bar`
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
