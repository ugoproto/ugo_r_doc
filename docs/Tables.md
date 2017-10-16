-   [Markdown tables](#markdown-tables)
    -   [Example 1](#example-1)
    -   [Example 2](#example-2)
-   [The `xtable` package](#the-xtable-package)
    -   [Example 1](#example-1-1)
-   [The `knitr::kable` function](#the-knitrkable-function)
    -   [Example 1](#example-1-2)
-   [The `htmlTable` package](#the-htmltable-package)
    -   [Example 1](#example-1-4)
    -   [Example 2](#example-2-1)
    -   [Example 3](#example-3)
    -   [Example 4](#example-4)
    -   [Example 5](#example-5)
    -   [Example 6](#example-6)
    -   [Example 7](#example-7)
-   [The `ztable` package](#the-ztable-package)
    -   [Example 1](#example-1-5)

------------------------------------------------------------------------

**Foreword**

-   Output options: 'pygments' syntax, the 'readable' theme.
-   Snippets and results.

------------------------------------------------------------------------

Markdown tables
---------------

### Example 1

    | Right | Left | Default | Center |
    |------:|:-----|---------|:------:|
    |   12  |  12  |    12   |    12  |
    |  123  |  123 |   123   |   123  |
    |    1  |    1 |     1   |     1  |

    : Demonstration of pipe table syntax.

<table>
<caption>Demonstration of pipe table syntax.</caption>
<thead>
<tr class="header">
<th align="right">Right</th>
<th align="left">Left</th>
<th>Default</th>
<th align="center">Center</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="right">12</td>
<td align="left">12</td>
<td>12</td>
<td align="center">12</td>
</tr>
<tr class="even">
<td align="right">123</td>
<td align="left">123</td>
<td>123</td>
<td align="center">123</td>
</tr>
<tr class="odd">
<td align="right">1</td>
<td align="left">1</td>
<td>1</td>
<td align="center">1</td>
</tr>
</tbody>
</table>

### Example 2

    : Sample grid table.

    +---------------+---------------+--------------------+
    | Fruit         | Price         | Advantages         |
    +===============+===============+====================+
    | Bananas       | $1.34         | - built-in wrapper |
    |               |               | - bright color     |
    +---------------+---------------+--------------------+
    | Oranges       | $2.10         | - cures scurvy     |
    |               |               | - tasty            |
    +---------------+---------------+--------------------+

<table style="width:74%;">
<caption>Sample grid table.</caption>
<colgroup>
<col width="22%" />
<col width="22%" />
<col width="29%" />
</colgroup>
<thead>
<tr class="header">
<th>Fruit</th>
<th>Price</th>
<th>Advantages</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Bananas</p></td>
<td><p>$1.34</p></td>
<td><ul>
<li>built-in wrapper</li>
<li>bright color</li>
</ul></td>
</tr>
<tr class="even">
<td><p>Oranges</p></td>
<td><p>$2.10</p></td>
<td><ul>
<li>cures scurvy</li>
<li>tasty</li>
</ul></td>
</tr>
</tbody>
</table>

The `xtable` package
--------------------

The output is in HTML.

### Example 1

``` r
library(xtable)

# given the data in the first row
print(xtable(output,
             caption = 'A test table', 
             align = c('l', 'c', 'r')),
      type = 'html')
```

<!-- html table generated in R 3.4.2 by xtable 1.8-2 package -->
<!-- Sun Oct 15 19:29:31 2017 -->
<table border=1>
<caption align="bottom"> A test table </caption>
<tr> <th>  </th> <th> 1st header </th> <th> 2nd header </th>  </tr>
  <tr> <td> 1st row </td> <td align="center"> Content A </td> <td align="right"> Content B </td> </tr>
  <tr> <td> 2nd row </td> <td align="center"> Content C </td> <td align="right"> Content D </td> </tr>
   </table>

The `knitr::kable` function
---------------------------

The output is in Markdown.

### Example 1

``` r
library(knitr)

# given the data in the first row
kable(output, 
      caption = 'A test table', 
      align = c('c', 'r'))
```

<table>
<caption>A test table</caption>
<thead>
<tr class="header">
<th></th>
<th align="center">1st header</th>
<th align="right">2nd header</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>1st row</td>
<td align="center">Content A</td>
<td align="right">Content B</td>
</tr>
<tr class="even">
<td>2nd row</td>
<td align="center">Content C</td>
<td align="right">Content D</td>
</tr>
</tbody>
</table>

We can also write `knitr::kable()` without calling `library(knitr)`.

The `htmlTable` package
-----------------------

[htmlTable](https://github.com/gforge/htmlTable) on GitHub.

### Example 1

``` r
output <- 
  matrix(paste('Content', LETTERS[1:16]), 
         ncol = 4, byrow = TRUE)

library(htmlTable)

htmlTable(output,
          header = paste(c('1st', '2nd', '3rd', '4th'), 'header'),
          rnames = paste(c('1st', '2nd', '3rd', '4th'), 'row'),
          rgroup = c('Group A', 'Group B'),
          n.rgroup = c(2,2),
          cgroup = c('Cgroup 1', 'Cgroup 2&dagger;'),
          n.cgroup = c(2,2), 
          caption = 'Basic table with both column spanners (groups) and row groups',
          tfoot = '&dagger; A table footer commment')
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<td colspan="6" style="text-align: left;">
Basic table with both column spanners (groups) and row groups
</td>
</tr>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Cgroup 1
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Cgroup 2†
</th>
</tr>
<tr>
<th style="border-bottom: 1px solid grey;">
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
1st header
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
2nd header
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
3rd header
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
4th header
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="6" style="font-weight: 900;">
Group A
</td>
</tr>
<tr>
<td style="text-align: left;">
  1st row
</td>
<td style="text-align: center;">
Content A
</td>
<td style="text-align: center;">
Content B
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
Content C
</td>
<td style="text-align: center;">
Content D
</td>
</tr>
<tr>
<td style="text-align: left;">
  2nd row
</td>
<td style="text-align: center;">
Content E
</td>
<td style="text-align: center;">
Content F
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
Content G
</td>
<td style="text-align: center;">
Content H
</td>
</tr>
<tr>
<td colspan="6" style="font-weight: 900;">
Group B
</td>
</tr>
<tr>
<td style="text-align: left;">
  3rd row
</td>
<td style="text-align: center;">
Content I
</td>
<td style="text-align: center;">
Content J
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
Content K
</td>
<td style="text-align: center;">
Content L
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  4th row
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
Content M
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
Content N
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
Content O
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
Content P
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="6">
† A table footer commment
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
### Example 2

``` r
library(htmlTable)

# given the data in the first row
htmlTable(txtRound(mx, 1), 
          cgroup = cgroup,
          n.cgroup = n.cgroup,
          rgroup = c('First period', 'Second period', 'Third period'),
          n.rgroup = rep(5, 3),
          tfoot = txtMergeLines('&Delta;<sub>int</sub> correspnds to the change since start',
                                '&Delta;<sub>std</sub> corresponds to the change compared to national average'))
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="5" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Sweden
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Norrbotten county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Stockholm county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Uppsala county
</th>
</tr>
<tr>
<th style>
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
</tr>
<tr>
<th style="border-bottom: 1px solid grey;">
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="30" style="font-weight: 900;">
First period
</td>
</tr>
<tr>
<td style="text-align: left;">
  1999
</td>
<td style="text-align: center;">
38.9
</td>
<td style="text-align: center;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.5
</td>
<td style="text-align: center;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.7
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.9
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.3
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.2
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.3
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2000
</td>
<td style="text-align: center;">
39.0
</td>
<td style="text-align: center;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.6
</td>
<td style="text-align: center;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.0
</td>
<td style="text-align: center;">
0.3
</td>
<td style="text-align: center;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.2
</td>
<td style="text-align: center;">
0.3
</td>
<td style="text-align: center;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.4
</td>
<td style="text-align: center;">
0.1
</td>
<td style="text-align: center;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.5
</td>
<td style="text-align: center;">
0.3
</td>
<td style="text-align: center;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.4
</td>
<td style="text-align: center;">
0.1
</td>
<td style="text-align: center;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2001
</td>
<td style="text-align: center;">
39.2
</td>
<td style="text-align: center;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.7
</td>
<td style="text-align: center;">
0.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.2
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.5
</td>
<td style="text-align: center;">
0.6
</td>
<td style="text-align: center;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.5
</td>
<td style="text-align: center;">
0.2
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.6
</td>
<td style="text-align: center;">
0.4
</td>
<td style="text-align: center;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.6
</td>
<td style="text-align: center;">
0.3
</td>
<td style="text-align: center;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2002
</td>
<td style="text-align: center;">
39.3
</td>
<td style="text-align: center;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.8
</td>
<td style="text-align: center;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.5
</td>
<td style="text-align: center;">
0.8
</td>
<td style="text-align: center;">
1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.8
</td>
<td style="text-align: center;">
0.9
</td>
<td style="text-align: center;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.6
</td>
<td style="text-align: center;">
0.3
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.2
</td>
<td style="text-align: center;">
0.1
</td>
<td style="text-align: center;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.8
</td>
<td style="text-align: center;">
0.6
</td>
<td style="text-align: center;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.7
</td>
<td style="text-align: center;">
0.4
</td>
<td style="text-align: center;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2003
</td>
<td style="text-align: center;">
39.4
</td>
<td style="text-align: center;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.9
</td>
<td style="text-align: center;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.7
</td>
<td style="text-align: center;">
1.0
</td>
<td style="text-align: center;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
43.0
</td>
<td style="text-align: center;">
1.1
</td>
<td style="text-align: center;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.7
</td>
<td style="text-align: center;">
0.4
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.2
</td>
<td style="text-align: center;">
0.1
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.0
</td>
<td style="text-align: center;">
0.8
</td>
<td style="text-align: center;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.8
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
-2.1
</td>
</tr>
<tr>
<td colspan="30" style="font-weight: 900;">
Second period
</td>
</tr>
<tr>
<td style="text-align: left;">
  2004
</td>
<td style="text-align: center;">
39.6
</td>
<td style="text-align: center;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.0
</td>
<td style="text-align: center;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.9
</td>
<td style="text-align: center;">
1.2
</td>
<td style="text-align: center;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
43.1
</td>
<td style="text-align: center;">
1.2
</td>
<td style="text-align: center;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.8
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
-1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.3
</td>
<td style="text-align: center;">
0.2
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.1
</td>
<td style="text-align: center;">
0.9
</td>
<td style="text-align: center;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.0
</td>
<td style="text-align: center;">
0.7
</td>
<td style="text-align: center;">
-2.0
</td>
</tr>
<tr>
<td style="text-align: left;">
  2005
</td>
<td style="text-align: center;">
39.7
</td>
<td style="text-align: center;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.0
</td>
<td style="text-align: center;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.1
</td>
<td style="text-align: center;">
1.4
</td>
<td style="text-align: center;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
43.4
</td>
<td style="text-align: center;">
1.5
</td>
<td style="text-align: center;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.9
</td>
<td style="text-align: center;">
0.6
</td>
<td style="text-align: center;">
-1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.3
</td>
<td style="text-align: center;">
0.2
</td>
<td style="text-align: center;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.3
</td>
<td style="text-align: center;">
1.1
</td>
<td style="text-align: center;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
0.8
</td>
<td style="text-align: center;">
-1.9
</td>
</tr>
<tr>
<td style="text-align: left;">
  2006
</td>
<td style="text-align: center;">
39.8
</td>
<td style="text-align: center;">
0.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.1
</td>
<td style="text-align: center;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.3
</td>
<td style="text-align: center;">
1.6
</td>
<td style="text-align: center;">
1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
43.5
</td>
<td style="text-align: center;">
1.6
</td>
<td style="text-align: center;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.9
</td>
<td style="text-align: center;">
0.6
</td>
<td style="text-align: center;">
-1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.2
</td>
<td style="text-align: center;">
0.1
</td>
<td style="text-align: center;">
-1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.5
</td>
<td style="text-align: center;">
1.3
</td>
<td style="text-align: center;">
-1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.4
</td>
<td style="text-align: center;">
1.1
</td>
<td style="text-align: center;">
-1.7
</td>
</tr>
<tr>
<td style="text-align: left;">
  2007
</td>
<td style="text-align: center;">
39.8
</td>
<td style="text-align: center;">
0.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.1
</td>
<td style="text-align: center;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.5
</td>
<td style="text-align: center;">
1.8
</td>
<td style="text-align: center;">
1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
43.8
</td>
<td style="text-align: center;">
1.9
</td>
<td style="text-align: center;">
1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.8
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.6
</td>
<td style="text-align: center;">
1.4
</td>
<td style="text-align: center;">
-1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.5
</td>
<td style="text-align: center;">
1.2
</td>
<td style="text-align: center;">
-1.6
</td>
</tr>
<tr>
<td style="text-align: left;">
  2008
</td>
<td style="text-align: center;">
39.9
</td>
<td style="text-align: center;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.1
</td>
<td style="text-align: center;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.7
</td>
<td style="text-align: center;">
2.0
</td>
<td style="text-align: center;">
1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
44.0
</td>
<td style="text-align: center;">
2.1
</td>
<td style="text-align: center;">
1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.8
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
0.0
</td>
<td style="text-align: center;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.7
</td>
<td style="text-align: center;">
1.5
</td>
<td style="text-align: center;">
-1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.5
</td>
<td style="text-align: center;">
1.2
</td>
<td style="text-align: center;">
-1.6
</td>
</tr>
<tr>
<td colspan="30" style="font-weight: 900;">
Third period
</td>
</tr>
<tr>
<td style="text-align: left;">
  2009
</td>
<td style="text-align: center;">
39.9
</td>
<td style="text-align: center;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.1
</td>
<td style="text-align: center;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
41.9
</td>
<td style="text-align: center;">
2.2
</td>
<td style="text-align: center;">
2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
44.2
</td>
<td style="text-align: center;">
2.3
</td>
<td style="text-align: center;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.8
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.0
</td>
<td style="text-align: center;">
-0.1
</td>
<td style="text-align: center;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.8
</td>
<td style="text-align: center;">
1.6
</td>
<td style="text-align: center;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.6
</td>
<td style="text-align: center;">
1.3
</td>
<td style="text-align: center;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2010
</td>
<td style="text-align: center;">
40.0
</td>
<td style="text-align: center;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.1
</td>
<td style="text-align: center;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.1
</td>
<td style="text-align: center;">
2.4
</td>
<td style="text-align: center;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
44.4
</td>
<td style="text-align: center;">
2.5
</td>
<td style="text-align: center;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.8
</td>
<td style="text-align: center;">
0.5
</td>
<td style="text-align: center;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.0
</td>
<td style="text-align: center;">
-0.1
</td>
<td style="text-align: center;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
38.9
</td>
<td style="text-align: center;">
1.7
</td>
<td style="text-align: center;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.6
</td>
<td style="text-align: center;">
1.3
</td>
<td style="text-align: center;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2011
</td>
<td style="text-align: center;">
40.1
</td>
<td style="text-align: center;">
1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.2
</td>
<td style="text-align: center;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.3
</td>
<td style="text-align: center;">
2.6
</td>
<td style="text-align: center;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
44.5
</td>
<td style="text-align: center;">
2.6
</td>
<td style="text-align: center;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.9
</td>
<td style="text-align: center;">
0.6
</td>
<td style="text-align: center;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.9
</td>
<td style="text-align: center;">
-0.2
</td>
<td style="text-align: center;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.0
</td>
<td style="text-align: center;">
1.8
</td>
<td style="text-align: center;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.7
</td>
<td style="text-align: center;">
1.4
</td>
<td style="text-align: center;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2012
</td>
<td style="text-align: center;">
40.2
</td>
<td style="text-align: center;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.2
</td>
<td style="text-align: center;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
42.4
</td>
<td style="text-align: center;">
2.7
</td>
<td style="text-align: center;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
44.6
</td>
<td style="text-align: center;">
2.7
</td>
<td style="text-align: center;">
2.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
37.9
</td>
<td style="text-align: center;">
0.6
</td>
<td style="text-align: center;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.9
</td>
<td style="text-align: center;">
-0.2
</td>
<td style="text-align: center;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
39.1
</td>
<td style="text-align: center;">
1.9
</td>
<td style="text-align: center;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: center;">
40.8
</td>
<td style="text-align: center;">
1.5
</td>
<td style="text-align: center;">
-1.4
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  2013
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
40.2
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
1.3
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
42.2
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
0.7
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
42.4
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
2.7
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
44.7
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
2.8
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
2.5
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
38.0
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
0.7
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
-2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
39.9
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
-0.2
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
-2.3
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
39.2
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
2.0
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
-1.0
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
40.9
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
1.6
</td>
<td style="border-bottom: 2px solid grey; text-align: center;">
-1.3
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="30">
Δ<sub>int</sub> correspnds to the change since start<br><br>
Δ<sub>std</sub> corresponds to the change compared to national average
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
### Example 3

``` r
library(htmlTable)

# given the data in the first row
htmlTable(txtRound(mx, 1), 
          align = 'rrrr|r',
          cgroup = cgroup,
          n.cgroup = n.cgroup,
          rgroup = c('First period', 'Second period', 'Third period'),
          n.rgroup = rep(5, 3),
          tfoot = txtMergeLines('&Delta;<sub>int</sub> correspnds to the change since start',
                                '&Delta;<sub>std</sub> corresponds to the change compared to national average'))
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="5" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Sweden
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Norrbotten county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Stockholm county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Uppsala county
</th>
</tr>
<tr>
<th style>
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
</tr>
<tr>
<th style="border-bottom: 1px solid grey;">
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="30" style="font-weight: 900;">
First period
</td>
</tr>
<tr>
<td style="text-align: left;">
  1999
</td>
<td style="text-align: right;">
38.9
</td>
<td style="text-align: right;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.5
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.2
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2000
</td>
<td style="text-align: right;">
39.0
</td>
<td style="text-align: right;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.6
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2001
</td>
<td style="text-align: right;">
39.2
</td>
<td style="text-align: right;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.7
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.5
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2002
</td>
<td style="text-align: right;">
39.3
</td>
<td style="text-align: right;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.8
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2003
</td>
<td style="text-align: right;">
39.4
</td>
<td style="text-align: right;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.0
</td>
<td style="text-align: right;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.0
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td colspan="30" style="font-weight: 900;">
Second period
</td>
</tr>
<tr>
<td style="text-align: left;">
  2004
</td>
<td style="text-align: right;">
39.6
</td>
<td style="text-align: right;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.9
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.1
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.3
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.1
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.7
</td>
<td style="text-align: right;">
-2.0
</td>
</tr>
<tr>
<td style="text-align: left;">
  2005
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.1
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.4
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.3
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.3
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
-1.9
</td>
</tr>
<tr>
<td style="text-align: left;">
  2006
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.3
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.5
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.5
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.4
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
-1.7
</td>
</tr>
<tr>
<td style="text-align: left;">
  2007
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.5
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.8
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.6
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
-1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
-1.6
</td>
</tr>
<tr>
<td style="text-align: left;">
  2008
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.7
</td>
<td style="text-align: right;">
2.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.0
</td>
<td style="text-align: right;">
2.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.7
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
-1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
-1.6
</td>
</tr>
<tr>
<td colspan="30" style="font-weight: 900;">
Third period
</td>
</tr>
<tr>
<td style="text-align: left;">
  2009
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
2.2
</td>
<td style="text-align: right;">
2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.2
</td>
<td style="text-align: right;">
2.3
</td>
<td style="text-align: right;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.8
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2010
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="text-align: right;">
2.4
</td>
<td style="text-align: right;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.4
</td>
<td style="text-align: right;">
2.5
</td>
<td style="text-align: right;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2011
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.3
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.5
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2012
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.4
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.6
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
2.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.8
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
-1.4
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  2013
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
40.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
1.3
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
42.2
</td>
<td style="border-bottom: 2px solid grey; border-right: 1px solid black; text-align: right;">
0.7
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
42.4
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
44.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.8
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.5
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
38.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
0.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-0.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-2.3
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-1.0
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
40.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
1.6
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-1.3
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="30">
Δ<sub>int</sub> correspnds to the change since start<br><br>
Δ<sub>std</sub> corresponds to the change compared to national average
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
### Example 4

``` r
library(htmlTable)

# given the data in the first row
htmlTable(txtRound(mx, 1), 
          col.columns = c(rep('#E6E6F0', 4),
                          rep('none', ncol(mx) - 4)),
          align = 'rrrr|r',
          cgroup = cgroup,
          n.cgroup = n.cgroup,
          rgroup = c('First period', 'Second period', 'Third period'),
          n.rgroup = rep(5, 3),
                    tfoot = txtMergeLines('&Delta;<sub>int</sub> correspnds to the change since start',
                                          '&Delta;<sub>std</sub> corresponds to the change compared to national average'))
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="5" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Sweden
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Norrbotten county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Stockholm county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Uppsala county
</th>
</tr>
<tr>
<th style>
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
</tr>
<tr>
<th style="border-bottom: 1px solid grey;">
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="30" style="font-weight: 900;">
First period
</td>
</tr>
<tr>
<td style="text-align: left;">
  1999
</td>
<td style="text-align: right; background-color: #e6e6f0;">
38.9
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.0
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
41.5
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.2
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2000
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.0
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.1
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
41.6
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2001
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.2
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.3
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
41.7
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.5
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2002
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.3
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.4
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
41.8
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2003
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.4
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.5
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
41.9
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.0
</td>
<td style="text-align: right;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.0
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td colspan="30" style="font-weight: 900;">
Second period
</td>
</tr>
<tr>
<td style="text-align: left;">
  2004
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.6
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.7
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.9
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.1
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.3
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.1
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.7
</td>
<td style="text-align: right;">
-2.0
</td>
</tr>
<tr>
<td style="text-align: left;">
  2005
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.7
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.8
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.1
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.4
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.3
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.3
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
-1.9
</td>
</tr>
<tr>
<td style="text-align: left;">
  2006
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.8
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.9
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.3
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.5
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.5
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.4
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
-1.7
</td>
</tr>
<tr>
<td style="text-align: left;">
  2007
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.8
</td>
<td style="text-align: right; background-color: #e6e6f0;">
0.9
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.5
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.8
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.6
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
-1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
-1.6
</td>
</tr>
<tr>
<td style="text-align: left;">
  2008
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.9
</td>
<td style="text-align: right; background-color: #e6e6f0;">
1.0
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.7
</td>
<td style="text-align: right;">
2.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.0
</td>
<td style="text-align: right;">
2.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.7
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
-1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
1.2
</td>
<td style="text-align: right;">
-1.6
</td>
</tr>
<tr>
<td colspan="30" style="font-weight: 900;">
Third period
</td>
</tr>
<tr>
<td style="text-align: left;">
  2009
</td>
<td style="text-align: right; background-color: #e6e6f0;">
39.9
</td>
<td style="text-align: right; background-color: #e6e6f0;">
1.0
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
2.2
</td>
<td style="text-align: right;">
2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.2
</td>
<td style="text-align: right;">
2.3
</td>
<td style="text-align: right;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.8
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2010
</td>
<td style="text-align: right; background-color: #e6e6f0;">
40.0
</td>
<td style="text-align: right; background-color: #e6e6f0;">
1.1
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="text-align: right;">
2.4
</td>
<td style="text-align: right;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.4
</td>
<td style="text-align: right;">
2.5
</td>
<td style="text-align: right;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2011
</td>
<td style="text-align: right; background-color: #e6e6f0;">
40.1
</td>
<td style="text-align: right; background-color: #e6e6f0;">
1.2
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.3
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.5
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2012
</td>
<td style="text-align: right; background-color: #e6e6f0;">
40.2
</td>
<td style="text-align: right; background-color: #e6e6f0;">
1.3
</td>
<td style="background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #e6e6f0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.4
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.6
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
2.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.8
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
-1.4
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  2013
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #e6e6f0;">
40.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #e6e6f0;">
1.3
</td>
<td style="border-bottom: 2px solid grey; background-color: #e6e6f0;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #e6e6f0;">
42.2
</td>
<td style="border-bottom: 2px solid grey; border-right: 1px solid black; text-align: right; background-color: #e6e6f0;">
0.7
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
42.4
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
44.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.8
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.5
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
38.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
0.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-0.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-2.3
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-1.0
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
40.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
1.6
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-1.3
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="30">
Δ<sub>int</sub> correspnds to the change since start<br><br>
Δ<sub>std</sub> corresponds to the change compared to national average
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
### Example 5

``` r
library(htmlTable)

# given the data in the first row
htmlTable(txtRound(mx, 1),
          col.rgroup = c('none', '#FFFFCC'),
          col.columns = c(rep('#EFEFF0', 4),
                          rep('none', ncol(mx) - 4)),
          align = 'rrrr|r',
          cgroup = cgroup,
          n.cgroup = n.cgroup,
          # I use the &nbsp; - the no breaking space as I don't want to have a
          # row break in the row group. This adds a little space in the table
          # when used together with the cspan.rgroup=1.
          rgroup = c('1st&nbsp;period', '2nd&nbsp;period', '3rd&nbsp;period'),
          n.rgroup = rep(5, 3),
          tfoot = txtMergeLines('&Delta;<sub>int</sub> correspnds to the change since start',
                                '&Delta;<sub>std</sub> corresponds to the change compared to national average'),
          cspan.rgroup = 1)
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="5" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Sweden
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Norrbotten county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Stockholm county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Uppsala county
</th>
</tr>
<tr>
<th style>
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
</tr>
<tr>
<th style="border-bottom: 1px solid grey;">
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="1" style="font-weight: 900;">
1st period
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; background-color: #efeff0;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
</tr>
<tr>
<td style="text-align: left;">
  1999
</td>
<td style="text-align: right; background-color: #efeff0;">
38.9
</td>
<td style="text-align: right; background-color: #efeff0;">
0.0
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.5
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.2
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2000
</td>
<td style="text-align: right; background-color: #efeff0;">
39.0
</td>
<td style="text-align: right; background-color: #efeff0;">
0.1
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.6
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-2.2
</td>
</tr>
<tr>
<td style="text-align: left;">
  2001
</td>
<td style="text-align: right; background-color: #efeff0;">
39.2
</td>
<td style="text-align: right; background-color: #efeff0;">
0.3
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.7
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.5
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
0.8
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2002
</td>
<td style="text-align: right; background-color: #efeff0;">
39.3
</td>
<td style="text-align: right; background-color: #efeff0;">
0.4
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.8
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
1.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
1.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-1.5
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td style="text-align: left;">
  2003
</td>
<td style="text-align: right; background-color: #efeff0;">
39.4
</td>
<td style="text-align: right; background-color: #efeff0;">
0.5
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.9
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.0
</td>
<td style="text-align: right;">
1.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.0
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
-1.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
-1.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
</tr>
<tr>
<td colspan="1" style="font-weight: 900; background-color: #ffffcc;">
2nd period
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2004
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.6
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.7
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.3
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.8
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.7
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
-2.0
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2005
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.7
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.8
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.8
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.7
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.4
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.9
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2006
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.8
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.9
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.9
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.9
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.3
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.7
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2007
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.8
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.9
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.7
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.7
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
-2.0
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
-2.0
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.2
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.6
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2008
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.9
</td>
<td style="text-align: right; background-color: #F7F7DE;">
1.0
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
2.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.8
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
44.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
2.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.9
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
-2.1
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
-2.0
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.2
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
-1.6
</td>
</tr>
<tr>
<td colspan="1" style="font-weight: 900;">
3rd period
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; background-color: #efeff0;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
</tr>
<tr>
<td style="text-align: left;">
  2009
</td>
<td style="text-align: right; background-color: #efeff0;">
39.9
</td>
<td style="text-align: right; background-color: #efeff0;">
1.0
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
2.2
</td>
<td style="text-align: right;">
2.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.2
</td>
<td style="text-align: right;">
2.3
</td>
<td style="text-align: right;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.8
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2010
</td>
<td style="text-align: right; background-color: #efeff0;">
40.0
</td>
<td style="text-align: right; background-color: #efeff0;">
1.1
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="text-align: right;">
2.4
</td>
<td style="text-align: right;">
2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.4
</td>
<td style="text-align: right;">
2.5
</td>
<td style="text-align: right;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
-2.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2011
</td>
<td style="text-align: right; background-color: #efeff0;">
40.1
</td>
<td style="text-align: right; background-color: #efeff0;">
1.2
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.3
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.5
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
-1.5
</td>
</tr>
<tr>
<td style="text-align: left;">
  2012
</td>
<td style="text-align: right; background-color: #efeff0;">
40.2
</td>
<td style="text-align: right; background-color: #efeff0;">
1.3
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.4
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
2.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.6
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
2.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
-2.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
-1.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.8
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
-1.4
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  2013
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
40.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
1.3
</td>
<td style="border-bottom: 2px solid grey; background-color: #efeff0;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-bottom: 2px solid grey; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
42.4
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
44.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.8
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.5
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
38.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
0.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-2.2
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-0.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-2.3
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-1.0
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
40.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
1.6
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-1.3
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="30">
Δ<sub>int</sub> correspnds to the change since start<br><br>
Δ<sub>std</sub> corresponds to the change compared to national average
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
### Example 6

``` r
library(htmlTable)

# given the data in the first row
htmlTable(out_mx,
          caption = 'Average age in Sweden counties over a period of
                     15 years. The Norbotten county is typically known
                     for having a negative migration pattern compared to
                     Stockholm, while Uppsala has a proportionally large 
                     population of students.',
          pos.rowlabel = 'bottom',
          rowlabel='Year', 
          col.rgroup = c('none', '#FFFFCC'),
          col.columns = c(rep('#EFEFF0', 4),
                          rep('none', ncol(mx) - 4)),
          align = 'rrrr|r',
          cgroup = cgroup,
          n.cgroup = n.cgroup,
          rgroup = c('1st&nbsp;period', '2nd&nbsp;period', '3rd&nbsp;period'),
          n.rgroup = rep(5, 3),
          tfoot = txtMergeLines('&Delta;<sub>int</sub> correspnds to the change since start',
                                '&Delta;<sub>std</sub> corresponds to the change compared to national average'),
          cspan.rgroup = 1)
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<td colspan="30" style="text-align: left;">
Average age in Sweden counties over a period of 15 years. The Norbotten
county is typically known for having a negative migration pattern
compared to Stockholm, while Uppsala has a proportionally large
population of students.
</td>
</tr>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="5" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Sweden
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Norrbotten county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Stockholm county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Uppsala county
</th>
</tr>
<tr>
<th style>
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
</tr>
<tr>
<th style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Year
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="1" style="font-weight: 900;">
1st period
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; background-color: #efeff0;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
</tr>
<tr>
<td style="text-align: left;">
  1999
</td>
<td style="text-align: right; background-color: #efeff0;">
38.9
</td>
<td style="text-align: right; background-color: #efeff0;">
0.0
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.5
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #2D000F">0.8</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #120006">0.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.2
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2000
</td>
<td style="text-align: right; background-color: #efeff0;">
39.0
</td>
<td style="text-align: right; background-color: #efeff0;">
0.1
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.6
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #3A0013">1.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #1E000A">0.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2001
</td>
<td style="text-align: right; background-color: #efeff0;">
39.2
</td>
<td style="text-align: right; background-color: #efeff0;">
0.3
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.7
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #3A0013">1.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.5
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #2D000F">0.8</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2002
</td>
<td style="text-align: right; background-color: #efeff0;">
39.3
</td>
<td style="text-align: right; background-color: #efeff0;">
0.4
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.8
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #460017">1.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #3A0013">1.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2003
</td>
<td style="text-align: right; background-color: #efeff0;">
39.4
</td>
<td style="text-align: right; background-color: #efeff0;">
0.5
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.9
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #4C0019">1.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.0
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #400015">1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
</tr>
<tr>
<td colspan="1" style="font-weight: 900; background-color: #ffffcc;">
2nd period
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2004
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.6
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.7
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #4C0019">1.3</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #400015">1.1</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007A00">-1.8</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2005
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.7
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.8
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #52001B">1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #52001B">1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007A00">-1.8</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008000">-1.9</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2006
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.8
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.9
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #58001D">1.5</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #52001B">1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008000">-1.9</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008000">-1.9</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005800">-1.3</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2007
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.8
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.9
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #640021">1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #640021">1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005200">-1.2</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2008
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.9
</td>
<td style="text-align: right; background-color: #F7F7DE;">
1.0
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
2.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #6B0023">1.8</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
44.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
2.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #710025">1.9</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005200">-1.2</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
</tr>
<tr>
<td colspan="1" style="font-weight: 900;">
3rd period
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; background-color: #efeff0;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
</tr>
<tr>
<td style="text-align: left;">
  2009
</td>
<td style="text-align: right; background-color: #efeff0;">
39.9
</td>
<td style="text-align: right; background-color: #efeff0;">
1.0
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
2.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #7A0028">2.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.2
</td>
<td style="text-align: right;">
2.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #80002A">2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.8
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2010
</td>
<td style="text-align: right; background-color: #efeff0;">
40.0
</td>
<td style="text-align: right; background-color: #efeff0;">
1.1
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="text-align: right;">
2.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #80002A">2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.4
</td>
<td style="text-align: right;">
2.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #8C002E">2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2011
</td>
<td style="text-align: right; background-color: #efeff0;">
40.1
</td>
<td style="text-align: right; background-color: #efeff0;">
1.2
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.3
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #86002C">2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.5
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #8C002E">2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2012
</td>
<td style="text-align: right; background-color: #efeff0;">
40.2
</td>
<td style="text-align: right; background-color: #efeff0;">
1.3
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.4
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #86002C">2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.6
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #920030">2.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.8
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  2013
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
40.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
1.3
</td>
<td style="border-bottom: 2px solid grey; background-color: #efeff0;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-bottom: 2px solid grey; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
42.4
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #86002C">2.2</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
44.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.8
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #990033">2.5</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
38.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
0.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-0.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #004600">-1.0</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
40.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
1.6
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #005800">-1.3</span>
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="30">
Δ<sub>int</sub> correspnds to the change since start<br><br>
Δ<sub>std</sub> corresponds to the change compared to national average
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
### Example 7

``` r
library(htmlTable)

# given the data in the first row
htmlTable(out_mx,
          caption = 'Average age in Sweden counties over a period of
                     15 years. The Norbotten county is typically known
                     for having a negative migration pattern compared to
                     Stockholm, while Uppsala has a proportionally large 
                     population of students.',
          pos.rowlabel = 'bottom',
          rowlabel = 'Year', 
          col.rgroup = c('none', '#FFFFCC'),
          col.columns = c(rep('#EFEFF0', 4), rep('none', ncol(mx) - 4)),
          align = 'rrrr|r',
          cgroup = cgroup,
          n.cgroup = n.cgroup,
          rgroup = c('1st&nbsp;period', '2nd&nbsp;period', '3rd&nbsp;period'),
          n.rgroup = rep(5, 3),
          tfoot = txtMergeLines('&Delta;<sub>int</sub> correspnds to the change since start',
                                '&Delta;<sub>std</sub> corresponds to the change compared to national average'),
          cspan.rgroup = 1)
```

<!--html_preserve-->
<table class="gmisc_table" style="border-collapse: collapse; margin-top: 1em; margin-bottom: 1em;">
<thead>
<tr>
<td colspan="30" style="text-align: left;">
Average age in Sweden counties over a period of 15 years. The Norbotten
county is typically known for having a negative migration pattern
compared to Stockholm, while Uppsala has a proportionally large
population of students.
</td>
</tr>
<tr>
<th style="border-top: 2px solid grey;">
</th>
<th colspan="5" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Sweden
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Norrbotten county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Stockholm county
</th>
<th style="border-top: 2px solid grey;; border-bottom: hidden;">
 
</th>
<th colspan="7" style="font-weight: 900; border-bottom: 1px solid grey; border-top: 2px solid grey; text-align: center;">
Uppsala county
</th>
</tr>
<tr>
<th style>
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="2" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Men
</th>
<th style="; border-bottom: hidden;">
 
</th>
<th colspan="3" style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Women
</th>
</tr>
<tr>
<th style="font-weight: 900; border-bottom: 1px solid grey; text-align: center;">
Year
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
<th style="border-bottom: 1px solid grey;" colspan="1">
 
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Age
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>int</sub>
</th>
<th style="border-bottom: 1px solid grey; text-align: center;">
Δ<sub>std</sub>
</th>
</tr>
</thead>
<tbody>
<tr>
<td colspan="1" style="font-weight: 900;">
1st period
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; background-color: #efeff0;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
</tr>
<tr>
<td style="text-align: left;">
  1999
</td>
<td style="text-align: right; background-color: #efeff0;">
38.9
</td>
<td style="text-align: right; background-color: #efeff0;">
0.0
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.5
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.0
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #2D000F">0.8</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #120006">0.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.2
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.3
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2000
</td>
<td style="text-align: right; background-color: #efeff0;">
39.0
</td>
<td style="text-align: right; background-color: #efeff0;">
0.1
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.6
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.1
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #3A0013">1.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.2
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #1E000A">0.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.4
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2001
</td>
<td style="text-align: right; background-color: #efeff0;">
39.2
</td>
<td style="text-align: right; background-color: #efeff0;">
0.3
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.7
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.2
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #3A0013">1.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.5
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #2D000F">0.8</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.5
</td>
<td style="text-align: right;">
0.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.1
</td>
<td style="text-align: right;">
0.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2002
</td>
<td style="text-align: right; background-color: #efeff0;">
39.3
</td>
<td style="text-align: right; background-color: #efeff0;">
0.4
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.8
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.3
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.5
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #460017">1.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.8
</td>
<td style="text-align: right;">
0.9
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #3A0013">1.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.6
</td>
<td style="text-align: right;">
0.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2003
</td>
<td style="text-align: right; background-color: #efeff0;">
39.4
</td>
<td style="text-align: right; background-color: #efeff0;">
0.5
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
41.9
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.4
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.0
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #4C0019">1.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
43.0
</td>
<td style="text-align: right;">
1.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #400015">1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.7
</td>
<td style="text-align: right;">
0.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.2
</td>
<td style="text-align: right;">
0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.0
</td>
<td style="text-align: right;">
0.8
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
</tr>
<tr>
<td colspan="1" style="font-weight: 900; background-color: #ffffcc;">
2nd period
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc;" colspan="1">
 
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
<td style="font-weight: 900; background-color: #ffffcc; text-align: right;">
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2004
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.6
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.7
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #4C0019">1.3</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #400015">1.1</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007A00">-1.8</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2005
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.7
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.8
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.0
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.5
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #52001B">1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #52001B">1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007A00">-1.8</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008000">-1.9</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2006
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.8
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.9
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #58001D">1.5</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #52001B">1.4</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008000">-1.9</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008000">-1.9</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.3
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005800">-1.3</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #007100">-1.7</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2007
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.8
</td>
<td style="text-align: right; background-color: #F7F7DE;">
0.9
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #640021">1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
43.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.9
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #640021">1.7</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.6
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.4
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005200">-1.2</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
</tr>
<tr style="background-color: #ffffcc;">
<td style="background-color: #ffffcc; text-align: left;">
  2008
</td>
<td style="text-align: right; background-color: #F7F7DE;">
39.9
</td>
<td style="text-align: right; background-color: #F7F7DE;">
1.0
</td>
<td style="background-color: #F7F7DE;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #F7F7DE;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #F7F7DE;">
0.6
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
41.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
2.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #6B0023">1.8</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
44.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
2.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #710025">1.9</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
37.8
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.1
</td>
<td style="background-color: #ffffcc; text-align: right;">
0.0
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #008600">-2.0</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
38.7
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #005200">-1.2</span>
</td>
<td style="background-color: #ffffcc;" colspan="1">
 
</td>
<td style="background-color: #ffffcc; text-align: right;">
40.5
</td>
<td style="background-color: #ffffcc; text-align: right;">
1.2
</td>
<td style="background-color: #ffffcc; text-align: right;">
<span style="font-weight: 900; color: #006B00">-1.6</span>
</td>
</tr>
<tr>
<td colspan="1" style="font-weight: 900;">
3rd period
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; background-color: #efeff0;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900;" colspan="1">
 
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
<td style="font-weight: 900; text-align: right;">
</td>
</tr>
<tr>
<td style="text-align: left;">
  2009
</td>
<td style="text-align: right; background-color: #efeff0;">
39.9
</td>
<td style="text-align: right; background-color: #efeff0;">
1.0
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
41.9
</td>
<td style="text-align: right;">
2.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #7A0028">2.0</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.2
</td>
<td style="text-align: right;">
2.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #80002A">2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.8
</td>
<td style="text-align: right;">
1.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2010
</td>
<td style="text-align: right; background-color: #efeff0;">
40.0
</td>
<td style="text-align: right; background-color: #efeff0;">
1.1
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.1
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.6
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.1
</td>
<td style="text-align: right;">
2.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #80002A">2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.4
</td>
<td style="text-align: right;">
2.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #8C002E">2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.8
</td>
<td style="text-align: right;">
0.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.0
</td>
<td style="text-align: right;">
-0.1
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #008C00">-2.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
38.9
</td>
<td style="text-align: right;">
1.7
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.6
</td>
<td style="text-align: right;">
1.3
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2011
</td>
<td style="text-align: right; background-color: #efeff0;">
40.1
</td>
<td style="text-align: right; background-color: #efeff0;">
1.2
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.3
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #86002C">2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.5
</td>
<td style="text-align: right;">
2.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #8C002E">2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.0
</td>
<td style="text-align: right;">
1.8
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.7
</td>
<td style="text-align: right;">
1.4
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #006400">-1.5</span>
</td>
</tr>
<tr>
<td style="text-align: left;">
  2012
</td>
<td style="text-align: right; background-color: #efeff0;">
40.2
</td>
<td style="text-align: right; background-color: #efeff0;">
1.3
</td>
<td style="background-color: #efeff0;" colspan="1">
 
</td>
<td style="text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
42.4
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #86002C">2.2</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
44.6
</td>
<td style="text-align: right;">
2.7
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #920030">2.4</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
37.9
</td>
<td style="text-align: right;">
0.6
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.9
</td>
<td style="text-align: right;">
-0.2
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
39.1
</td>
<td style="text-align: right;">
1.9
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #004C00">-1.1</span>
</td>
<td style colspan="1">
 
</td>
<td style="text-align: right;">
40.8
</td>
<td style="text-align: right;">
1.5
</td>
<td style="text-align: right;">
<span style="font-weight: 900; color: #005E00">-1.4</span>
</td>
</tr>
<tr>
<td style="border-bottom: 2px solid grey; text-align: left;">
  2013
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
40.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
1.3
</td>
<td style="border-bottom: 2px solid grey; background-color: #efeff0;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right; background-color: #efeff0;">
42.2
</td>
<td style="border-bottom: 2px solid grey; border-right: 1px solid black; text-align: right; background-color: #efeff0;">
0.7
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
42.4
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #86002C">2.2</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
44.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.8
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #990033">2.5</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
38.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
0.7
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #009200">-2.2</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
-0.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #009900">-2.3</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
39.2
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
2.0
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #004600">-1.0</span>
</td>
<td style="border-bottom: 2px solid grey;" colspan="1">
 
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
40.9
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
1.6
</td>
<td style="border-bottom: 2px solid grey; text-align: right;">
<span style="font-weight: 900; color: #005800">-1.3</span>
</td>
</tr>
</tbody>
<tfoot>
<tr>
<td colspan="30">
Δ<sub>int</sub> correspnds to the change since start<br><br>
Δ<sub>std</sub> corresponds to the change compared to national average
</td>
</tr>
</tfoot>
</table>
<!--/html_preserve-->
The `ztable` package
--------------------

The package can also export to *L**a**T**e**X*.

### Example 1

``` r
library(ztable)

options(ztable.type='html')

# given the data in the first row
zt <- ztable(out_mx, 
             caption = 'Average age in Sweden counties over a period of
             15 years. The Norbotten county is typically known
             for having a negative migration pattern compared to
             Stockholm, while Uppsala has a proportionally large 
             population of students.',
             zebra.type = 1,
             zebra = 'peach',
             align=paste(rep('r', ncol(out_mx) + 1), collapse = ''))
# zt <- addcgroup(zt,
#                 cgroup = cgroup,
#                 n.cgroup = n.cgroup)
# Causes an error:
# Error in if (result <= length(vlines)) { : 
zt <- addrgroup(zt, 
                rgroup = c('1st&nbsp;period', '2nd&nbsp;period', '3rd&nbsp;period'),
                n.rgroup = rep(5, 3))

print(zt)
```
