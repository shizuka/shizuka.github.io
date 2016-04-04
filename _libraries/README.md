# Libraries

Files to define a **library**, collection of writing posts

e.g: /writing/**blog**/

* * * * *

## Format

`_/libraries/libname.html`

~~~
---
library: libname
name: Library Name
desc: 

# Overrides
splash: /assets/img/splash_470x80_journal.png
sortdesc: false
icon: book
---
~~~

* * * * *

## Fields
### Required
 - **library**
  - identifying slug for library
  - used in url, breadcrumb links
 - **name**
  - friendly name for library
  - used in page title, links
 - **desc**
  - max *50* character tagline (e.g. "Articles about stuff and things")
  - used in master index, library index, *defaults empty*

### Overrides
 - **splash**
  - absolute path to image for splash banner at top of page
  - *default*: `/assets/img/splash_470x80_journal.png`
 - **sortdesc**
  - boolean, toggle to sort library's posts in newest-first order
  - *default*: false
 - **icon**
  - name of `_includes/svg/*` to use for library icon
  - *default*: book (`{ % include svg/book % }`)
