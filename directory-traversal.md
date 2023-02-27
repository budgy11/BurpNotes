---
description: Super Bare
---

# 📁 Directory Traversal

* Absolute path
* traversal sequence not stripped recursively `....//`
* Try without original filename at first, it'll start with / or ..

### Predefined wordlists

* Fuzz potential encodings
* Intruder Add From list drop down
  * does NOT detect non-recursive stripping
* Payload processing add
  * Match and replace
    * Match \\{FILE\\}
    * Replace with file (passwd NOT password cmon man)
* Deselect URL-encode these characters
