---

[TOC]

---

**Foreword**

Notes, snippets, and results.

---

## The `leaflet` package

The [package](http://leafletjs.com/) is an open-source JavaScript library for mobile-friendly interactive maps. We can make interactive panning/zooming and overlay object on maps:

- Map tiles.
- Markers (with custom icons).
- Polygons.
- Lines.
- Popups.
- GeoJSON.
- Choropleth maps.
- Non-geographical maps.
- WMS and TMS (weather and time measurement systems).
- Map panes.
- Videos.

We can embed maps in R Markdown documents and Shiny apps. We can embed maps into websites.

We can render spatial objects from the sp or sf packages, or data frames with latitude/longitude columns.

With `leftlet` grammar, we can build a set of overlays with the `magrittr` pipe operator (`%>%`).

Consult the documentation and the [website](http://leafletjs.com/) for the specifics.

## Embedding Leaflet outputs with `<iframe>` or `<embed>`

```
<iframe seamless src="../leaflet.html" width=600px height=400px ></iframe>
```

<iframe seamless src="../leaflet.html" width=600px height=400px ></iframe>

```
<embed seamless src="../leaflet.html" width=600px height=400px ></embed>
```

<embed seamless src="../leaflet.html" width=600px height=400px ></embed>

## Embedding HTML outputs in general

We can embed just about any HTML snippet.

On the other hand, Leaflet is a JavaScript library. JavaScript should be the prime language for creating maps and embedding them into web sites.

Nonetheless, we can render [htmlwidgets](http://www.htmlwidgets.org/showcase_leaflet.html) (including Leaflet) in any HTML output. We can publish [R Markdown Websites](http://rmarkdown.rstudio.com/rmarkdown_websites.html#additional_examples) or blogs with [blogdown](https://bookdown.org/yihui/blogdown/). We can build a [flexdashboard](http://rmarkdown.rstudio.com/flexdashboard/). A flexdashboard is an alternative to an HTML report or presentation, using other formats than 'pages' or 'slides'. We can publish a book with [bookdown](https://bookdown.org/). And so on.

These tools seamlessly integrate interactive [htmlwidgets](http://www.htmlwidgets.org/showcase_leaflet.html) along with static tabulating, charting, and mapping visualizations (and optionally use [Shiny](https://shiny.rstudio.com/) to drive visualizations dynamically).

---
