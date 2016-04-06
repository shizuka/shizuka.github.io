# Liquid Layout Templates

 - **pg_home.html** - Home page, w/ summary of projects and recent n posts/pages
 - **pg_resume.html** - resume page, banner uses special text
 - **pg_root.html** - Root level pages - indexes, banner is just image
 - **pg_subpage.html** - Sublevel pages - banner includes breadcrumbs to index
 - **pg_subpost.html** - Sublevel posts - banner w/ breadcrumbs + postinfo block
 - **raw.html** - straight page content dump, if necessary

* * * * *

## Template Allocation

 - `/`
   - **pg_home**
   - splash: minnehaha falls
   - content:
     - blogroll, reverse chrono list of most recent 3? posts/pages excerpts
     - figure out combining collection pages w/ blog posts
 - `/resume`
   - **pg_resume**
   - splash: full asuka avi pic
   - box text: shizuka kamishima, major titles, ultrashort blurb?
   - content:
     - skills, list + star rating?
     - positions
 - `/projects`
   - **pg_root**
   - splash: code stock photo
 - `/projects/*`
   - **pg_subpage**
   - splash: project-specified
   - box text: breadcrumbs to projects index
   - content:
     - project page contents
 - `/writing`
   - **pg_root**
   - splash: library shelves
   - content:
     - list of categories - blog, tag index, major libraries
     - list of 10 most recent posts/pages by title
 - `/writing/*/` (_libraries)
   - **pg_subpage**
   - splash: gehn's desk (optional per-post / per-library override)
   - box text: breadcrumbs to writing index
   - content:
     - page content
 - `/writing/*/*` (_writing)
   - **pg_subpost**
   - splash: gehn's desk (per-library/post override)
   - box text: breadcrumbs + post info
   - content:
     - post content