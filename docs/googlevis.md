# The `googleVis` package

The package provides an interface to Google's chart tools, allowing users to create interactive charts based on data frames. It included maps.

The interactive maps are displayed in a browser. We can plot a complete set of interactive graphs and embed them into a web page.

Some motion charts cannot be displays in tablets and mobile phones (using HTML5) because they are rendered with Flash; Flash has to be installed on a PC.

- [Examples](https://cran.r-project.org/web/packages/googleVis/vignettes/googleVis_examples.html).
	- Charts: line, bar, column, area, stepped area, combo, scatter, bubble, customizing, candlestick (or boxplot), pie, gauge, annotation, Sankey, histogram, and motion (GapMinder-like)
	- Maps: intensity, geo, choropleth, marker, Google Maps.
	- Table, organizational chart, tree map, calendar, timeline, merging.
	- Motion charts and some maps only work in Flash, not in HTML5 as with tablets and mobile phones).
- [Gallery](https://developers.google.com/chart/interactive/docs/gallery).
- [Documentation](https://developers.google.com/chart/).
	- As above.
- [Introduction](https://cran.r-project.org/web/packages/googleVis/vignettes/googleVis.pdf).
- [Roles](https://cran.r-project.org/web/packages/googleVis/vignettes/Using_Roles_via_googleVis.html).
- [Trendlines](https://cran.r-project.org/web/packages/googleVis/vignettes/Using_Trendlines_with_googleVis.html).
- [Markdown](https://cran.r-project.org/web/packages/googleVis/vignettes/Using_googleVis_with_knitr.html).
- In R, run a demo with `demo(googleVis)`.

Always cite the package:

```r
citation("googleVis")
```

# Suppress the message

We normally load the library...

```r
library(googleVis)
```

...but we can also suppress the message when loading the package.

```r
suppressPackageStartupMessages(library(googleVis))
```

# Printing

- Generate the chart.

```r
library(googleVis)

head(Fruits, 3)

M <- gvisMotionChart(Fruits,
                     idvar='Fruit',
                     timevar='Year',
                     options=list(width=400, height=350))

# xvar =
# yvar =
# colorvar =
# sizevar =
# date.format =
```

- Emulate the chart...

```r
# In the browser, with a tag underneath
plot(M)

# In the console (code only), with a tag underneath
M

# Header part
# Basic html and formatting tags
print(M, tag='header')

# Actual Google visualization code
# Can be copy-paste in a markdown or html document
print(M, tag='chart')

# Header + visualization code = what we see in the browser

# Components of the chart
print(M, tag='jsChart')

# Basic chart caption and html footer (what is underneath)
print(M, tag='caption')

# Save it locally
print(M, file="GoogleVis/M.html")
# Or
#cat(M$html$chart, file = "GoogleVis/M.html")
```

# Embedding a chart/map into a static website

The chart can be standing alone as an image file; even be embedded within a text.

- In RStudio, set the working directory with `setwd(' ')`.
- Set the option to print the output in a html file.

```r
{r, message=FALSE}
library(googleVis)

op <- options(gvis.plot.tag='chart')
```

- Set options back to original options.

```r
options(op)
```

- Generate the chart.

```r
library(googleVis)

# Set the option to print here (alternative)
options(gvis.plot.tag='chart')

# Create the chart
M <- gvisMotionChart(Fruits,
                     idvar='Fruit',
                     timevar='Year',
                     options=list(width=400, height=350))

# Do not print the chart in the browser
#plot(M) # in the browser

# Print the chart as a html file
cat(M$html$chart, file = "M.html")
```

- The file has several parts: JavaScript and HTML. The part that displays the chart is in the end.

```html
<!-- divChart -->
  
<div id="MotionChartID2cd02805862d" 
  	 style="width: 400; height: 350;">
</div>
```

- Following this bullet, in the Markdown document, paste the M.html code. First, open the HTML file to copy the code.
- Modify the markup language with more styling parameters.

```html
<div id="MotionChartID2cd02805862d" 
style="width: 400; height: 350; float:left;";>
</div>
```

- The HTML snippet must follow the JavaScript snippet in the document. Move the HTML snippet within a text (the text can separate both snippets). 

```html
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
<div id="MotionChartID2cd02805862d" 
style="width: 400; height: 350; float:left;";>
</div>
Duis aute irure dolor...
```

Here is the result:

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataMotionChartID2cd02805862d () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Apples",
2008,
"West",
98,
78,
20,
"2008-12-31"
],
[
"Apples",
2009,
"West",
111,
79,
32,
"2009-12-31"
],
[
"Apples",
2010,
"West",
89,
76,
13,
"2010-12-31"
],
[
"Oranges",
2008,
"East",
96,
81,
15,
"2008-12-31"
],
[
"Bananas",
2008,
"East",
85,
76,
9,
"2008-12-31"
],
[
"Oranges",
2009,
"East",
93,
80,
13,
"2009-12-31"
],
[
"Bananas",
2009,
"East",
94,
78,
16,
"2009-12-31"
],
[
"Oranges",
2010,
"East",
98,
91,
7,
"2010-12-31"
],
[
"Bananas",
2010,
"East",
81,
71,
10,
"2010-12-31"
] 
];
data.addColumn('string','Fruit');
data.addColumn('number','Year');
data.addColumn('string','Location');
data.addColumn('number','Sales');
data.addColumn('number','Expenses');
data.addColumn('number','Profit');
data.addColumn('string','Date');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartMotionChartID2cd02805862d() {
var data = gvisDataMotionChartID2cd02805862d();
var options = {};
options["width"] = 400;
options["height"] = 350;
options["state"] = "";


    var chart = new google.visualization.MotionChart(
    document.getElementById('MotionChartID2cd02805862d')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "motionchart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartMotionChartID2cd02805862d);
})();
function displayChartMotionChartID2cd02805862d() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartMotionChartID2cd02805862d"></script>
 
<!-- divChart -->
  
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.

<div id="MotionChartID2cd02805862d" 
style="width: 400; height: 350; float:left;";>
</div>

Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum. Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. Nemo enim ipsam voluptatem quia voluptas sit aspernatur aut odit aut fugit, sed quia consequuntur magni dolores eos qui ratione voluptatem sequi nesciunt. Neque porro quisquam est, qui dolorem ipsum quia dolor sit amet, consectetur, adipisci velit, sed quia non numquam eius modi tempora incidunt ut labore et dolore magnam aliquam quaerat voluptatem.

# Other static websites

There is a procedure for embedding graphics in:

- WordPress.
- Google Sites.
- Blogger.
- Google Code wiki pages.
- Wikipedia.
- others websites.

# Dynamic websites

- The `R.rsp` package allows the integration of R code into html code. 
- The `rApache` and `brew` packages support web application development using R and the Apache HTTP server.
- Rook is a lightweight web server interface for R.
- The `shiny` package builds interactive web application with R.

# Charts

Consult the examples (further above); charts are similar to what we find in other packages. 

`gvisMotionChart` is exclusive to `googleVis`.

## `gvisMotionChart`

<!--  -->
<!-- M -->
<!--  -->

```r
head(Fruits, 3)
```

```
# Output
   Fruit Year Location Sales Expenses Profit       Date
1 Apples 2008     West    98       78     20 2008-12-31
2 Apples 2009     West   111       79     32 2009-12-31
3 Apples 2010     West    89       76     13 2010-12-31
```

```r
M <- gvisMotionChart(Fruits,
                     idvar='Fruit',
                     timevar='Year',
                     options=list(width=400, height=350))

# xvar =
# yvar =
# colorvar =
# sizevar =
# date.format =

plot(M)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataMotionChartID336e668836ab () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Apples",
2008,
"West",
98,
78,
20,
"2008-12-31"
],
[
"Apples",
2009,
"West",
111,
79,
32,
"2009-12-31"
],
[
"Apples",
2010,
"West",
89,
76,
13,
"2010-12-31"
],
[
"Oranges",
2008,
"East",
96,
81,
15,
"2008-12-31"
],
[
"Bananas",
2008,
"East",
85,
76,
9,
"2008-12-31"
],
[
"Oranges",
2009,
"East",
93,
80,
13,
"2009-12-31"
],
[
"Bananas",
2009,
"East",
94,
78,
16,
"2009-12-31"
],
[
"Oranges",
2010,
"East",
98,
91,
7,
"2010-12-31"
],
[
"Bananas",
2010,
"East",
81,
71,
10,
"2010-12-31"
] 
];
data.addColumn('string','Fruit');
data.addColumn('number','Year');
data.addColumn('string','Location');
data.addColumn('number','Sales');
data.addColumn('number','Expenses');
data.addColumn('number','Profit');
data.addColumn('string','Date');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartMotionChartID336e668836ab() {
var data = gvisDataMotionChartID336e668836ab();
var options = {};
options["width"] = 400;
options["height"] = 350;
options["state"] = "";


    var chart = new google.visualization.MotionChart(
    document.getElementById('MotionChartID336e668836ab')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "motionchart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartMotionChartID336e668836ab);
})();
function displayChartMotionChartID336e668836ab() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartMotionChartID336e668836ab"></script>
 
<!-- divChart -->
  
<div id="MotionChartID336e668836ab" 
  style="width: 400; height: 350;">
</div>
<br>

## Embedding with `<iframe>` or `<embed>`

After we generate the HTML chart or map, save the object as a HTML file.

```r
library(htmlwidgets) 
library(DT) 

saveWidget(object, "googleVis.html")
```

Add these HTML snippets to the Markdown/HTML document.

```html
<iframe seamless src="../img/googlevis.html" width=600px 
height=400px ></iframe>
```

<iframe seamless src="../img/googlevis_m.html" width=600px 
height=400px ></iframe>

```html
<embed seamless src="../img/googlevis.html" width=600px height=400px ></embed>
```

<embed seamless src="../img/googlevis_m.html" width=600px height=400px >

In other words, we can either write the code or embed a file into the HTML/Markdown document.

## Information from the object

```r
M$type
M$chartid
```

```
"MotionChart"
"MotionChartID336e668836ab"
```

# Maps

## `gvisGeoChart`

<!--  -->
<!-- Geo -->
<!--  -->

```r
head(Exports, 3)
```

```
# Output
        Country Profit Online
1       Germany      3   TRUE
2        Brazil      4  FALSE
3 United States      5   TRUE
```

```r
Geo <- gvisGeoChart(Exports, 
                    locationvar = "Country",
                    colorvar = "Profit",
                    sizevar = "", # size of markers
                    hovervar = "", # text
                    options = list(projection="kavrayskiy-vii"))

# locationvar can be lat:long or address, country name, region, state, city
                    
plot(Geo)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID49617fef373 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Germany",
3
],
[
"Brazil",
4
],
[
"United States",
5
],
[
"France",
4
],
[
"Hungary",
3
],
[
"India",
2
],
[
"Iceland",
1
],
[
"Norway",
4
],
[
"Spain",
5
],
[
"Turkey",
1
] 
];
data.addColumn('string','Country');
data.addColumn('number','Profit');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID49617fef373() {
var data = gvisDataGeoChartID49617fef373();
var options = {};
options["width"] = 556;
options["height"] = 347;
options["projection"] = "kavrayskiy-vii";


    var chart = new google.visualization.GeoChart(
    document.getElementById('GeoChartID49617fef373')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geochart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID49617fef373);
})();
function displayChartGeoChartID49617fef373() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID49617fef373"></script>
 
<!-- divChart -->
  
<div id="GeoChartID49617fef373" 
  style="width: 556; height: 347;">
</div>

<!--  -->
<!-- Geo2 -->
<!--  -->

```r
head(CityPopularity, 3)
```

```
# Output
      City Popularity
1 New York        200
2   Boston        300
3    Miami        400
```

```r
Geo2 <- gvisGeoChart(CityPopularity, 
                     locationvar='City',
                     colorvar='Popularity',
                     options=list(region='US',
                                  height=350,
                                  displayMode='markers', 
                                  colorAxis="{values:[200,400,600,800], colors:[\'red', \'pink\', \'orange',\'green']}"))
                                    
plot(Geo2)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID4961741b9385 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"New York",
200
],
[
"Boston",
300
],
[
"Miami",
400
],
[
"Chicago",
500
],
[
"Los Angeles",
600
],
[
"Houston",
700
] 
];
data.addColumn('string','City');
data.addColumn('number','Popularity');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID4961741b9385() {
var data = gvisDataGeoChartID4961741b9385();
var options = {};
options["width"] = 556;
options["height"] = 350;
options["region"] = "US";
options["displayMode"] = "markers";
options["colorAxis"] = {values:[200,400,600,800], colors:['red', 'pink', 'orange','green']};


    var chart = new google.visualization.GeoChart(
    document.getElementById('GeoChartID4961741b9385')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geochart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID4961741b9385);
})();
function displayChartGeoChartID4961741b9385() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID4961741b9385"></script>
 
<!-- divChart -->
  
<div id="GeoChartID4961741b9385" 
  style="width: 556; height: 350;">
</div>

<!--  -->
<!-- Geo3 -->
<!--  -->

```r
CityPopularity3 <- data.frame(City = c('Montreal', 'Toronto'),
                              Popularity = c(400, 200))

Geo3 <- gvisGeoChart(CityPopularity3, 
                     locationvar='City',
                     colorvar='Popularity',
                     options=list(region = 'CA',
                                  height=350,
                                  displayMode='markers', 
                                  colorAxis="{values:[200,400,600,800], colors:[\'red', \'pink\', \'orange',\'green']}"))
                                    
plot(Geo3)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID49614f24acff () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Montreal",
700
],
[
"Toronto",
200
] 
];
data.addColumn('string','City');
data.addColumn('number','Popularity');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID49614f24acff() {
var data = gvisDataGeoChartID49614f24acff();
var options = {};
options["width"] = 556;
options["height"] = 350;
options["region"] = "CA";
options["displayMode"] = "markers";
options["colorAxis"] = {values:[200,400,600,800], colors:['red', 'pink', 'orange','green']};


    var chart = new google.visualization.GeoChart(
    document.getElementById('GeoChartID49614f24acff')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geochart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID49614f24acff);
})();
function displayChartGeoChartID49614f24acff() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID49614f24acff"></script>
 
