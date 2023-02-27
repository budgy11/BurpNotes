# 🛕 Server Side Template Injection

* Creds to try if no wiener:peter -> content-manager:C0nt3ntM4n4g3r

### Detect

* Fuzzing with special characters. `${{<%[%'"}}%\` , for any exception
* Also has predefined payloads in intruder for fuzzing

#### Plaintext Context

* `render('Hello ' + username)'`
* Commonly mistaken for XSS
  * Differentiate through math ${7\*7}

#### Code Context

* `greeting = getQueryParameter('greeting')`
* `engine.render("Hello {{"+greeting+"}},data))`
* [http://website.com/?greeting=data.username](http://website.com/?greeeting=data.username)
* Easier to miss, doesn't do XSS
  * Instead leads to blank entries, encoded tags, or an error
* Try to breakout and inject HTML `data.username}}<tag>`

### Identify

* Figure out the template engine
* Try to get an error message i.e `<%=foobar%>` in Ruby - ERB engine
* Decision Tree for some engines

!\[SSTI Template Tree]\([https://github.com/budgy11/BurpNotes/blob/main/.gitbook/assets/template-decision-tree.png](.gitbook/assets/template-decision-tree.png))



#### Exploit

* Read Docs after Identify
  * Security sections
* [https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)
  * READ THOROUGHLY AND DECODE SOME REMOVE FILES BY DEFAULT
* Many template engines expose a "self" or "environment" object of some kind
  * `${T(java.lang.System).getenv()}` in Java
  * `{% debug %}` in django
  * Read into revealed objects in documentation for sensitive information i.e settings.SECRET\_KEY in Django
* Django - [https://lifars.com/wp-content/uploads/2021/06/Django-Templates-Server-Side-Template-Injection-v1.0.pdf](https://lifars.com/wp-content/uploads/2021/06/Django-Templates-Server-Side-Template-Injection-v1.0.pdf)
