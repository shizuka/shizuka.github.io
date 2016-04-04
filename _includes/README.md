# Liquid Includes

Master Partials - present on all pages
 - **mstr_prefix.html** - opening `html` through `div#wrapper`, appends **meta_header**
 - **mstr_postfix.html** - appends **meta_footer**, closes `div#wrapper`, delayed JS load, closes `html`
 
Zone Partials - blocks of stuff on page
 - `div#contentwrapper` must be explicitly opened and closed (for alternate main content formats)
 - **zone_banner.html** - `div.box` with splash banner
 - **zone_banner-path.html** - **zone_banner** with breadcrumbs to category index
 - **zone_banner-postinfo.html** - **zone_banner-path** with post info line (date, tags/collection)
 - **zone_content.html** - `.md` contents, broken into `section`s by `hr`
 - **zone_sidebar.html** - `aside#sidebar` with blurb, contact links, other stuff
 - **zone_libindex.html** - index for `page.library`, default sort date descending, add `sortasc=true`

Meta Partials - included by other partials
 - **meta_header.html** - logo and navigation, included by **mstr_prefix**
 - **meta_footer.html** - end of page, included by **mstr_postfix**
 - **meta_backtotop.html** - top link, included by **zone_content** in each `section`

SVG Partials - `<svg>` images for inline inclusion, *no file extension*
 - **svg/toran** - Vortal Cord Logo, toran icon
 - **svg/sakura** - sakura blossom, Shizuka's icon
 - **svg/book** - open book, from ahyoheek
 - **svg/pen** - ink pen, from ahyoheek
 - **svg/ki** - KI device symbol