<!-- divChart -->
  
<div id="GeoChartID49614f24acff" 
  style="width: 556; height: 350;">
</div>

<!--  -->
<!-- GeoMarker -->
<!--  -->

```r
head(Andrew, 3)
```

```
# Output
        Date/Time UTC  Lat  Long Pressure_mb Speed_kt            Category
1 1992-08-16 18:00:00 10.8 -35.5        1010       25 Tropical Depression
2 1992-08-17 00:00:00 11.2 -37.4        1009       30 Tropical Depression
3 1992-08-17 06:00:00 11.7 -39.6        1008       30 Tropical Depression
     LatLong                                              Tip
1 10.8:-35.5 Tropical Depression<BR>Pressure=1010<BR>Speed=25
2 11.2:-37.4 Tropical Depression<BR>Pressure=1009<BR>Speed=30
3 11.7:-39.6 Tropical Depression<BR>Pressure=1008<BR>Speed=30
```

```r
GeoMarker <- gvisGeoChart(Andrew,
                          locationvar="LatLong", 
                          sizevar='Speed_kt',
                          colorvar="Pressure_mb", 
                          options=list(region="US"))
                          
plot(GeoMarker)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID2cd02f295f7 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
10.8,
-35.5,
1010,
25
],
[
11.2,
-37.4,
1009,
30
],
[
11.7,
-39.6,
1008,
30
],
[
12.3,
-42,
1006,
35
],
[
13.1,
-44.2,
1003,
35
],
[
13.6,
-46.2,
1002,
40
],
[
14.1,
-48,
1001,
45
],
[
14.6,
-49.9,
1000,
45
],
[
15.4,
-51.8,
1000,
45
],
[
16.3,
-53.5,
1001,
45
],
[
17.2,
-55.3,
1002,
45
],
[
18,
-56.9,
1005,
45
],
[
18.8,
-58.3,
1007,
45
],
[
19.8,
-59.3,
1011,
40
],
[
20.7,
-60,
1013,
40
],
[
21.7,
-60.7,
1015,
40
],
[
22.5,
-61.5,
1014,
40
],
[
23.2,
-62.4,
1014,
45
],
[
23.9,
-63.3,
1010,
45
],
[
24.4,
-64.2,
1007,
50
],
[
24.8,
-64.9,
1004,
50
],
[
25.3,
-65.9,
1000,
55
],
[
25.6,
-67,
994,
60
],
[
25.8,
-68.3,
981,
70
],
[
25.7,
-69.7,
969,
80
],
[
25.6,
-71.1,
961,
90
],
[
25.5,
-72.5,
947,
105
],
[
25.4,
-74.2,
933,
120
],
[
25.4,
-75.8,
922,
135
],
[
25.4,
-77.5,
930,
125
],
[
25.4,
-79.3,
937,
120
],
[
25.6,
-81.2,
951,
110
],
[
25.8,
-83.1,
947,
115
],
[
26.2,
-85,
943,
115
],
[
26.6,
-86.7,
948,
115
],
[
27.2,
-88.2,
946,
115
],
[
27.8,
-89.6,
941,
120
],
[
28.5,
-90.5,
937,
120
],
[
29.2,
-91.3,
955,
115
],
[
30.1,
-91.7,
973,
80
],
[
30.9,
-91.6,
991,
50
],
[
31.5,
-91.1,
995,
35
],
[
32.1,
-90.5,
997,
30
],
[
32.8,
-89.6,
998,
30
],
[
33.6,
-88.4,
999,
25
],
[
34.4,
-86.7,
1000,
20
],
[
35.4,
-84,
1000,
20
] 
];
data.addColumn('number','Latitude');
data.addColumn('number','Longitude');
data.addColumn('number','Pressure_mb');
data.addColumn('number','Speed_kt');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID2cd02f295f7() {
var data = gvisDataGeoChartID2cd02f295f7();
var options = {};
options["width"] = 556;
options["height"] = 347;
options["region"] = "US";


    var chart = new google.visualization.GeoChart(
    document.getElementById('GeoChartID2cd02f295f7')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geochart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID2cd02f295f7);
})();
function displayChartGeoChartID2cd02f295f7() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID2cd02f295f7"></script>
 
<!-- divChart -->
  
<div id="GeoChartID2cd02f295f7" 
  style="width: 556; height: 347;">
</div>
<br>

## `gvisMap` or Google Maps

<!--  -->
<!-- AndrewMap -->
<!--  -->

```r
AndrewMap <- gvisMap(Andrew,
                     locationvar="LatLong" ,
                     tipvar="Tip", #  text displayed over the tip icon
                     options=list(showTip=TRUE, 
                                  showLine=TRUE, 
                                  enableScrollWheel=TRUE,
                                  mapType='terrain', 
                                  useMapTypeControl=TRUE))
                                                                   
