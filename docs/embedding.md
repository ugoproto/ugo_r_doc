# Embedding HTML outputs in general

- We can embed just about any HTML snippet with `<iframe>` or `<embed>`. 
- Embedding static visualizations from packages such as `ggplot2`, `ggmap` or `rworldmap`, to name a few, can be done with image outputs (.png, .pdf) or HTML outputs. 
- For htmlwidgets, we can work with HTML outputs, as we demonstrate further down, or with the Shiny server. In both cases, the final results are interactive.
- Some interactive visualization packages, like `ggvis`, require the Shiny server to be fully interactive.
- The HTML document heading:

```
---
output:
  html_document:
    code_folding: hide
---
```

If the figures are generated with `fig.width=5, fig.height=5`; they should fit in a 520x520px box. In some case, we might need more room...

# Embedding outputs from...

## ...the `leaflet` package

```
<iframe seamless src="../img/leaflet_frag.html" width=520px height=520px ></iframe>
```

<iframe seamless src="../img/leaflet_frag.html" width=520px height=520px ></iframe>

```
<embed seamless src="../img/leaflet_frag.html" width=520px height=520px ></embed>
```

<embed seamless src="../img/leaflet_frag.html" width=520px height=520px ></embed>

## ...the `dygraphs` package

```
<embed seamless src="../img/dygraphs_frag.html" width=520px height=520px ></embed>
```

<embed seamless src="../img/dygraphs_frag.html" width=520px height=520px ></embed>

## ...the `plotly` package

```
<embed seamless src="../img/plotly_frag.html" width=520px height=520px ></embed>
```

<embed seamless src="../img/plotly_frag.html" width=520px height=520px ></embed>

## ...the `rbokeh` package

```
<embed seamless src="../img/rbokeh_frag.html" width=540px height=540px ></embed>
```

<embed seamless src="../img/rbokeh_frag.html" width=540px height=540px ></embed>

## ...the `highcharters` package

*Highcharts (www.highcharts.com) is a Highsoft software product which is not free for commercial and Governmental use.*

```
<embed seamless src="../img/highcharters_frag.html" width=600px height=600px ></embed>
```

<embed seamless src="../img/highcharters_frag.html" width=600px height=600px ></embed>

## ...the `datatables` package

```
<embed seamless src="../img/datatable_frag.html" width=700px height=600px ></embed>
```

<embed seamless src="../img/datatable_frag.html" width=700px height=600px ></embed>

---
