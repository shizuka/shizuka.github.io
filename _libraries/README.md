# Libraries

Files to define a **library**, collection of writing posts

e.g: /writing/**blog**

* * * * *

## Format

Create `_/libraries/libname.html`

`library` and `name` are *required*

~~~
---
# REQUIRED fields
library: libname
name: Friendly Library Name

# Optional fields
desc: short description (e.g. "Articles about stuff and things")
splash: /root/path/to/splash/banner (default: /assets/img/splash_470x80_journal.png)
sortdesc: true for newest-first listing (default: false)
icon: name of `_includes/svg/[icon]` for use on library indexes (default: book)
---
~~~