plot(AndrewMap)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataMapID496129f7ada () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
10.8,
-35.5,
"Tropical Depression<BR>Pressure=1010<BR>Speed=25"
],
[
11.2,
-37.4,
"Tropical Depression<BR>Pressure=1009<BR>Speed=30"
],
[
11.7,
-39.6,
"Tropical Depression<BR>Pressure=1008<BR>Speed=30"
],
[
12.3,
-42,
"Tropical Storm<BR>Pressure=1006<BR>Speed=35"
],
[
13.1,
-44.2,
"Tropical Storm<BR>Pressure=1003<BR>Speed=35"
],
[
13.6,
-46.2,
"Tropical Storm<BR>Pressure=1002<BR>Speed=40"
],
[
14.1,
-48,
"Tropical Storm<BR>Pressure=1001<BR>Speed=45"
],
[
14.6,
-49.9,
"Tropical Storm<BR>Pressure=1000<BR>Speed=45"
],
[
15.4,
-51.8,
"Tropical Storm<BR>Pressure=1000<BR>Speed=45"
],
[
16.3,
-53.5,
"Tropical Storm<BR>Pressure=1001<BR>Speed=45"
],
[
17.2,
-55.3,
"Tropical Storm<BR>Pressure=1002<BR>Speed=45"
],
[
18,
-56.9,
"Tropical Storm<BR>Pressure=1005<BR>Speed=45"
],
[
18.8,
-58.3,
"Tropical Storm<BR>Pressure=1007<BR>Speed=45"
],
[
19.8,
-59.3,
"Tropical Storm<BR>Pressure=1011<BR>Speed=40"
],
[
20.7,
-60,
"Tropical Storm<BR>Pressure=1013<BR>Speed=40"
],
[
21.7,
-60.7,
"Tropical Storm<BR>Pressure=1015<BR>Speed=40"
],
[
22.5,
-61.5,
"Tropical Storm<BR>Pressure=1014<BR>Speed=40"
],
[
23.2,
-62.4,
"Tropical Storm<BR>Pressure=1014<BR>Speed=45"
],
[
23.9,
-63.3,
"Tropical Storm<BR>Pressure=1010<BR>Speed=45"
],
[
24.4,
-64.2,
"Tropical Storm<BR>Pressure=1007<BR>Speed=50"
],
[
24.8,
-64.9,
"Tropical Storm<BR>Pressure=1004<BR>Speed=50"
],
[
25.3,
-65.9,
"Tropical Storm<BR>Pressure=1000<BR>Speed=55"
],
[
25.6,
-67,
"Tropical Storm<BR>Pressure=994<BR>Speed=60"
],
[
25.8,
-68.3,
"Hurricane<BR>Pressure=981<BR>Speed=70"
],
[
25.7,
-69.7,
"Hurricane<BR>Pressure=969<BR>Speed=80"
],
[
25.6,
-71.1,
"Hurricane<BR>Pressure=961<BR>Speed=90"
],
[
25.5,
-72.5,
"Hurricane<BR>Pressure=947<BR>Speed=105"
],
[
25.4,
-74.2,
"Hurricane<BR>Pressure=933<BR>Speed=120"
],
[
25.4,
-75.8,
"Hurricane<BR>Pressure=922<BR>Speed=135"
],
[
25.4,
-77.5,
"Hurricane<BR>Pressure=930<BR>Speed=125"
],
[
25.4,
-79.3,
"Hurricane<BR>Pressure=937<BR>Speed=120"
],
[
25.6,
-81.2,
"Hurricane<BR>Pressure=951<BR>Speed=110"
],
[
25.8,
-83.1,
"Hurricane<BR>Pressure=947<BR>Speed=115"
],
[
26.2,
-85,
"Hurricane<BR>Pressure=943<BR>Speed=115"
],
[
26.6,
-86.7,
"Hurricane<BR>Pressure=948<BR>Speed=115"
],
[
27.2,
-88.2,
"Hurricane<BR>Pressure=946<BR>Speed=115"
],
[
27.8,
-89.6,
"Hurricane<BR>Pressure=941<BR>Speed=120"
],
[
28.5,
-90.5,
"Hurricane<BR>Pressure=937<BR>Speed=120"
],
[
29.2,
-91.3,
"Hurricane<BR>Pressure=955<BR>Speed=115"
],
[
30.1,
-91.7,
"Tropical Storm<BR>Pressure=973<BR>Speed=80"
],
[
30.9,
-91.6,
"Tropical Storm<BR>Pressure=991<BR>Speed=50"
],
[
31.5,
-91.1,
"Tropical Depression<BR>Pressure=995<BR>Speed=35"
],
[
32.1,
-90.5,
"Tropical Depression<BR>Pressure=997<BR>Speed=30"
],
[
32.8,
-89.6,
"Tropical Depression<BR>Pressure=998<BR>Speed=30"
],
[
33.6,
-88.4,
"Tropical Depression<BR>Pressure=999<BR>Speed=25"
],
[
34.4,
-86.7,
"Tropical Depression<BR>Pressure=1000<BR>Speed=20"
],
[
35.4,
-84,
"Tropical Depression<BR>Pressure=1000<BR>Speed=20"
] 
];
data.addColumn('number','Latitude');
data.addColumn('number','Longitude');
data.addColumn('string','Tip');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartMapID496129f7ada() {
var data = gvisDataMapID496129f7ada();
var options = {};
options["showTip"] = true;
options["showLine"] = true;
options["enableScrollWheel"] = true;
options["mapType"] = "terrain";
options["useMapTypeControl"] = true;


    var chart = new google.visualization.Map(
    document.getElementById('MapID496129f7ada')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "map";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartMapID496129f7ada);
})();
function displayChartMapID496129f7ada() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartMapID496129f7ada"></script>
 
<!-- divChart -->
  
<div id="MapID496129f7ada" 
  style="width: 500; height: automatic;">
</div>

## Editor

This is a fantastic option that let us start with one chart or map and change everything with a menu.

<!--  -->
<!-- Editor -->
<!--  -->

```r
Editor <- gvisGeoChart(Exports, 
                       locationvar="Country",
                       colorvar="Profit",
                       options=list(gvis.editor='Edit me!'))
                       
plot(Editor)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID49613aa5c54a () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Germany",
3
],
[
"Brazil",
4
],
[
"United States",
5
],
[
"France",
4
],
[
"Hungary",
3
],
[
"India",
2
],
[
"Iceland",
1
],
[
"Norway",
4
],
[
"Spain",
5
],
[
"Turkey",
1
] 
];
data.addColumn('string','Country');
data.addColumn('number','Profit');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID49613aa5c54a() {
var data = gvisDataGeoChartID49613aa5c54a();
var options = {};
options["width"] = 556;
options["height"] = 347;


    chartGeoChartID49613aa5c54a = new google.visualization.ChartWrapper({
    dataTable: data,       
    chartType: 'GeoChart',
    containerId: 'GeoChartID49613aa5c54a',
    options: options
    });
    chartGeoChartID49613aa5c54a.draw();
    

}

  function openEditorGeoChartID49613aa5c54a() {
  var editor = new google.visualization.ChartEditor();
  google.visualization.events.addListener(editor, 'ok',
  function() { 
  chartGeoChartID49613aa5c54a = editor.getChartWrapper();  
  chartGeoChartID49613aa5c54a.draw(document.getElementById('GeoChartID49613aa5c54a')); 
  }); 
  editor.openDialog(chartGeoChartID49613aa5c54a);
  }
    
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "charteditor";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID49613aa5c54a);
})();
function displayChartGeoChartID49613aa5c54a() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID49613aa5c54a"></script>
 
<!-- divChart -->
<input type='button' onclick='openEditorGeoChartID49613aa5c54a()' value='Edit me!'/>  
<div id="GeoChartID49613aa5c54a" 
  style="width: 556; height: 347;">
</div>
<br>

# Tables with `gvisTable`

<!--  -->
<!-- Table -->
<!--  -->

```r
head(Stock, 3)
```

```
# Output
        Date  Device Value Title Annotation
1 2008-01-01 Pencils  3000  <NA>       <NA>
2 2008-01-02 Pencils 14045  <NA>       <NA>
3 2008-01-03 Pencils  5502  <NA>       <NA>
```

```r
Table <- gvisTable(Stock, 
                   formats=list(Value="#,###"))

plot(Table)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataTableID2cd03b735929 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
new Date(2008,0,1),
"Pencils",
3000,
null,
null
],
[
new Date(2008,0,2),
"Pencils",
14045,
null,
null
],
[
new Date(2008,0,3),
"Pencils",
5502,
null,
null
],
[
new Date(2008,0,4),
"Pencils",
75284,
null,
null
],
[
new Date(2008,0,5),
"Pencils",
41476,
"Bought pencils",
"Bought 200k pencils"
],
[
new Date(2008,0,6),
"Pencils",
333222,
null,
null
],
[
new Date(2008,0,1),
"Pens",
40645,
null,
null
],
[
new Date(2008,0,2),
"Pens",
20374,
null,
null
],
[
new Date(2008,0,3),
"Pens",
50766,
null,
null
],
[
new Date(2008,0,4),
"Pens",
14334,
"Out of stock",
"Ran out of stock of pens at 4pm"
],
[
new Date(2008,0,5),
"Pens",
66467,
null,
null
],
[
new Date(2008,0,6),
"Pens",
39463,
null,
null
] 
];
data.addColumn('date','Date');
data.addColumn('string','Device');
data.addColumn('number','Value');
data.addColumn('string','Title');
data.addColumn('string','Annotation');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartTableID2cd03b735929() {
var data = gvisDataTableID2cd03b735929();
var options = {};
options["allowHtml"] = true;

  var dataFormat1 = new google.visualization.NumberFormat({pattern:"#,###"});
  dataFormat1.format(data, 2);

    var chart = new google.visualization.Table(
    document.getElementById('TableID2cd03b735929')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "table";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartTableID2cd03b735929);
})();
function displayChartTableID2cd03b735929() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartTableID2cd03b735929"></script>
 
<!-- divChart -->
  
<div id="TableID2cd03b735929" 
  style="width: 500; height: automatic;">
</div>
<br>

<!--  -->
<!-- PopTable -->
<!--  -->

```r
head(Population, 3)
```

