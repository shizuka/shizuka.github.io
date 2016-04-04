# Liquid Includes

Master Partials - present on all pages
 - **mstr_prefix** - opening `html` through `div#wrapper`, appends **meta_header**
 - **mstr_postfix** - appends **meta_footer**, closes `div#wrapper`, delayed JS load, closes `html`
 
Zone Partials - blocks of stuff on page
 - `div#contentwrapper` must be explicitly opened and closed (for alternate main content formats)
 - **zone_banner** - `div.box` with splash banner
 - **zone_banner-path** - **zone_banner** with breadcrumbs to category index
 - **zone_banner-postinfo** - **zone_banner-path** with post info line (date, tags/collection)
 - **zone_content** - `.md` contents, broken into `section`s by `hr`
 - **zone_sidebar** - `aside#sidebar` with blurb, contact links, other stuff
 - **zone_libindex** - index for `page.library`, default sort date descending, add `sortasc=true`

Meta Partials - included by other partials
 - **meta_header** - logo and navigation, included by **mstr_prefix**
 - **meta_footer** - end of page, included by **mstr_postfix**
 - **meta_backtotop** - top link, included by **zone_content** in each `section`

SVG Partials - `<svg>` images for inline inclusion, *no file extension*
 - **svg_toran** - Vortal Cord Logo, toran icon
 - **svg_sakura** - sakura blossom, Shizuka's icon
 - **svg_book** - open book, from ahyoheek
 - **svg_pen** - ink pen, from ahyoheek
 - **svg_ki** - KI device symbol
