# Libraries

Files to define a **library**, collection of writing posts

e.g: /writing/**blog**/

* * * * *

## Library Definition

Add following to `_config.yml` - `defaults`

~~~
  -
    scope:
      type: writing
      path: "writing/LIBNAME"
    values
      library: LIBNAME
      splash: "/assets/img/splash_470x80_SPLASHNAME.png"
~~~

**NOTE**: Overriding `splash` here will set all pages in this library to that
splash. They can still override it in their frontmatter block.
The index's splash is defined in the metadata file in this folder.

**NOTE**: Files in any folder can set their own library independent of this.

* * * * *

## Library Metadata

Create `_/libraries/libname.html`

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
  - absolute path to image for splash banner at top of index page
  - **NOTE:** Default subpage splashes with the `defaults` block above
  - *default*: `/assets/img/splash_470x80_books.png`
 - **sortdesc**
  - boolean, toggle to sort library's posts in newest-first order
  - *default*: false
 - **icon**
  - name of `_includes/svg/[icon].svg` to use for library icon
  - *default*: book (`{ % include svg/book.svg % }`)