```
# Output
                                                                                 Rank       Country Population % of World Population
1    1         China 1339940000                0.1950
2    2         India 1188650000                0.1730
3    3 United States  310438000                0.0452
                                                                                                                                                                     Flag
1 <img src="http://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Flag_of_the_People%27s_Republic_of_China.svg/22px-Flag_of_the_People%27s_Republic_of_China.svg.png">
2                                                       <img src="http://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Flag_of_India.svg/22px-Flag_of_India.svg.png">
3                               <img src="http://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Flag_of_the_United_States.svg/22px-Flag_of_the_United_States.svg.png">
  Mode       Date
1 TRUE 2010-10-09
2 TRUE 2010-10-09
3 TRUE 2010-10-09
```

```r
PopTable <- gvisTable(Population, 
                      formats=list(Population="#,###",
                                   '% of World Population'='#.#%'),
                      options=list(page='enable'))

plot(PopTable)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataTableID2cd013050362 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"1",
"China",
1339940000,
0.195,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Flag_of_the_People%27s_Republic_of_China.svg/22px-Flag_of_the_People%27s_Republic_of_China.svg.png\">",
true,
new Date(2010,9,9)
],
[
"2",
"India",
1188650000,
0.173,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Flag_of_India.svg/22px-Flag_of_India.svg.png\">",
true,
new Date(2010,9,9)
],
[
"3",
"United States",
310438000,
0.0452,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/a4/Flag_of_the_United_States.svg/22px-Flag_of_the_United_States.svg.png\">",
true,
new Date(2010,9,9)
],
[
"4",
"Indonesia",
237556363,
0.0346,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Flag_of_Indonesia.svg/22px-Flag_of_Indonesia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"5",
"Brazil",
193626000,
0.0282,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Flag_of_Brazil.svg/22px-Flag_of_Brazil.svg.png\">",
true,
new Date(2010,9,9)
],
[
"6",
"Pakistan",
170745000,
0.0248,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/32/Flag_of_Pakistan.svg/22px-Flag_of_Pakistan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"7",
"Bangladesh",
164425000,
0.0239,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f9/Flag_of_Bangladesh.svg/22px-Flag_of_Bangladesh.svg.png\">",
true,
new Date(2010,9,9)
],
[
"8",
"Nigeria",
158259000,
0.023,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/79/Flag_of_Nigeria.svg/22px-Flag_of_Nigeria.svg.png\">",
true,
new Date(2010,9,9)
],
[
"9",
"Russia",
141927297,
0.0206,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/Flag_of_Russia.svg/22px-Flag_of_Russia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"10",
"Japan",
127390000,
0.0185,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9e/Flag_of_Japan.svg/22px-Flag_of_Japan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"11",
"Mexico",
108396211,
0.0158,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fc/Flag_of_Mexico.svg/22px-Flag_of_Mexico.svg.png\">",
true,
new Date(2010,9,9)
],
[
"12",
"Philippines",
94013200,
0.0137,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Flag_of_the_Philippines.svg/22px-Flag_of_the_Philippines.svg.png\">",
true,
new Date(2010,9,9)
],
[
"13",
"Vietnam",
85846997,
0.0125,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Flag_of_Vietnam.svg/22px-Flag_of_Vietnam.svg.png\">",
true,
new Date(2010,9,9)
],
[
"14",
"Ethiopia",
84976000,
0.0124,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Flag_of_Ethiopia.svg/22px-Flag_of_Ethiopia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"15",
"Germany",
81802257,
0.0119,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/Flag_of_Germany.svg/22px-Flag_of_Germany.svg.png\">",
true,
new Date(2010,9,9)
],
[
"16",
"Egypt",
79135000,
0.0115,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fe/Flag_of_Egypt.svg/22px-Flag_of_Egypt.svg.png\">",
true,
new Date(2010,9,9)
],
[
"17",
"Iran",
75078000,
0.0109,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/Flag_of_Iran.svg/22px-Flag_of_Iran.svg.png\">",
true,
new Date(2010,9,9)
],
[
"18",
"Turkey",
72561312,
0.0106,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/b4/Flag_of_Turkey.svg/22px-Flag_of_Turkey.svg.png\">",
true,
new Date(2010,9,9)
],
[
"19",
"Dem. Rep. of Congo",
67827000,
0.0099,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Flag_of_the_Democratic_Republic_of_the_Congo.svg/22px-Flag_of_the_Democratic_Republic_of_the_Congo.svg.png\">",
true,
new Date(2010,9,9)
],
[
"20",
"Thailand",
67070000,
0.01,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/a9/Flag_of_Thailand.svg/22px-Flag_of_Thailand.svg.png\">",
true,
new Date(2010,9,9)
],
[
"21",
"France",
65447374,
0.0095,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/c3/Flag_of_France.svg/22px-Flag_of_France.svg.png\">",
true,
new Date(2010,9,9)
],
[
"22",
"United Kingdom",
62008049,
0.009,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Flag_of_the_United_Kingdom.svg/22px-Flag_of_the_United_Kingdom.svg.png\">",
true,
new Date(2010,9,9)
],
[
"23",
"Italy",
60402499,
0.0088,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Flag_of_Italy.svg/22px-Flag_of_Italy.svg.png\">",
true,
new Date(2010,9,9)
],
[
"24",
"Myanmar",
50496000,
0.0073,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/8c/Flag_of_Myanmar.svg/22px-Flag_of_Myanmar.svg.png\">",
true,
new Date(2010,9,9)
],
[
"25",
"South Africa",
49991300,
0.0073,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/af/Flag_of_South_Africa.svg/22px-Flag_of_South_Africa.svg.png\">",
true,
new Date(2010,9,9)
],
[
"26",
"South Korea",
49773145,
0.0072,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/09/Flag_of_South_Korea.svg/22px-Flag_of_South_Korea.svg.png\">",
true,
new Date(2010,9,9)
],
[
"27",
"Spain",
46072834,
0.0067,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Flag_of_Spain.svg/22px-Flag_of_Spain.svg.png\">",
true,
new Date(2010,9,9)
],
[
"28",
"Ukraine",
45871738,
0.0067,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/49/Flag_of_Ukraine.svg/22px-Flag_of_Ukraine.svg.png\">",
true,
new Date(2010,9,9)
],
[
"29",
"Colombia",
45655000,
0.0066,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/21/Flag_of_Colombia.svg/22px-Flag_of_Colombia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"30",
"Tanzania",
45040000,
0.0066,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Flag_of_Tanzania.svg/22px-Flag_of_Tanzania.svg.png\">",
true,
new Date(2010,9,9)
],
[
"31",
"Sudan",
43192000,
0.0063,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/01/Flag_of_Sudan.svg/22px-Flag_of_Sudan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"32",
"Argentina",
40518951,
0.0059,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/1a/Flag_of_Argentina.svg/22px-Flag_of_Argentina.svg.png\">",
true,
new Date(2010,9,9)
],
[
"33",
"Kenya",
38610097,
0.0056,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/49/Flag_of_Kenya.svg/22px-Flag_of_Kenya.svg.png\">",
true,
new Date(2010,9,9)
],
[
"34",
"Poland",
38167329,
0.0056,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Flag_of_Poland.svg/22px-Flag_of_Poland.svg.png\">",
true,
new Date(2010,9,9)
],
[
"35",
"Algeria",
35423000,
0.0052,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/77/Flag_of_Algeria.svg/22px-Flag_of_Algeria.svg.png\">",
true,
new Date(2010,9,9)
],
[
"36",
"Canada",
34272000,
0.005,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Flag_of_Canada.svg/22px-Flag_of_Canada.svg.png\">",
true,
new Date(2010,9,9)
],
[
"37",
"Uganda",
33796000,
0.0049,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4e/Flag_of_Uganda.svg/22px-Flag_of_Uganda.svg.png\">",
true,
new Date(2010,9,9)
],
[
"38",
"Morocco",
31944000,
0.0046,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/2c/Flag_of_Morocco.svg/22px-Flag_of_Morocco.svg.png\">",
true,
new Date(2010,9,9)
],
[
"39",
"Iraq",
31467000,
0.0046,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f6/Flag_of_Iraq.svg/22px-Flag_of_Iraq.svg.png\">",
true,
new Date(2010,9,9)
],
[
"40",
"Nepal",
29853000,
0.0043,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9b/Flag_of_Nepal.svg/16px-Flag_of_Nepal.svg.png\">",
true,
new Date(2010,9,9)
],
[
"41",
"Peru",
29461933,
0.0043,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/cf/Flag_of_Peru.svg/22px-Flag_of_Peru.svg.png\">",
true,
new Date(2010,9,9)
],
[
"42",
"Afghanistan",
29117000,
0.0042,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Flag_of_Afghanistan.svg/22px-Flag_of_Afghanistan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"43",
"Venezuela",
28958000,
0.0042,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Flag_of_Venezuela.svg/22px-Flag_of_Venezuela.svg.png\">",
true,
new Date(2010,9,9)
],
[
"44",
"Malaysia",
28250500,
0.0041,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/66/Flag_of_Malaysia.svg/22px-Flag_of_Malaysia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"45",
"Uzbekistan",
27794000,
0.004,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/84/Flag_of_Uzbekistan.svg/22px-Flag_of_Uzbekistan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"46",
"Saudi Arabia",
27136977,
0.0039,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/0d/Flag_of_Saudi_Arabia.svg/22px-Flag_of_Saudi_Arabia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"47",
"Ghana",
24333000,
0.0035,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Flag_of_Ghana.svg/22px-Flag_of_Ghana.svg.png\">",
true,
new Date(2010,9,9)
],
[
"48",
"Yemen",
24256000,
0.0035,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/89/Flag_of_Yemen.svg/22px-Flag_of_Yemen.svg.png\">",
true,
new Date(2010,9,9)
],
[
"49",
"North Korea",
23991000,
0.0035,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/51/Flag_of_North_Korea.svg/22px-Flag_of_North_Korea.svg.png\">",
true,
new Date(2010,9,9)
],
[
"50",
"Mozambique",
23406000,
0.0034,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Flag_of_Mozambique.svg/22px-Flag_of_Mozambique.svg.png\">",
true,
new Date(2010,9,9)
],
[
"52",
"Syria",
22505000,
0.0033,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/53/Flag_of_Syria.svg/22px-Flag_of_Syria.svg.png\">",
true,
new Date(2010,9,9)
],
[
"53",
"Australia",
22483305,
0.0033,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/b9/Flag_of_Australia.svg/22px-Flag_of_Australia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"54",
"Cote d'Ivoire",
21571000,
0.0031,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Flag_of_Cote_d%27Ivoire.svg/22px-Flag_of_Cote_d%27Ivoire.svg.png\">",
true,
new Date(2010,9,9)
],
[
"55",
"Romania",
21466174,
0.0031,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Flag_of_Romania.svg/22px-Flag_of_Romania.svg.png\">",
true,
new Date(2010,9,9)
],
[
"56",
"Sri Lanka",
20410000,
0.003,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/11/Flag_of_Sri_Lanka.svg/22px-Flag_of_Sri_Lanka.svg.png\">",
true,
new Date(2010,9,9)
],
[
"57",
"Madagascar",
20146000,
0.0029,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Flag_of_Madagascar.svg/22px-Flag_of_Madagascar.svg.png\">",
true,
new Date(2010,9,9)
],
[
"58",
"Cameroon",
19958000,
0.0029,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Flag_of_Cameroon.svg/22px-Flag_of_Cameroon.svg.png\">",
true,
new Date(2010,9,9)
],
[
"59",
"Angola",
18993000,
0.0028,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Flag_of_Angola.svg/22px-Flag_of_Angola.svg.png\">",
true,
new Date(2010,9,9)
],
[
"60",
"Chile",
17140000,
0.0025,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/78/Flag_of_Chile.svg/22px-Flag_of_Chile.svg.png\">",
true,
new Date(2010,9,9)
],
[
"61",
"Netherlands",
16619500,
0.00242,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/20/Flag_of_the_Netherlands.svg/22px-Flag_of_the_Netherlands.svg.png\">",
true,
new Date(2010,9,9)
],
[
"62",
"Burkina Faso",
16287000,
0.0024,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Flag_of_Burkina_Faso.svg/22px-Flag_of_Burkina_Faso.svg.png\">",
true,
new Date(2010,9,9)
],
[
"63",
"Kazakhstan",
16197000,
0.0024,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Flag_of_Kazakhstan.svg/22px-Flag_of_Kazakhstan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"64",
"Niger",
15891000,
0.0023,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f4/Flag_of_Niger.svg/22px-Flag_of_Niger.svg.png\">",
true,
new Date(2010,9,9)
],
[
"65",
"Malawi",
15692000,
0.0023,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d1/Flag_of_Malawi.svg/22px-Flag_of_Malawi.svg.png\">",
true,
new Date(2010,9,9)
],
[
"66",
"Mali",
14517176,
0.0021,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Flag_of_Mali.svg/22px-Flag_of_Mali.svg.png\">",
true,
new Date(2010,9,9)
],
[
"67",
"Guatemala",
14377000,
0.0021,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/ec/Flag_of_Guatemala.svg/22px-Flag_of_Guatemala.svg.png\">",
true,
new Date(2010,9,9)
],
[
"68",
"Ecuador",
14259000,
0.0021,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/e8/Flag_of_Ecuador.svg/22px-Flag_of_Ecuador.svg.png\">",
true,
new Date(2010,9,9)
],
[
"69",
"Cambodia",
13395682,
0.0019,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Flag_of_Cambodia.svg/22px-Flag_of_Cambodia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"70",
"Zambia",
13257000,
0.0019,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/06/Flag_of_Zambia.svg/22px-Flag_of_Zambia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"71",
"Senegal",
12861000,
0.0019,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fd/Flag_of_Senegal.svg/22px-Flag_of_Senegal.svg.png\">",
true,
new Date(2010,9,9)
],
[
"72",
"Zimbabwe",
12644000,
0.0018,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/6a/Flag_of_Zimbabwe.svg/22px-Flag_of_Zimbabwe.svg.png\">",
true,
new Date(2010,9,9)
],
[
"73",
"Chad",
11506000,
0.0017,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/Flag_of_Chad.svg/22px-Flag_of_Chad.svg.png\">",
true,
new Date(2010,9,9)
],
[
"74",
"Greece",
11306183,
0.0016,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Flag_of_Greece.svg/22px-Flag_of_Greece.svg.png\">",
true,
new Date(2010,9,9)
],
[
"75",
"Cuba",
11204000,
0.0016,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/bd/Flag_of_Cuba.svg/22px-Flag_of_Cuba.svg.png\">",
true,
new Date(2010,9,9)
],
[
"76",
"Belgium",
10827519,
0.0016,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Flag_of_Belgium_%28civil%29.svg/22px-Flag_of_Belgium_%28civil%29.svg.png\">",
true,
new Date(2010,9,9)
],
[
"77",
"Portugal",
10636888,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/5c/Flag_of_Portugal.svg/22px-Flag_of_Portugal.svg.png\">",
true,
new Date(2010,9,9)
],
[
"78",
"Czech Republic",
10512397,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Flag_of_the_Czech_Republic.svg/22px-Flag_of_the_Czech_Republic.svg.png\">",
true,
new Date(2010,9,9)
],
[
"79",
"Tunisia",
10432500,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/ce/Flag_of_Tunisia.svg/22px-Flag_of_Tunisia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"80",
"Guinea",
10324000,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Flag_of_Guinea.svg/22px-Flag_of_Guinea.svg.png\">",
true,
new Date(2010,9,9)
],
[
"81",
"Rwanda",
10277000,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/17/Flag_of_Rwanda.svg/22px-Flag_of_Rwanda.svg.png\">",
true,
new Date(2010,9,9)
],
[
"82",
"Dominican Republic",
10225000,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Flag_of_the_Dominican_Republic.svg/22px-Flag_of_the_Dominican_Republic.svg.png\">",
true,
new Date(2010,9,9)
],
[
"83",
"Haiti",
10188000,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Flag_of_Haiti.svg/22px-Flag_of_Haiti.svg.png\">",
true,
new Date(2010,9,9)
],
[
"84",
"Bolivia",
10031000,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Flag_of_Bolivia.svg/22px-Flag_of_Bolivia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"85",
"Hungary",
10013628,
0.0015,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/c1/Flag_of_Hungary.svg/22px-Flag_of_Hungary.svg.png\">",
true,
new Date(2010,9,9)
],
[
"86",
"Serbia",
9856000,
0.0014,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Flag_of_Serbia.svg/22px-Flag_of_Serbia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"87",
"Belarus",
9467700,
0.0014,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/85/Flag_of_Belarus.svg/22px-Flag_of_Belarus.svg.png\">",
true,
new Date(2010,9,9)
],
[
"88",
"Sweden",
9393648,
0.0014,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Flag_of_Sweden.svg/22px-Flag_of_Sweden.svg.png\">",
true,
new Date(2010,9,9)
],
[
"89",
"Somalia",
9359000,
0.0014,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/Flag_of_Somalia.svg/22px-Flag_of_Somalia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"90",
"Benin",
9212000,
0.0013,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/0a/Flag_of_Benin.svg/22px-Flag_of_Benin.svg.png\">",
true,
new Date(2010,9,9)
],
[
"91",
"Azerbaijan",
8997400,
0.0013,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Flag_of_Azerbaijan.svg/22px-Flag_of_Azerbaijan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"92",
"Burundi",
8519000,
0.0012,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Flag_of_Burundi.svg/22px-Flag_of_Burundi.svg.png\">",
true,
new Date(2010,9,9)
],
[
"93",
"Austria",
8372930,
0.0012,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/41/Flag_of_Austria.svg/22px-Flag_of_Austria.svg.png\">",
true,
new Date(2010,9,9)
],
[
"94",
"Switzerland",
7782900,
0.0011,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f3/Flag_of_Switzerland.svg/20px-Flag_of_Switzerland.svg.png\">",
true,
new Date(2010,9,9)
],
[
"95",
"Israel",
7640800,
0.0011,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Flag_of_Israel.svg/22px-Flag_of_Israel.svg.png\">",
true,
new Date(2010,9,9)
],
[
"96",
"Honduras",
7616000,
0.0011,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/82/Flag_of_Honduras.svg/22px-Flag_of_Honduras.svg.png\">",
true,
new Date(2010,9,9)
],
[
"97",
"Bulgaria",
7576751,
0.0011,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Flag_of_Bulgaria.svg/22px-Flag_of_Bulgaria.svg.png\">",
true,
new Date(2010,9,9)
],
[
"98",
"Tajikistan",
7075000,
0.00103,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d0/Flag_of_Tajikistan.svg/22px-Flag_of_Tajikistan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"100",
"Papua New Guinea",
6888000,
0.001,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/e3/Flag_of_Papua_New_Guinea.svg/22px-Flag_of_Papua_New_Guinea.svg.png\">",
true,
new Date(2010,9,9)
],
[
"101",
"Togo",
6780000,
0.00099,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/68/Flag_of_Togo.svg/22px-Flag_of_Togo.svg.png\">",
true,
new Date(2010,9,9)
],
[
"102",
"Libya",
6546000,
0.00095,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Flag_of_Libya.svg/22px-Flag_of_Libya.svg.png\">",
true,
new Date(2010,9,9)
],
[
"103",
"Jordan",
6472000,
0.00094,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/c0/Flag_of_Jordan.svg/22px-Flag_of_Jordan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"104",
"Paraguay",
6460000,
0.00094,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Flag_of_Paraguay.svg/22px-Flag_of_Paraguay.svg.png\">",
true,
new Date(2010,9,9)
],
[
"105",
"Laos",
6436000,
0.00094,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Flag_of_Laos.svg/22px-Flag_of_Laos.svg.png\">",
true,
new Date(2010,9,9)
],
[
"106",
"El Salvador",
6194000,
9e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/34/Flag_of_El_Salvador.svg/22px-Flag_of_El_Salvador.svg.png\">",
true,
new Date(2010,9,9)
],
[
"107",
"Sierra Leone",
5836000,
0.00085,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/17/Flag_of_Sierra_Leone.svg/22px-Flag_of_Sierra_Leone.svg.png\">",
true,
new Date(2010,9,9)
],
[
"108",
"Nicaragua",
5822000,
0.00085,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Flag_of_Nicaragua.svg/22px-Flag_of_Nicaragua.svg.png\">",
true,
new Date(2010,9,9)
],
[
"109",
"Kyrgyzstan",
5550000,
0.00081,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/Flag_of_Kyrgyzstan.svg/22px-Flag_of_Kyrgyzstan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"110",
"Denmark",
5543819,
8e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Flag_of_Denmark.svg/22px-Flag_of_Denmark.svg.png\">",
true,
new Date(2010,9,9)
],
[
"111",
"Slovakia",
5429763,
0.00079,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/e6/Flag_of_Slovakia.svg/22px-Flag_of_Slovakia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"112",
"Finland",
5370000,
0.00078,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Flag_of_Finland.svg/22px-Flag_of_Finland.svg.png\">",
true,
new Date(2010,9,9)
],
[
"113",
"Eritrea",
5224000,
0.00076,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/29/Flag_of_Eritrea.svg/22px-Flag_of_Eritrea.svg.png\">",
true,
new Date(2010,9,9)
],
[
"114",
"Turkmenistan",
5177000,
0.00075,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Flag_of_Turkmenistan.svg/22px-Flag_of_Turkmenistan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"115",
"Singapore",
5076700,
0.00074,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Flag_of_Singapore.svg/22px-Flag_of_Singapore.svg.png\">",
true,
new Date(2010,9,9)
],
[
"116",
"Norway",
4906500,
0.00071,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d9/Flag_of_Norway.svg/22px-Flag_of_Norway.svg.png\">",
true,
new Date(2010,9,9)
],
[
"117",
"United Arab Emirates",
4707000,
0.00068,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/cb/Flag_of_the_United_Arab_Emirates.svg/22px-Flag_of_the_United_Arab_Emirates.svg.png\">",
true,
new Date(2010,9,9)
],
[
"118",
"Costa Rica",
4640000,
0.00068,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f2/Flag_of_Costa_Rica.svg/22px-Flag_of_Costa_Rica.svg.png\">",
true,
new Date(2010,9,9)
],
[
"119",
"Central African Republic",
4506000,
0.00066,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/6f/Flag_of_the_Central_African_Republic.svg/22px-Flag_of_the_Central_African_Republic.svg.png\">",
true,
new Date(2010,9,9)
],
[
"120",
"Ireland",
4470700,
0.00064,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/45/Flag_of_Ireland.svg/22px-Flag_of_Ireland.svg.png\">",
true,
new Date(2010,9,9)
],
[
"121",
"Georgia",
4436000,
0.00065,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Flag_of_Georgia.svg/22px-Flag_of_Georgia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"122",
"Croatia",
4435056,
0.00065,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/1b/Flag_of_Croatia.svg/22px-Flag_of_Croatia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"123",
"New Zealand",
4393000,
0.00064,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/3e/Flag_of_New_Zealand.svg/22px-Flag_of_New_Zealand.svg.png\">",
true,
new Date(2010,9,9)
],
[
"124",
"Lebanon",
4255000,
0.00062,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/5/59/Flag_of_Lebanon.svg/22px-Flag_of_Lebanon.svg.png\">",
true,
new Date(2010,9,9)
],
[
"125",
"Liberia",
4102000,
6e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/b8/Flag_of_Liberia.svg/22px-Flag_of_Liberia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"127",
"Palestinian territories",
3935249,
0.00055,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/00/Flag_of_Palestine.svg/22px-Flag_of_Palestine.svg.png\">",
true,
new Date(2010,9,9)
],
[
"128",
"Bosnia and Herzegovina",
3760000,
0.00055,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/bf/Flag_of_Bosnia_and_Herzegovina.svg/22px-Flag_of_Bosnia_and_Herzegovina.svg.png\">",
true,
new Date(2010,9,9)
],
[
"129",
"Republic of the Congo",
3759000,
0.00055,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Flag_of_the_Republic_of_the_Congo.svg/22px-Flag_of_the_Republic_of_the_Congo.svg.png\">",
true,
new Date(2010,9,9)
],
[
"130",
"Moldova",
3563800,
0.00052,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/27/Flag_of_Moldova.svg/22px-Flag_of_Moldova.svg.png\">",
true,
new Date(2010,9,9)
],
[
"131",
"Uruguay",
3372000,
0.00049,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fe/Flag_of_Uruguay.svg/22px-Flag_of_Uruguay.svg.png\">",
true,
new Date(2010,9,9)
],
[
"132",
"Mauritania",
3366000,
0.00049,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/43/Flag_of_Mauritania.svg/22px-Flag_of_Mauritania.svg.png\">",
true,
new Date(2010,9,9)
],
[
"133",
"Lithuania",
3329227,
0.00048,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/11/Flag_of_Lithuania.svg/22px-Flag_of_Lithuania.svg.png\">",
true,
new Date(2010,9,9)
],
[
"134",
"Panama",
3322576,
0.00048,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/ab/Flag_of_Panama.svg/22px-Flag_of_Panama.svg.png\">",
true,
new Date(2010,9,9)
],
[
"135",
"Armenia",
3238000,
0.00047,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/2f/Flag_of_Armenia.svg/22px-Flag_of_Armenia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"136",
"Albania",
3195000,
0.00046,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/36/Flag_of_Albania.svg/22px-Flag_of_Albania.svg.png\">",
true,
new Date(2010,9,9)
],
[
"137",
"Kuwait",
3051000,
0.00044,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/a/aa/Flag_of_Kuwait.svg/22px-Flag_of_Kuwait.svg.png\">",
true,
new Date(2010,9,9)
],
[
"138",
"Oman",
2905000,
0.00042,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Flag_of_Oman.svg/22px-Flag_of_Oman.svg.png\">",
true,
new Date(2010,9,9)
],
[
"139",
"Mongolia",
2776500,
4e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Flag_of_Mongolia.svg/22px-Flag_of_Mongolia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"140",
"Jamaica",
2730000,
4e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/0a/Flag_of_Jamaica.svg/22px-Flag_of_Jamaica.svg.png\">",
true,
new Date(2010,9,9)
],
[
"141",
"Latvia",
2236300,
0.00033,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/84/Flag_of_Latvia.svg/22px-Flag_of_Latvia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"142",
"Namibia",
2212000,
0.00032,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/00/Flag_of_Namibia.svg/22px-Flag_of_Namibia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"143",
"Lesotho",
2084000,
3e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4a/Flag_of_Lesotho.svg/22px-Flag_of_Lesotho.svg.png\">",
true,
new Date(2010,9,9)
],
[
"144",
"Slovenia",
2065720,
3e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f0/Flag_of_Slovenia.svg/22px-Flag_of_Slovenia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"145",
"Republic of Macedonia",
2048620,
3e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/f8/Flag_of_Macedonia.svg/22px-Flag_of_Macedonia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"146",
"Botswana",
1978000,
0.00029,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Flag_of_Botswana.svg/22px-Flag_of_Botswana.svg.png\">",
true,
new Date(2010,9,9)
],
[
"147",
"Gambia",
1751000,
0.00025,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/77/Flag_of_The_Gambia.svg/22px-Flag_of_The_Gambia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"148",
"Qatar",
1696563,
0.00025,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/65/Flag_of_Qatar.svg/22px-Flag_of_Qatar.svg.png\">",
true,
new Date(2010,9,9)
],
[
"149",
"Guinea-Bissau",
1647000,
0.00024,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/01/Flag_of_Guinea-Bissau.svg/22px-Flag_of_Guinea-Bissau.svg.png\">",
true,
new Date(2010,9,9)
],
[
"150",
"Gabon",
1501000,
0.00022,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/04/Flag_of_Gabon.svg/22px-Flag_of_Gabon.svg.png\">",
true,
new Date(2010,9,9)
],
[
"151",
"Trinidad and Tobago",
1344000,
2e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Flag_of_Trinidad_and_Tobago.svg/22px-Flag_of_Trinidad_and_Tobago.svg.png\">",
true,
new Date(2010,9,9)
],
[
"152",
"Estonia",
1340127,
0.00019,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/8f/Flag_of_Estonia.svg/22px-Flag_of_Estonia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"153",
"Mauritius",
1297000,
0.00019,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/77/Flag_of_Mauritius.svg/22px-Flag_of_Mauritius.svg.png\">",
true,
new Date(2010,9,9)
],
[
"154",
"Swaziland",
1202000,
0.00017,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/1e/Flag_of_Swaziland.svg/22px-Flag_of_Swaziland.svg.png\">",
true,
new Date(2010,9,9)
],
[
"155",
"East Timor",
1171000,
0.00017,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/26/Flag_of_East_Timor.svg/22px-Flag_of_East_Timor.svg.png\">",
true,
new Date(2010,9,9)
],
[
"156",
"Djibouti",
879000,
0.00013,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/34/Flag_of_Djibouti.svg/22px-Flag_of_Djibouti.svg.png\">",
true,
new Date(2010,9,9)
],
[
"157",
"Fiji",
854000,
0.00012,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/ba/Flag_of_Fiji.svg/22px-Flag_of_Fiji.svg.png\">",
true,
new Date(2010,9,9)
],
[
"158",
"Bahrain",
807000,
0.00012,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/2c/Flag_of_Bahrain.svg/22px-Flag_of_Bahrain.svg.png\">",
true,
new Date(2010,9,9)
],
[
"159",
"Cyprus",
801851,
0.00012,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d4/Flag_of_Cyprus.svg/22px-Flag_of_Cyprus.svg.png\">",
true,
new Date(2010,9,9)
],
[
"160",
"Guyana",
761000,
0.00011,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Flag_of_Guyana.svg/22px-Flag_of_Guyana.svg.png\">",
true,
new Date(2010,9,9)
],
[
"161",
"Bhutan",
708000,
1e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Flag_of_Bhutan.svg/22px-Flag_of_Bhutan.svg.png\">",
true,
new Date(2010,9,9)
],
[
"162",
"Equatorial Guinea",
693000,
1e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Flag_of_Equatorial_Guinea.svg/22px-Flag_of_Equatorial_Guinea.svg.png\">",
true,
new Date(2010,9,9)
],
[
"163",
"Comoros",
691000,
1e-04,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/94/Flag_of_the_Comoros.svg/22px-Flag_of_the_Comoros.svg.png\">",
true,
new Date(2010,9,9)
],
[
"164",
"Montenegro",
626000,
9e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Flag_of_Montenegro.svg/22px-Flag_of_Montenegro.svg.png\">",
true,
new Date(2010,9,9)
],
[
"166",
"Solomon Islands",
536000,
8e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/74/Flag_of_the_Solomon_Islands.svg/22px-Flag_of_the_Solomon_Islands.svg.png\">",
true,
new Date(2010,9,9)
],
[
"167",
"Western Sahara",
530000,
8e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/c8/Flag_of_Western_Sahara.svg/22px-Flag_of_Western_Sahara.svg.png\">",
true,
new Date(2010,9,9)
],
[
"168",
"Suriname",
524000,
8e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/60/Flag_of_Suriname.svg/22px-Flag_of_Suriname.svg.png\">",
true,
new Date(2010,9,9)
],
[
"169",
"Cape Verde",
513000,
7e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Flag_of_Cape_Verde.svg/22px-Flag_of_Cape_Verde.svg.png\">",
true,
new Date(2010,9,9)
],
[
"170",
"Luxembourg",
502207,
7e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Flag_of_Luxembourg.svg/22px-Flag_of_Luxembourg.svg.png\">",
true,
new Date(2010,9,9)
],
[
"171",
"Malta",
416333,
6e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Flag_of_Malta.svg/22px-Flag_of_Malta.svg.png\">",
true,
new Date(2010,9,9)
],
[
"172",
"Brunei",
407000,
6e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9c/Flag_of_Brunei.svg/22px-Flag_of_Brunei.svg.png\">",
true,
new Date(2010,9,9)
],
[
"173",
"Bahamas",
346000,
5e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/93/Flag_of_the_Bahamas.svg/22px-Flag_of_the_Bahamas.svg.png\">",
true,
new Date(2010,9,9)
],
[
"174",
"Belize",
322100,
5e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Flag_of_Belize.svg/22px-Flag_of_Belize.svg.png\">",
true,
new Date(2010,9,9)
],
[
"175",
"Iceland",
318006,
5e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/ce/Flag_of_Iceland.svg/22px-Flag_of_Iceland.svg.png\">",
true,
new Date(2010,9,9)
],
[
"176",
"Maldives",
314000,
5e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/0f/Flag_of_Maldives.svg/22px-Flag_of_Maldives.svg.png\">",
true,
new Date(2010,9,9)
],
[
"177",
"Barbados",
257000,
4e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/ef/Flag_of_Barbados.svg/22px-Flag_of_Barbados.svg.png\">",
true,
new Date(2010,9,9)
],
[
"178",
"Vanuatu",
246000,
4e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Flag_of_Vanuatu.svg/22px-Flag_of_Vanuatu.svg.png\">",
true,
new Date(2010,9,9)
],
[
"181",
"Samoa",
179000,
3e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Flag_of_Samoa.svg/22px-Flag_of_Samoa.svg.png\">",
true,
new Date(2010,9,9)
],
[
"182",
"Saint Lucia",
174000,
3e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9f/Flag_of_Saint_Lucia.svg/22px-Flag_of_Saint_Lucia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"183",
"Sao Tome and Principe",
165000,
2e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Flag_of_Sao_Tome_and_Principe.svg/22px-Flag_of_Sao_Tome_and_Principe.svg.png\">",
true,
new Date(2010,9,9)
],
[
"184",
"Federated States of Micronesia",
111000,
2e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Flag_of_Federated_States_of_Micronesia.svg/22px-Flag_of_Federated_States_of_Micronesia.svg.png\">",
true,
new Date(2010,9,9)
],
[
"186",
"Saint Vincent and the Grenadines",
109000,
2e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/6/6d/Flag_of_Saint_Vincent_and_the_Grenadines.svg/22px-Flag_of_Saint_Vincent_and_the_Grenadines.svg.png\">",
true,
new Date(2010,9,9)
],
[
"188",
"Grenada",
104000,
2e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/bc/Flag_of_Grenada.svg/22px-Flag_of_Grenada.svg.png\">",
true,
new Date(2010,9,9)
],
[
"189",
"Tonga",
104000,
2e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/9a/Flag_of_Tonga.svg/22px-Flag_of_Tonga.svg.png\">",
true,
new Date(2010,9,9)
],
[
"190",
"Kiribati",
100000,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/d/d3/Flag_of_Kiribati.svg/22px-Flag_of_Kiribati.svg.png\">",
true,
new Date(2010,9,9)
],
[
"192",
"Antigua and Barbuda",
89000,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/8/89/Flag_of_Antigua_and_Barbuda.svg/22px-Flag_of_Antigua_and_Barbuda.svg.png\">",
true,
new Date(2010,9,9)
],
[
"194",
"Seychelles",
85000,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/9/92/Flag_of_the_Seychelles.svg/22px-Flag_of_the_Seychelles.svg.png\">",
true,
new Date(2010,9,9)
],
[
"195",
"Andorra",
84082,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Flag_of_Andorra.svg/22px-Flag_of_Andorra.svg.png\">",
true,
new Date(2010,9,9)
],
[
"198",
"Dominica",
67000,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/c/c4/Flag_of_Dominica.svg/22px-Flag_of_Dominica.svg.png\">",
true,
new Date(2010,9,9)
],
[
"200",
"Marshall Islands",
63000,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/2/2e/Flag_of_the_Marshall_Islands.svg/22px-Flag_of_the_Marshall_Islands.svg.png\">",
true,
new Date(2010,9,9)
],
[
"204",
"Saint Kitts and Nevis",
52000,
1e-05,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/f/fe/Flag_of_Saint_Kitts_and_Nevis.svg/22px-Flag_of_Saint_Kitts_and_Nevis.svg.png\">",
true,
new Date(2010,9,9)
],
[
"206",
"Liechtenstein",
35904,
5e-06,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/47/Flag_of_Liechtenstein.svg/22px-Flag_of_Liechtenstein.svg.png\">",
true,
new Date(2010,9,9)
],
[
"207",
"Monaco",
33000,
5e-06,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Flag_of_Monaco.svg/22px-Flag_of_Monaco.svg.png\">",
true,
new Date(2010,9,9)
],
[
"209",
"San Marino",
31794,
5e-06,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/b/b1/Flag_of_San_Marino.svg/22px-Flag_of_San_Marino.svg.png\">",
true,
new Date(2010,9,9)
],
[
"213",
"Palau",
20000,
3e-06,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/4/48/Flag_of_Palau.svg/22px-Flag_of_Palau.svg.png\">",
true,
new Date(2010,9,9)
],
[
"215",
"Tuvalu",
10000,
1e-06,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/38/Flag_of_Tuvalu.svg/22px-Flag_of_Tuvalu.svg.png\">",
true,
new Date(2010,9,9)
],
[
"216",
"Nauru",
10000,
1e-06,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Flag_of_Nauru.svg/22px-Flag_of_Nauru.svg.png\">",
true,
new Date(2010,9,9)
],
[
"222",
"Vatican City",
800,
2e-07,
"<img src=\"http://upload.wikimedia.org/wikipedia/commons/thumb/0/00/Flag_of_the_Vatican_City.svg/20px-Flag_of_the_Vatican_City.svg.png\">",
true,
new Date(2010,9,9)
] 
];
data.addColumn('string','Rank');
data.addColumn('string','Country');
data.addColumn('number','Population');
data.addColumn('number','% of World Population');
data.addColumn('string','Flag');
data.addColumn('boolean','Mode');
data.addColumn('date','Date');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartTableID2cd013050362() {
var data = gvisDataTableID2cd013050362();
var options = {};
options["allowHtml"] = true;
options["page"] = "enable";

  var dataFormat1 = new google.visualization.NumberFormat({pattern:"#,###"});
  dataFormat1.format(data, 2);
  var dataFormat2 = new google.visualization.NumberFormat({pattern:"#.#%"});
  dataFormat2.format(data, 3);

    var chart = new google.visualization.Table(
    document.getElementById('TableID2cd013050362')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "table";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartTableID2cd013050362);
})();
function displayChartTableID2cd013050362() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartTableID2cd013050362"></script>
 
<!-- divChart -->
  
<div id="TableID2cd013050362" 
  style="width: 500; height: automatic;">
</div>
<br>

# Dashboards with `gvisMerge`

<!--  -->
<!-- GT -->
<!--  -->

```r
G <- gvisGeoChart(Exports,
                  locationvar="Country",
                  colorvar="Profit",
                  options=list(width=300, height=200))

