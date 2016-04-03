# Site Data

## libraries.yml
Defines categories for `writing` articles  
**NOTE:** Requires existence of `/writing/[lib]/index.md`

Data Format:
~~~
libname:
  name: friendly name
  desc: optional description
anotherlib:
  name: another library
~~~

Page Usage:
~~~
---
title: Page Title
library: libname
---

Lorem ipsum dolor sit amet, consectetur adipiscing elit...
~~~

Index Format:
~~~
---
layout: pg_subpage
library: libname
---
{% include zone_libindex.html %}
~~~

* * * * *