T <- gvisTable(Exports,
               options=list(width=300, height=370))
```

```r
GT <- gvisMerge(G, T, horizontal=FALSE)

plot(GT)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID49611ffdda49 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Germany",
3
],
[
"Brazil",
4
],
[
"United States",
5
],
[
"France",
4
],
[
"Hungary",
3
],
[
"India",
2
],
[
"Iceland",
1
],
[
"Norway",
4
],
[
"Spain",
5
],
[
"Turkey",
1
] 
];
data.addColumn('string','Country');
data.addColumn('number','Profit');
data.addRows(datajson);
return(data);
}


// jsData 
function gvisDataTableID496155ab963a () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Germany",
3,
true
],
[
"Brazil",
4,
false
],
[
"United States",
5,
true
],
[
"France",
4,
true
],
[
"Hungary",
3,
false
],
[
"India",
2,
true
],
[
"Iceland",
1,
false
],
[
"Norway",
4,
true
],
[
"Spain",
5,
true
],
[
"Turkey",
1,
false
] 
];
data.addColumn('string','Country');
data.addColumn('number','Profit');
data.addColumn('boolean','Online');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID49611ffdda49() {
var data = gvisDataGeoChartID49611ffdda49();
var options = {};
options["width"] = 300;
options["height"] = 200;


    var chart = new google.visualization.GeoChart(
    document.getElementById('GeoChartID49611ffdda49')
    );
    chart.draw(data,options);
    

}
  


// jsDrawChart
function drawChartTableID496155ab963a() {
var data = gvisDataTableID496155ab963a();
var options = {};
options["allowHtml"] = true;
options["width"] = 300;
options["height"] = 370;


    var chart = new google.visualization.Table(
    document.getElementById('TableID496155ab963a')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geochart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID49611ffdda49);
})();
function displayChartGeoChartID49611ffdda49() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}


// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "table";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartTableID496155ab963a);
})();
function displayChartTableID496155ab963a() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID49611ffdda49"></script>


<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartTableID496155ab963a"></script>
 
<table border="0">
<tr>
<td>

<!-- divChart -->
  
<div id="GeoChartID49611ffdda49" 
  style="width: 300; height: 200;">
</div>

</td>
</tr>
<tr>
<td>

<!-- divChart -->
  
<div id="TableID496155ab963a" 
  style="width: 300; height: 370;">
</div>

</td>
</tr>
</table>
<br>

```r
G <- gvisGeoChart(Exports,
                  locationvar="Country",
                  colorvar="Profit",
                  options=list(width=300, height=370))
```

```r
GT <- gvisMerge(G, T, horizontal=TRUE)

plot(GT)
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataGeoChartID114f1fa5c040 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Germany",
3
],
[
"Brazil",
4
],
[
"United States",
5
],
[
"France",
4
],
[
"Hungary",
3
],
[
"India",
2
],
[
"Iceland",
1
],
[
"Norway",
4
],
[
"Spain",
5
],
[
"Turkey",
1
] 
];
data.addColumn('string','Country');
data.addColumn('number','Profit');
data.addRows(datajson);
return(data);
}


// jsData 
function gvisDataTableID114f79fb112f () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"Germany",
3,
true
],
[
"Brazil",
4,
false
],
[
"United States",
5,
true
],
[
"France",
4,
true
],
[
"Hungary",
3,
false
],
[
"India",
2,
true
],
[
"Iceland",
1,
false
],
[
"Norway",
4,
true
],
[
"Spain",
5,
true
],
[
"Turkey",
1,
false
] 
];
data.addColumn('string','Country');
data.addColumn('number','Profit');
data.addColumn('boolean','Online');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartGeoChartID114f1fa5c040() {
var data = gvisDataGeoChartID114f1fa5c040();
var options = {};
options["width"] = 300;
options["height"] = 370;


    var chart = new google.visualization.GeoChart(
    document.getElementById('GeoChartID114f1fa5c040')
    );
    chart.draw(data,options);
    

}
  


// jsDrawChart
function drawChartTableID114f79fb112f() {
var data = gvisDataTableID114f79fb112f();
var options = {};
options["allowHtml"] = true;
options["width"] = 200;
options["height"] = 270;


    var chart = new google.visualization.Table(
    document.getElementById('TableID114f79fb112f')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "geochart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartGeoChartID114f1fa5c040);
})();
function displayChartGeoChartID114f1fa5c040() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}


// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "table";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartTableID114f79fb112f);
})();
function displayChartTableID114f79fb112f() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartGeoChartID114f1fa5c040"></script>


<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartTableID114f79fb112f"></script>
 
<table border="0">
<tr>
<td>

<!-- divChart -->
  
<div id="GeoChartID114f1fa5c040" 
  style="width: 300; height: 370;">
</div>

</td>
<td>

<!-- divChart -->
  
<div id="TableID114f79fb112f" 
  style="width: 200; height: 270;">
</div>

</td>
</tr>
</table>

# Options

<!--  -->
<!-- Line -->
<!--  -->

```r
df <- data.frame(country=c("US", "GB", "BR"),
                 val1=c(1,3,4),
                 val2=c(23,12,32))

Line <- gvisLineChart(df,
                      xvar="country",
                      yvar=c("val1","val2"),
                      options=list(
                        title="Hello World",
                        titleTextStyle="{color:'red',
                                         fontName:'Courier',
                                         fontSize:16}",
                        backgroundColor="#D3D3D3",
                        vAxis="{gridlines:{color:'red', count:3}}",
                        hAxis="{title:'Country',
                                titleTextStyle:{color:'blue'}}",
                        series="[{color:'green',
                                  targetAxisIndex: 0},
                                 {color: 'orange',targetAxisIndex:1}]",
                        vAxes="[{title:'val1'}, {title:'val2'}]",
                        legend="bottom",
                        curveType="function",
                        width=500,
                        height=300))
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataLineChartID584f5d6f811a () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
"US",
1,
23
],
[
"GB",
3,
12
],
[
"BR",
4,
32
] 
];
data.addColumn('string','country');
data.addColumn('number','val1');
data.addColumn('number','val2');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartLineChartID584f5d6f811a() {
var data = gvisDataLineChartID584f5d6f811a();
var options = {};
options["allowHtml"] = true;
options["title"] = "Hello World";
options["titleTextStyle"] = {color:'red',
                                         fontName:'Courier',
                                         fontSize:16};
options["backgroundColor"] = "#D3D3D3";
options["vAxis"] = {gridlines:{color:'red', count:3}};
options["hAxis"] = {title:'Country',
                                titleTextStyle:{color:'blue'}};
options["series"] = [{color:'green',
                                  targetAxisIndex: 0},
                                 {color: 'orange',targetAxisIndex:1}];
options["vAxes"] = [{title:'val1'}, {title:'val2'}];
options["legend"] = "bottom";
options["curveType"] = "function";
options["width"] = 500;
options["height"] = 300;


    var chart = new google.visualization.LineChart(
    document.getElementById('LineChartID584f5d6f811a')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "corechart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartLineChartID584f5d6f811a);
})();
function displayChartLineChartID584f5d6f811a() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartLineChartID584f5d6f811a"></script>
 
<!-- divChart -->
  
<div id="LineChartID584f5d6f811a" 
  style="width: 500; height: 300;">
</div>
<br>

## Apostrophes

<!--  -->
<!-- R -->
<!--  -->

```r
df <- data.frame("Year"=c(2009,2010),
                 "Lloyd\\'s"=c(86.1, 93.3),
                 "Munich Re\\'s R/I"=c(95.3, 100.5), check.names=FALSE)

R <- gvisColumnChart(df, 
                     options=list(vAxis='{baseline:0}',
                                  title="Combined Ratio %",
                                  legend="{position:'bottom'}"))

cat(R$html$chart, file = "GoogleVis/R.html") # save
```

<!-- jsHeader -->
<script type="text/javascript">
 
// jsData 
function gvisDataColumnChartID336e54fe1b42 () {
var data = new google.visualization.DataTable();
var datajson =
[
 [
2009,
86.1,
95.3
],
[
2010,
93.3,
100.5
] 
];
data.addColumn('number','Year');
data.addColumn('number','Lloyd\'s');
data.addColumn('number','Munich Re\'s R/I');
data.addRows(datajson);
return(data);
}
 
// jsDrawChart
function drawChartColumnChartID336e54fe1b42() {
var data = gvisDataColumnChartID336e54fe1b42();
var options = {};
options["allowHtml"] = true;
options["vAxis"] = {baseline:0};
options["title"] = "Combined Ratio %";
options["legend"] = {position:'bottom'};


    var chart = new google.visualization.ColumnChart(
    document.getElementById('ColumnChartID336e54fe1b42')
    );
    chart.draw(data,options);
    

}
  
 
// jsDisplayChart
(function() {
var pkgs = window.__gvisPackages = window.__gvisPackages || [];
var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
var chartid = "corechart";
  
// Manually see if chartid is in pkgs (not all browsers support Array.indexOf)
var i, newPackage = true;
for (i = 0; newPackage && i < pkgs.length; i++) {
if (pkgs[i] === chartid)
newPackage = false;
}
if (newPackage)
  pkgs.push(chartid);
  
// Add the drawChart function to the global list of callbacks
callbacks.push(drawChartColumnChartID336e54fe1b42);
})();
function displayChartColumnChartID336e54fe1b42() {
  var pkgs = window.__gvisPackages = window.__gvisPackages || [];
  var callbacks = window.__gvisCallbacks = window.__gvisCallbacks || [];
  window.clearTimeout(window.__gvisLoad);
  // The timeout is set to 100 because otherwise the container div we are
  // targeting might not be part of the document yet
  window.__gvisLoad = setTimeout(function() {
  var pkgCount = pkgs.length;
  google.load("visualization", "1", { packages:pkgs, callback: function() {
  if (pkgCount != pkgs.length) {
  // Race condition where another setTimeout call snuck in after us; if
  // that call added a package, we must not shift its callback
  return;
}
while (callbacks.length > 0)
callbacks.shift()();
} });
}, 100);
}
 
// jsFooter
</script>
 
<!-- jsChart -->  
<script type="text/javascript" src="https://www.google.com/jsapi?callback=displayChartColumnChartID336e54fe1b42"></script>
 
<!-- divChart -->
  
<div id="ColumnChartID336e54fe1b42" 
  style="width: 500; height: automatic;">
</div>
