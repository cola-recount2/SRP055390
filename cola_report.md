cola Report for Consensus Partitioning
==================

**Date**: 2019-12-05 08:02:51 CET, **cola version**: 1.3.2

----------------------------------------------------------------

<style type='text/css'>

body, td, th {
   font-family: Arial,Helvetica,sans-serif;
   background-color: white;
   font-size: 13px;
  max-width: 800px;
  margin: auto;
  margin-left:210px;
  padding: 0px 10px 0px 10px;
  border-left: 1px solid #EEEEEE;
  line-height: 150%;
}

tt, code, pre {
   font-family: 'DejaVu Sans Mono', 'Droid Sans Mono', 'Lucida Console', Consolas, Monaco, 

monospace;
}

h1 {
   font-size:2.2em;
}

h2 {
   font-size:1.8em;
}

h3 {
   font-size:1.4em;
}

h4 {
   font-size:1.0em;
}

h5 {
   font-size:0.9em;
}

h6 {
   font-size:0.8em;
}

a {
  text-decoration: none;
  color: #0366d6;
}

a:hover {
  text-decoration: underline;
}

a:visited {
   color: #0366d6;
}

pre, img {
  max-width: 100%;
}
pre {
  overflow-x: auto;
}
pre code {
   display: block; padding: 0.5em;
}

code {
  font-size: 92%;
  border: 1px solid #ccc;
}

code[class] {
  background-color: #F8F8F8;
}

table, td, th {
  border: 1px solid #ccc;
}

blockquote {
   color:#666666;
   margin:0;
   padding-left: 1em;
   border-left: 0.5em #EEE solid;
}

hr {
   height: 0px;
   border-bottom: none;
   border-top-width: thin;
   border-top-style: dotted;
   border-top-color: #999999;
}

@media print {
   * {
      background: transparent !important;
      color: black !important;
      filter:none !important;
      -ms-filter: none !important;
   }

   body {
      font-size:12pt;
      max-width:100%;
   }

   a, a:visited {
      text-decoration: underline;
   }

   hr {
      visibility: hidden;
      page-break-before: always;
   }

   pre, blockquote {
      padding-right: 1em;
      page-break-inside: avoid;
   }

   tr, img {
      page-break-inside: avoid;
   }

   img {
      max-width: 100% !important;
   }

   @page :left {
      margin: 15mm 20mm 15mm 10mm;
   }

   @page :right {
      margin: 15mm 10mm 15mm 20mm;
   }

   p, h2, h3 {
      orphans: 3; widows: 3;
   }

   h2, h3 {
      page-break-after: avoid;
   }
}
</style>




## Summary





All available functions which can be applied to this `res_list` object:


```r
res_list
```

```
#> A 'ConsensusPartitionList' object with 24 methods.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows are extracted by 'SD, CV, MAD, ATC' methods.
#>   Subgroups are detected by 'hclust, kmeans, skmeans, pam, mclust, NMF' method.
#>   Number of partitions are tried for k = 2, 3, 4, 5, 6.
#>   Performed in total 30000 partitions by row resampling.
#> 
#> Following methods can be applied to this 'ConsensusPartitionList' object:
#>  [1] "cola_report"           "collect_classes"       "collect_plots"         "collect_stats"        
#>  [5] "colnames"              "functional_enrichment" "get_anno_col"          "get_anno"             
#>  [9] "get_classes"           "get_matrix"            "get_membership"        "get_stats"            
#> [13] "is_best_k"             "is_stable_k"           "ncol"                  "nrow"                 
#> [17] "rownames"              "show"                  "suggest_best_k"        "test_to_known_factors"
#> [21] "top_rows_heatmap"      "top_rows_overlap"     
#> 
#> You can get result for a single method by, e.g. object["SD", "hclust"] or object["SD:hclust"]
#> or a subset of methods by object[c("SD", "CV")], c("hclust", "kmeans")]
```

The call of `run_all_consensus_partition_methods()` was:


```
#> run_all_consensus_partition_methods(data = mat, mc.cores = 4)
```

Dimension of the input matrix:


```r
mat = get_matrix(res_list)
dim(mat)
```

```
#> [1] 14626    51
```

### Density distribution

The density distribution for each sample is visualized as in one column in the
following heatmap. The clustering is based on the distance which is the
Kolmogorov-Smirnov statistic between two distributions.




```r
library(ComplexHeatmap)
densityHeatmap(mat, ylab = "value", cluster_columns = TRUE, show_column_names = FALSE,
    mc.cores = 4)
```

![plot of chunk density-heatmap](figure_cola/density-heatmap-1.png)





### Suggest the best k



Folowing table shows the best `k` (number of partitions) for each combination
of top-value methods and partition methods. Clicking on the method name in
the table goes to the section for a single combination of methods.

[The cola vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13)
explains the definition of the metrics used for determining the best
number of partitions.


```r
suggest_best_k(res_list)
```


|                            | The best k| 1-PAC| Mean silhouette| Concordance|   |Optional k |
|:---------------------------|----------:|-----:|---------------:|-----------:|:--|:----------|
|[CV:hclust](#CV-hclust)     |          2| 1.000|           0.966|       0.987|** |           |
|[CV:kmeans](#CV-kmeans)     |          2| 1.000|           0.996|       0.997|** |           |
|[CV:skmeans](#CV-skmeans)   |          2| 1.000|           0.971|       0.984|** |           |
|[CV:pam](#CV-pam)           |          2| 1.000|           0.991|       0.996|** |           |
|[CV:NMF](#CV-NMF)           |          2| 1.000|           0.971|       0.990|** |           |
|[ATC:NMF](#ATC-NMF)         |          2| 0.910|           0.903|       0.960|*  |           |
|[ATC:pam](#ATC-pam)         |          5| 0.909|           0.876|       0.950|*  |2,4        |
|[ATC:skmeans](#ATC-skmeans) |          6| 0.909|           0.896|       0.921|*  |2          |
|[SD:NMF](#SD-NMF)           |          3| 0.869|           0.902|       0.959|   |           |
|[SD:pam](#SD-pam)           |          4| 0.822|           0.880|       0.949|   |           |
|[MAD:NMF](#MAD-NMF)         |          3| 0.793|           0.858|       0.923|   |           |
|[ATC:kmeans](#ATC-kmeans)   |          2| 0.769|           0.885|       0.929|   |           |
|[MAD:pam](#MAD-pam)         |          2| 0.764|           0.879|       0.949|   |           |
|[ATC:hclust](#ATC-hclust)   |          5| 0.702|           0.660|       0.771|   |           |
|[SD:kmeans](#SD-kmeans)     |          5| 0.660|           0.746|       0.792|   |           |
|[MAD:skmeans](#MAD-skmeans) |          2| 0.645|           0.897|       0.948|   |           |
|[SD:hclust](#SD-hclust)     |          4| 0.580|           0.698|       0.862|   |           |
|[SD:skmeans](#SD-skmeans)   |          2| 0.546|           0.774|       0.893|   |           |
|[ATC:mclust](#ATC-mclust)   |          4| 0.490|           0.699|       0.801|   |           |
|[SD:mclust](#SD-mclust)     |          3| 0.434|           0.588|       0.799|   |           |
|[CV:mclust](#CV-mclust)     |          3| 0.401|           0.603|       0.741|   |           |
|[MAD:mclust](#MAD-mclust)   |          5| 0.382|           0.581|       0.724|   |           |
|[MAD:kmeans](#MAD-kmeans)   |          2| 0.369|           0.781|       0.862|   |           |
|[MAD:hclust](#MAD-hclust)   |          2| 0.327|           0.618|       0.837|   |           |

\*\*: 1-PAC > 0.95, \*: 1-PAC > 0.9




### CDF of consensus matrices

Cumulative distribution function curves of consensus matrix for all methods.




```r
collect_plots(res_list, fun = plot_ecdf)
```

![plot of chunk collect-plots](figure_cola/collect-plots-1.png)



### Consensus heatmap

Consensus heatmaps for all methods. ([What is a consensus heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_9))


<style type='text/css'>



.ui-helper-hidden {
	display: none;
}
.ui-helper-hidden-accessible {
	border: 0;
	clip: rect(0 0 0 0);
	height: 1px;
	margin: -1px;
	overflow: hidden;
	padding: 0;
	position: absolute;
	width: 1px;
}
.ui-helper-reset {
	margin: 0;
	padding: 0;
	border: 0;
	outline: 0;
	line-height: 1.3;
	text-decoration: none;
	font-size: 100%;
	list-style: none;
}
.ui-helper-clearfix:before,
.ui-helper-clearfix:after {
	content: "";
	display: table;
	border-collapse: collapse;
}
.ui-helper-clearfix:after {
	clear: both;
}
.ui-helper-zfix {
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	position: absolute;
	opacity: 0;
	filter:Alpha(Opacity=0); 
}

.ui-front {
	z-index: 100;
}



.ui-state-disabled {
	cursor: default !important;
	pointer-events: none;
}



.ui-icon {
	display: inline-block;
	vertical-align: middle;
	margin-top: -.25em;
	position: relative;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
}

.ui-widget-icon-block {
	left: 50%;
	margin-left: -8px;
	display: block;
}




.ui-widget-overlay {
	position: fixed;
	top: 0;
	left: 0;
	width: 100%;
	height: 100%;
}
.ui-accordion .ui-accordion-header {
	display: block;
	cursor: pointer;
	position: relative;
	margin: 2px 0 0 0;
	padding: .5em .5em .5em .7em;
	font-size: 100%;
}
.ui-accordion .ui-accordion-content {
	padding: 1em 2.2em;
	border-top: 0;
	overflow: auto;
}
.ui-autocomplete {
	position: absolute;
	top: 0;
	left: 0;
	cursor: default;
}
.ui-menu {
	list-style: none;
	padding: 0;
	margin: 0;
	display: block;
	outline: 0;
}
.ui-menu .ui-menu {
	position: absolute;
}
.ui-menu .ui-menu-item {
	margin: 0;
	cursor: pointer;
	
	list-style-image: url("data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7");
}
.ui-menu .ui-menu-item-wrapper {
	position: relative;
	padding: 3px 1em 3px .4em;
}
.ui-menu .ui-menu-divider {
	margin: 5px 0;
	height: 0;
	font-size: 0;
	line-height: 0;
	border-width: 1px 0 0 0;
}
.ui-menu .ui-state-focus,
.ui-menu .ui-state-active {
	margin: -1px;
}


.ui-menu-icons {
	position: relative;
}
.ui-menu-icons .ui-menu-item-wrapper {
	padding-left: 2em;
}


.ui-menu .ui-icon {
	position: absolute;
	top: 0;
	bottom: 0;
	left: .2em;
	margin: auto 0;
}


.ui-menu .ui-menu-icon {
	left: auto;
	right: 0;
}
.ui-button {
	padding: .4em 1em;
	display: inline-block;
	position: relative;
	line-height: normal;
	margin-right: .1em;
	cursor: pointer;
	vertical-align: middle;
	text-align: center;
	-webkit-user-select: none;
	-moz-user-select: none;
	-ms-user-select: none;
	user-select: none;

	
	overflow: visible;
}

.ui-button,
.ui-button:link,
.ui-button:visited,
.ui-button:hover,
.ui-button:active {
	text-decoration: none;
}


.ui-button-icon-only {
	width: 2em;
	box-sizing: border-box;
	text-indent: -9999px;
	white-space: nowrap;
}


input.ui-button.ui-button-icon-only {
	text-indent: 0;
}


.ui-button-icon-only .ui-icon {
	position: absolute;
	top: 50%;
	left: 50%;
	margin-top: -8px;
	margin-left: -8px;
}

.ui-button.ui-icon-notext .ui-icon {
	padding: 0;
	width: 2.1em;
	height: 2.1em;
	text-indent: -9999px;
	white-space: nowrap;

}

input.ui-button.ui-icon-notext .ui-icon {
	width: auto;
	height: auto;
	text-indent: 0;
	white-space: normal;
	padding: .4em 1em;
}



input.ui-button::-moz-focus-inner,
button.ui-button::-moz-focus-inner {
	border: 0;
	padding: 0;
}
.ui-controlgroup {
	vertical-align: middle;
	display: inline-block;
}
.ui-controlgroup > .ui-controlgroup-item {
	float: left;
	margin-left: 0;
	margin-right: 0;
}
.ui-controlgroup > .ui-controlgroup-item:focus,
.ui-controlgroup > .ui-controlgroup-item.ui-visual-focus {
	z-index: 9999;
}
.ui-controlgroup-vertical > .ui-controlgroup-item {
	display: block;
	float: none;
	width: 100%;
	margin-top: 0;
	margin-bottom: 0;
	text-align: left;
}
.ui-controlgroup-vertical .ui-controlgroup-item {
	box-sizing: border-box;
}
.ui-controlgroup .ui-controlgroup-label {
	padding: .4em 1em;
}
.ui-controlgroup .ui-controlgroup-label span {
	font-size: 80%;
}
.ui-controlgroup-horizontal .ui-controlgroup-label + .ui-controlgroup-item {
	border-left: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label + .ui-controlgroup-item {
	border-top: none;
}
.ui-controlgroup-horizontal .ui-controlgroup-label.ui-widget-content {
	border-right: none;
}
.ui-controlgroup-vertical .ui-controlgroup-label.ui-widget-content {
	border-bottom: none;
}


.ui-controlgroup-vertical .ui-spinner-input {

	
	width: 75%;
	width: calc( 100% - 2.4em );
}
.ui-controlgroup-vertical .ui-spinner .ui-spinner-up {
	border-top-style: solid;
}

.ui-checkboxradio-label .ui-icon-background {
	box-shadow: inset 1px 1px 1px #ccc;
	border-radius: .12em;
	border: none;
}
.ui-checkboxradio-radio-label .ui-icon-background {
	width: 16px;
	height: 16px;
	border-radius: 1em;
	overflow: visible;
	border: none;
}
.ui-checkboxradio-radio-label.ui-checkboxradio-checked .ui-icon,
.ui-checkboxradio-radio-label.ui-checkboxradio-checked:hover .ui-icon {
	background-image: none;
	width: 8px;
	height: 8px;
	border-width: 4px;
	border-style: solid;
}
.ui-checkboxradio-disabled {
	pointer-events: none;
}
.ui-datepicker {
	width: 17em;
	padding: .2em .2em 0;
	display: none;
}
.ui-datepicker .ui-datepicker-header {
	position: relative;
	padding: .2em 0;
}
.ui-datepicker .ui-datepicker-prev,
.ui-datepicker .ui-datepicker-next {
	position: absolute;
	top: 2px;
	width: 1.8em;
	height: 1.8em;
}
.ui-datepicker .ui-datepicker-prev-hover,
.ui-datepicker .ui-datepicker-next-hover {
	top: 1px;
}
.ui-datepicker .ui-datepicker-prev {
	left: 2px;
}
.ui-datepicker .ui-datepicker-next {
	right: 2px;
}
.ui-datepicker .ui-datepicker-prev-hover {
	left: 1px;
}
.ui-datepicker .ui-datepicker-next-hover {
	right: 1px;
}
.ui-datepicker .ui-datepicker-prev span,
.ui-datepicker .ui-datepicker-next span {
	display: block;
	position: absolute;
	left: 50%;
	margin-left: -8px;
	top: 50%;
	margin-top: -8px;
}
.ui-datepicker .ui-datepicker-title {
	margin: 0 2.3em;
	line-height: 1.8em;
	text-align: center;
}
.ui-datepicker .ui-datepicker-title select {
	font-size: 1em;
	margin: 1px 0;
}
.ui-datepicker select.ui-datepicker-month,
.ui-datepicker select.ui-datepicker-year {
	width: 45%;
}
.ui-datepicker table {
	width: 100%;
	font-size: .9em;
	border-collapse: collapse;
	margin: 0 0 .4em;
}
.ui-datepicker th {
	padding: .7em .3em;
	text-align: center;
	font-weight: bold;
	border: 0;
}
.ui-datepicker td {
	border: 0;
	padding: 1px;
}
.ui-datepicker td span,
.ui-datepicker td a {
	display: block;
	padding: .2em;
	text-align: right;
	text-decoration: none;
}
.ui-datepicker .ui-datepicker-buttonpane {
	background-image: none;
	margin: .7em 0 0 0;
	padding: 0 .2em;
	border-left: 0;
	border-right: 0;
	border-bottom: 0;
}
.ui-datepicker .ui-datepicker-buttonpane button {
	float: right;
	margin: .5em .2em .4em;
	cursor: pointer;
	padding: .2em .6em .3em .6em;
	width: auto;
	overflow: visible;
}
.ui-datepicker .ui-datepicker-buttonpane button.ui-datepicker-current {
	float: left;
}


.ui-datepicker.ui-datepicker-multi {
	width: auto;
}
.ui-datepicker-multi .ui-datepicker-group {
	float: left;
}
.ui-datepicker-multi .ui-datepicker-group table {
	width: 95%;
	margin: 0 auto .4em;
}
.ui-datepicker-multi-2 .ui-datepicker-group {
	width: 50%;
}
.ui-datepicker-multi-3 .ui-datepicker-group {
	width: 33.3%;
}
.ui-datepicker-multi-4 .ui-datepicker-group {
	width: 25%;
}
.ui-datepicker-multi .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-multi .ui-datepicker-group-middle .ui-datepicker-header {
	border-left-width: 0;
}
.ui-datepicker-multi .ui-datepicker-buttonpane {
	clear: left;
}
.ui-datepicker-row-break {
	clear: both;
	width: 100%;
	font-size: 0;
}


.ui-datepicker-rtl {
	direction: rtl;
}
.ui-datepicker-rtl .ui-datepicker-prev {
	right: 2px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next {
	left: 2px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-prev:hover {
	right: 1px;
	left: auto;
}
.ui-datepicker-rtl .ui-datepicker-next:hover {
	left: 1px;
	right: auto;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane {
	clear: right;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button {
	float: left;
}
.ui-datepicker-rtl .ui-datepicker-buttonpane button.ui-datepicker-current,
.ui-datepicker-rtl .ui-datepicker-group {
	float: right;
}
.ui-datepicker-rtl .ui-datepicker-group-last .ui-datepicker-header,
.ui-datepicker-rtl .ui-datepicker-group-middle .ui-datepicker-header {
	border-right-width: 0;
	border-left-width: 1px;
}


.ui-datepicker .ui-icon {
	display: block;
	text-indent: -99999px;
	overflow: hidden;
	background-repeat: no-repeat;
	left: .5em;
	top: .3em;
}
.ui-dialog {
	position: absolute;
	top: 0;
	left: 0;
	padding: .2em;
	outline: 0;
}
.ui-dialog .ui-dialog-titlebar {
	padding: .4em 1em;
	position: relative;
}
.ui-dialog .ui-dialog-title {
	float: left;
	margin: .1em 0;
	white-space: nowrap;
	width: 90%;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-dialog .ui-dialog-titlebar-close {
	position: absolute;
	right: .3em;
	top: 50%;
	width: 20px;
	margin: -10px 0 0 0;
	padding: 1px;
	height: 20px;
}
.ui-dialog .ui-dialog-content {
	position: relative;
	border: 0;
	padding: .5em 1em;
	background: none;
	overflow: auto;
}
.ui-dialog .ui-dialog-buttonpane {
	text-align: left;
	border-width: 1px 0 0 0;
	background-image: none;
	margin-top: .5em;
	padding: .3em 1em .5em .4em;
}
.ui-dialog .ui-dialog-buttonpane .ui-dialog-buttonset {
	float: right;
}
.ui-dialog .ui-dialog-buttonpane button {
	margin: .5em .4em .5em 0;
	cursor: pointer;
}
.ui-dialog .ui-resizable-n {
	height: 2px;
	top: 0;
}
.ui-dialog .ui-resizable-e {
	width: 2px;
	right: 0;
}
.ui-dialog .ui-resizable-s {
	height: 2px;
	bottom: 0;
}
.ui-dialog .ui-resizable-w {
	width: 2px;
	left: 0;
}
.ui-dialog .ui-resizable-se,
.ui-dialog .ui-resizable-sw,
.ui-dialog .ui-resizable-ne,
.ui-dialog .ui-resizable-nw {
	width: 7px;
	height: 7px;
}
.ui-dialog .ui-resizable-se {
	right: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-sw {
	left: 0;
	bottom: 0;
}
.ui-dialog .ui-resizable-ne {
	right: 0;
	top: 0;
}
.ui-dialog .ui-resizable-nw {
	left: 0;
	top: 0;
}
.ui-draggable .ui-dialog-titlebar {
	cursor: move;
}
.ui-draggable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable {
	position: relative;
}
.ui-resizable-handle {
	position: absolute;
	font-size: 0.1px;
	display: block;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-resizable-disabled .ui-resizable-handle,
.ui-resizable-autohide .ui-resizable-handle {
	display: none;
}
.ui-resizable-n {
	cursor: n-resize;
	height: 7px;
	width: 100%;
	top: -5px;
	left: 0;
}
.ui-resizable-s {
	cursor: s-resize;
	height: 7px;
	width: 100%;
	bottom: -5px;
	left: 0;
}
.ui-resizable-e {
	cursor: e-resize;
	width: 7px;
	right: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-w {
	cursor: w-resize;
	width: 7px;
	left: -5px;
	top: 0;
	height: 100%;
}
.ui-resizable-se {
	cursor: se-resize;
	width: 12px;
	height: 12px;
	right: 1px;
	bottom: 1px;
}
.ui-resizable-sw {
	cursor: sw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	bottom: -5px;
}
.ui-resizable-nw {
	cursor: nw-resize;
	width: 9px;
	height: 9px;
	left: -5px;
	top: -5px;
}
.ui-resizable-ne {
	cursor: ne-resize;
	width: 9px;
	height: 9px;
	right: -5px;
	top: -5px;
}
.ui-progressbar {
	height: 2em;
	text-align: left;
	overflow: hidden;
}
.ui-progressbar .ui-progressbar-value {
	margin: -1px;
	height: 100%;
}
.ui-progressbar .ui-progressbar-overlay {
	background: url("data:image/gif;base64,R0lGODlhKAAoAIABAAAAAP///yH/C05FVFNDQVBFMi4wAwEAAAAh+QQJAQABACwAAAAAKAAoAAACkYwNqXrdC52DS06a7MFZI+4FHBCKoDeWKXqymPqGqxvJrXZbMx7Ttc+w9XgU2FB3lOyQRWET2IFGiU9m1frDVpxZZc6bfHwv4c1YXP6k1Vdy292Fb6UkuvFtXpvWSzA+HycXJHUXiGYIiMg2R6W459gnWGfHNdjIqDWVqemH2ekpObkpOlppWUqZiqr6edqqWQAAIfkECQEAAQAsAAAAACgAKAAAApSMgZnGfaqcg1E2uuzDmmHUBR8Qil95hiPKqWn3aqtLsS18y7G1SzNeowWBENtQd+T1JktP05nzPTdJZlR6vUxNWWjV+vUWhWNkWFwxl9VpZRedYcflIOLafaa28XdsH/ynlcc1uPVDZxQIR0K25+cICCmoqCe5mGhZOfeYSUh5yJcJyrkZWWpaR8doJ2o4NYq62lAAACH5BAkBAAEALAAAAAAoACgAAAKVDI4Yy22ZnINRNqosw0Bv7i1gyHUkFj7oSaWlu3ovC8GxNso5fluz3qLVhBVeT/Lz7ZTHyxL5dDalQWPVOsQWtRnuwXaFTj9jVVh8pma9JjZ4zYSj5ZOyma7uuolffh+IR5aW97cHuBUXKGKXlKjn+DiHWMcYJah4N0lYCMlJOXipGRr5qdgoSTrqWSq6WFl2ypoaUAAAIfkECQEAAQAsAAAAACgAKAAAApaEb6HLgd/iO7FNWtcFWe+ufODGjRfoiJ2akShbueb0wtI50zm02pbvwfWEMWBQ1zKGlLIhskiEPm9R6vRXxV4ZzWT2yHOGpWMyorblKlNp8HmHEb/lCXjcW7bmtXP8Xt229OVWR1fod2eWqNfHuMjXCPkIGNileOiImVmCOEmoSfn3yXlJWmoHGhqp6ilYuWYpmTqKUgAAIfkECQEAAQAsAAAAACgAKAAAApiEH6kb58biQ3FNWtMFWW3eNVcojuFGfqnZqSebuS06w5V80/X02pKe8zFwP6EFWOT1lDFk8rGERh1TTNOocQ61Hm4Xm2VexUHpzjymViHrFbiELsefVrn6XKfnt2Q9G/+Xdie499XHd2g4h7ioOGhXGJboGAnXSBnoBwKYyfioubZJ2Hn0RuRZaflZOil56Zp6iioKSXpUAAAh+QQJAQABACwAAAAAKAAoAAACkoQRqRvnxuI7kU1a1UU5bd5tnSeOZXhmn5lWK3qNTWvRdQxP8qvaC+/yaYQzXO7BMvaUEmJRd3TsiMAgswmNYrSgZdYrTX6tSHGZO73ezuAw2uxuQ+BbeZfMxsexY35+/Qe4J1inV0g4x3WHuMhIl2jXOKT2Q+VU5fgoSUI52VfZyfkJGkha6jmY+aaYdirq+lQAACH5BAkBAAEALAAAAAAoACgAAAKWBIKpYe0L3YNKToqswUlvznigd4wiR4KhZrKt9Upqip61i9E3vMvxRdHlbEFiEXfk9YARYxOZZD6VQ2pUunBmtRXo1Lf8hMVVcNl8JafV38aM2/Fu5V16Bn63r6xt97j09+MXSFi4BniGFae3hzbH9+hYBzkpuUh5aZmHuanZOZgIuvbGiNeomCnaxxap2upaCZsq+1kAACH5BAkBAAEALAAAAAAoACgAAAKXjI8By5zf4kOxTVrXNVlv1X0d8IGZGKLnNpYtm8Lr9cqVeuOSvfOW79D9aDHizNhDJidFZhNydEahOaDH6nomtJjp1tutKoNWkvA6JqfRVLHU/QUfau9l2x7G54d1fl995xcIGAdXqMfBNadoYrhH+Mg2KBlpVpbluCiXmMnZ2Sh4GBqJ+ckIOqqJ6LmKSllZmsoq6wpQAAAh+QQJAQABACwAAAAAKAAoAAAClYx/oLvoxuJDkU1a1YUZbJ59nSd2ZXhWqbRa2/gF8Gu2DY3iqs7yrq+xBYEkYvFSM8aSSObE+ZgRl1BHFZNr7pRCavZ5BW2142hY3AN/zWtsmf12p9XxxFl2lpLn1rseztfXZjdIWIf2s5dItwjYKBgo9yg5pHgzJXTEeGlZuenpyPmpGQoKOWkYmSpaSnqKileI2FAAACH5BAkBAAEALAAAAAAoACgAAAKVjB+gu+jG4kORTVrVhRlsnn2dJ3ZleFaptFrb+CXmO9OozeL5VfP99HvAWhpiUdcwkpBH3825AwYdU8xTqlLGhtCosArKMpvfa1mMRae9VvWZfeB2XfPkeLmm18lUcBj+p5dnN8jXZ3YIGEhYuOUn45aoCDkp16hl5IjYJvjWKcnoGQpqyPlpOhr3aElaqrq56Bq7VAAAOw==");
	height: 100%;
	filter: alpha(opacity=25); 
	opacity: 0.25;
}
.ui-progressbar-indeterminate .ui-progressbar-value {
	background-image: none;
}
.ui-selectable {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-selectable-helper {
	position: absolute;
	z-index: 100;
	border: 1px dotted black;
}
.ui-selectmenu-menu {
	padding: 0;
	margin: 0;
	position: absolute;
	top: 0;
	left: 0;
	display: none;
}
.ui-selectmenu-menu .ui-menu {
	overflow: auto;
	overflow-x: hidden;
	padding-bottom: 1px;
}
.ui-selectmenu-menu .ui-menu .ui-selectmenu-optgroup {
	font-size: 1em;
	font-weight: bold;
	line-height: 1.5;
	padding: 2px 0.4em;
	margin: 0.5em 0 0 0;
	height: auto;
	border: 0;
}
.ui-selectmenu-open {
	display: block;
}
.ui-selectmenu-text {
	display: block;
	margin-right: 20px;
	overflow: hidden;
	text-overflow: ellipsis;
}
.ui-selectmenu-button.ui-button {
	text-align: left;
	white-space: nowrap;
	width: 14em;
}
.ui-selectmenu-icon.ui-icon {
	float: right;
	margin-top: 0;
}
.ui-slider {
	position: relative;
	text-align: left;
}
.ui-slider .ui-slider-handle {
	position: absolute;
	z-index: 2;
	width: 1.2em;
	height: 1.2em;
	cursor: default;
	-ms-touch-action: none;
	touch-action: none;
}
.ui-slider .ui-slider-range {
	position: absolute;
	z-index: 1;
	font-size: .7em;
	display: block;
	border: 0;
	background-position: 0 0;
}


.ui-slider.ui-state-disabled .ui-slider-handle,
.ui-slider.ui-state-disabled .ui-slider-range {
	filter: inherit;
}

.ui-slider-horizontal {
	height: .8em;
}
.ui-slider-horizontal .ui-slider-handle {
	top: -.3em;
	margin-left: -.6em;
}
.ui-slider-horizontal .ui-slider-range {
	top: 0;
	height: 100%;
}
.ui-slider-horizontal .ui-slider-range-min {
	left: 0;
}
.ui-slider-horizontal .ui-slider-range-max {
	right: 0;
}

.ui-slider-vertical {
	width: .8em;
	height: 100px;
}
.ui-slider-vertical .ui-slider-handle {
	left: -.3em;
	margin-left: 0;
	margin-bottom: -.6em;
}
.ui-slider-vertical .ui-slider-range {
	left: 0;
	width: 100%;
}
.ui-slider-vertical .ui-slider-range-min {
	bottom: 0;
}
.ui-slider-vertical .ui-slider-range-max {
	top: 0;
}
.ui-sortable-handle {
	-ms-touch-action: none;
	touch-action: none;
}
.ui-spinner {
	position: relative;
	display: inline-block;
	overflow: hidden;
	padding: 0;
	vertical-align: middle;
}
.ui-spinner-input {
	border: none;
	background: none;
	color: inherit;
	padding: .222em 0;
	margin: .2em 0;
	vertical-align: middle;
	margin-left: .4em;
	margin-right: 2em;
}
.ui-spinner-button {
	width: 1.6em;
	height: 50%;
	font-size: .5em;
	padding: 0;
	margin: 0;
	text-align: center;
	position: absolute;
	cursor: default;
	display: block;
	overflow: hidden;
	right: 0;
}

.ui-spinner a.ui-spinner-button {
	border-top-style: none;
	border-bottom-style: none;
	border-right-style: none;
}
.ui-spinner-up {
	top: 0;
}
.ui-spinner-down {
	bottom: 0;
}
.ui-tabs {
	position: relative;
	padding: .2em;
}
.ui-tabs .ui-tabs-nav {
	margin: 0;
	padding: .2em .2em 0;
}
.ui-tabs .ui-tabs-nav li {
	list-style: none;
	float: left;
	position: relative;
	top: 0;
	margin: 1px .2em 0 0;
	border-bottom-width: 0;
	padding: 0;
	white-space: nowrap;
}
.ui-tabs .ui-tabs-nav .ui-tabs-anchor {
	float: left;
	padding: .5em 1em;
	text-decoration: none;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active {
	margin-bottom: -1px;
	padding-bottom: 1px;
}
.ui-tabs .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-state-disabled .ui-tabs-anchor,
.ui-tabs .ui-tabs-nav li.ui-tabs-loading .ui-tabs-anchor {
	cursor: text;
}
.ui-tabs-collapsible .ui-tabs-nav li.ui-tabs-active .ui-tabs-anchor {
	cursor: pointer;
}
.ui-tabs .ui-tabs-panel {
	display: block;
	border-width: 0;
	padding: 1em 1.4em;
	background: none;
}
.ui-tooltip {
	padding: 8px;
	position: absolute;
	z-index: 9999;
	max-width: 300px;
}
body .ui-tooltip {
	border-width: 2px;
}

.ui-widget {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget .ui-widget {
	font-size: 1em;
}
.ui-widget input,
.ui-widget select,
.ui-widget textarea,
.ui-widget button {
	font-family: Arial,Helvetica,sans-serif;
	font-size: 1em;
}
.ui-widget.ui-widget-content {
	border: 1px solid #c5c5c5;
}
.ui-widget-content {
	border: 1px solid #dddddd;
	background: #ffffff;
	color: #333333;
}
.ui-widget-content a {
	color: #333333;
}
.ui-widget-header {
	border: 1px solid #dddddd;
	background: #e9e9e9;
	color: #333333;
	font-weight: bold;
}
.ui-widget-header a {
	color: #333333;
}


.ui-state-default,
.ui-widget-content .ui-state-default,
.ui-widget-header .ui-state-default,
.ui-button,


html .ui-button.ui-state-disabled:hover,
html .ui-button.ui-state-disabled:active {
	border: 1px solid #c5c5c5;
	background: #f6f6f6;
	font-weight: normal;
	color: #454545;
}
.ui-state-default a,
.ui-state-default a:link,
.ui-state-default a:visited,
a.ui-button,
a:link.ui-button,
a:visited.ui-button,
.ui-button {
	color: #454545;
	text-decoration: none;
}
.ui-state-hover,
.ui-widget-content .ui-state-hover,
.ui-widget-header .ui-state-hover,
.ui-state-focus,
.ui-widget-content .ui-state-focus,
.ui-widget-header .ui-state-focus,
.ui-button:hover,
.ui-button:focus {
	border: 1px solid #cccccc;
	background: #ededed;
	font-weight: normal;
	color: #2b2b2b;
}
.ui-state-hover a,
.ui-state-hover a:hover,
.ui-state-hover a:link,
.ui-state-hover a:visited,
.ui-state-focus a,
.ui-state-focus a:hover,
.ui-state-focus a:link,
.ui-state-focus a:visited,
a.ui-button:hover,
a.ui-button:focus {
	color: #2b2b2b;
	text-decoration: none;
}

.ui-visual-focus {
	box-shadow: 0 0 3px 1px rgb(94, 158, 214);
}
.ui-state-active,
.ui-widget-content .ui-state-active,
.ui-widget-header .ui-state-active,
a.ui-button:active,
.ui-button:active,
.ui-button.ui-state-active:hover {
	border: 1px solid #003eff;
	background: #007fff;
	font-weight: normal;
	color: #ffffff;
}
.ui-icon-background,
.ui-state-active .ui-icon-background {
	border: #003eff;
	background-color: #ffffff;
}
.ui-state-active a,
.ui-state-active a:link,
.ui-state-active a:visited {
	color: #ffffff;
	text-decoration: none;
}


.ui-state-highlight,
.ui-widget-content .ui-state-highlight,
.ui-widget-header .ui-state-highlight {
	border: 1px solid #dad55e;
	background: #fffa90;
	color: #777620;
}
.ui-state-checked {
	border: 1px solid #dad55e;
	background: #fffa90;
}
.ui-state-highlight a,
.ui-widget-content .ui-state-highlight a,
.ui-widget-header .ui-state-highlight a {
	color: #777620;
}
.ui-state-error,
.ui-widget-content .ui-state-error,
.ui-widget-header .ui-state-error {
	border: 1px solid #f1a899;
	background: #fddfdf;
	color: #5f3f3f;
}
.ui-state-error a,
.ui-widget-content .ui-state-error a,
.ui-widget-header .ui-state-error a {
	color: #5f3f3f;
}
.ui-state-error-text,
.ui-widget-content .ui-state-error-text,
.ui-widget-header .ui-state-error-text {
	color: #5f3f3f;
}
.ui-priority-primary,
.ui-widget-content .ui-priority-primary,
.ui-widget-header .ui-priority-primary {
	font-weight: bold;
}
.ui-priority-secondary,
.ui-widget-content .ui-priority-secondary,
.ui-widget-header .ui-priority-secondary {
	opacity: .7;
	filter:Alpha(Opacity=70); 
	font-weight: normal;
}
.ui-state-disabled,
.ui-widget-content .ui-state-disabled,
.ui-widget-header .ui-state-disabled {
	opacity: .35;
	filter:Alpha(Opacity=35); 
	background-image: none;
}
.ui-state-disabled .ui-icon {
	filter:Alpha(Opacity=35); 
}




.ui-icon {
	width: 16px;
	height: 16px;
}
.ui-icon,
.ui-widget-content .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-widget-header .ui-icon {
	background-image: url("images/ui-icons_444444_256x240.png");
}
.ui-state-hover .ui-icon,
.ui-state-focus .ui-icon,
.ui-button:hover .ui-icon,
.ui-button:focus .ui-icon {
	background-image: url("images/ui-icons_555555_256x240.png");
}
.ui-state-active .ui-icon,
.ui-button:active .ui-icon {
	background-image: url("images/ui-icons_ffffff_256x240.png");
}
.ui-state-highlight .ui-icon,
.ui-button .ui-state-highlight.ui-icon {
	background-image: url("images/ui-icons_777620_256x240.png");
}
.ui-state-error .ui-icon,
.ui-state-error-text .ui-icon {
	background-image: url("images/ui-icons_cc0000_256x240.png");
}
.ui-button .ui-icon {
	background-image: url("images/ui-icons_777777_256x240.png");
}


.ui-icon-blank { background-position: 16px 16px; }
.ui-icon-caret-1-n { background-position: 0 0; }
.ui-icon-caret-1-ne { background-position: -16px 0; }
.ui-icon-caret-1-e { background-position: -32px 0; }
.ui-icon-caret-1-se { background-position: -48px 0; }
.ui-icon-caret-1-s { background-position: -65px 0; }
.ui-icon-caret-1-sw { background-position: -80px 0; }
.ui-icon-caret-1-w { background-position: -96px 0; }
.ui-icon-caret-1-nw { background-position: -112px 0; }
.ui-icon-caret-2-n-s { background-position: -128px 0; }
.ui-icon-caret-2-e-w { background-position: -144px 0; }
.ui-icon-triangle-1-n { background-position: 0 -16px; }
.ui-icon-triangle-1-ne { background-position: -16px -16px; }
.ui-icon-triangle-1-e { background-position: -32px -16px; }
.ui-icon-triangle-1-se { background-position: -48px -16px; }
.ui-icon-triangle-1-s { background-position: -65px -16px; }
.ui-icon-triangle-1-sw { background-position: -80px -16px; }
.ui-icon-triangle-1-w { background-position: -96px -16px; }
.ui-icon-triangle-1-nw { background-position: -112px -16px; }
.ui-icon-triangle-2-n-s { background-position: -128px -16px; }
.ui-icon-triangle-2-e-w { background-position: -144px -16px; }
.ui-icon-arrow-1-n { background-position: 0 -32px; }
.ui-icon-arrow-1-ne { background-position: -16px -32px; }
.ui-icon-arrow-1-e { background-position: -32px -32px; }
.ui-icon-arrow-1-se { background-position: -48px -32px; }
.ui-icon-arrow-1-s { background-position: -65px -32px; }
.ui-icon-arrow-1-sw { background-position: -80px -32px; }
.ui-icon-arrow-1-w { background-position: -96px -32px; }
.ui-icon-arrow-1-nw { background-position: -112px -32px; }
.ui-icon-arrow-2-n-s { background-position: -128px -32px; }
.ui-icon-arrow-2-ne-sw { background-position: -144px -32px; }
.ui-icon-arrow-2-e-w { background-position: -160px -32px; }
.ui-icon-arrow-2-se-nw { background-position: -176px -32px; }
.ui-icon-arrowstop-1-n { background-position: -192px -32px; }
.ui-icon-arrowstop-1-e { background-position: -208px -32px; }
.ui-icon-arrowstop-1-s { background-position: -224px -32px; }
.ui-icon-arrowstop-1-w { background-position: -240px -32px; }
.ui-icon-arrowthick-1-n { background-position: 1px -48px; }
.ui-icon-arrowthick-1-ne { background-position: -16px -48px; }
.ui-icon-arrowthick-1-e { background-position: -32px -48px; }
.ui-icon-arrowthick-1-se { background-position: -48px -48px; }
.ui-icon-arrowthick-1-s { background-position: -64px -48px; }
.ui-icon-arrowthick-1-sw { background-position: -80px -48px; }
.ui-icon-arrowthick-1-w { background-position: -96px -48px; }
.ui-icon-arrowthick-1-nw { background-position: -112px -48px; }
.ui-icon-arrowthick-2-n-s { background-position: -128px -48px; }
.ui-icon-arrowthick-2-ne-sw { background-position: -144px -48px; }
.ui-icon-arrowthick-2-e-w { background-position: -160px -48px; }
.ui-icon-arrowthick-2-se-nw { background-position: -176px -48px; }
.ui-icon-arrowthickstop-1-n { background-position: -192px -48px; }
.ui-icon-arrowthickstop-1-e { background-position: -208px -48px; }
.ui-icon-arrowthickstop-1-s { background-position: -224px -48px; }
.ui-icon-arrowthickstop-1-w { background-position: -240px -48px; }
.ui-icon-arrowreturnthick-1-w { background-position: 0 -64px; }
.ui-icon-arrowreturnthick-1-n { background-position: -16px -64px; }
.ui-icon-arrowreturnthick-1-e { background-position: -32px -64px; }
.ui-icon-arrowreturnthick-1-s { background-position: -48px -64px; }
.ui-icon-arrowreturn-1-w { background-position: -64px -64px; }
.ui-icon-arrowreturn-1-n { background-position: -80px -64px; }
.ui-icon-arrowreturn-1-e { background-position: -96px -64px; }
.ui-icon-arrowreturn-1-s { background-position: -112px -64px; }
.ui-icon-arrowrefresh-1-w { background-position: -128px -64px; }
.ui-icon-arrowrefresh-1-n { background-position: -144px -64px; }
.ui-icon-arrowrefresh-1-e { background-position: -160px -64px; }
.ui-icon-arrowrefresh-1-s { background-position: -176px -64px; }
.ui-icon-arrow-4 { background-position: 0 -80px; }
.ui-icon-arrow-4-diag { background-position: -16px -80px; }
.ui-icon-extlink { background-position: -32px -80px; }
.ui-icon-newwin { background-position: -48px -80px; }
.ui-icon-refresh { background-position: -64px -80px; }
.ui-icon-shuffle { background-position: -80px -80px; }
.ui-icon-transfer-e-w { background-position: -96px -80px; }
.ui-icon-transferthick-e-w { background-position: -112px -80px; }
.ui-icon-folder-collapsed { background-position: 0 -96px; }
.ui-icon-folder-open { background-position: -16px -96px; }
.ui-icon-document { background-position: -32px -96px; }
.ui-icon-document-b { background-position: -48px -96px; }
.ui-icon-note { background-position: -64px -96px; }
.ui-icon-mail-closed { background-position: -80px -96px; }
.ui-icon-mail-open { background-position: -96px -96px; }
.ui-icon-suitcase { background-position: -112px -96px; }
.ui-icon-comment { background-position: -128px -96px; }
.ui-icon-person { background-position: -144px -96px; }
.ui-icon-print { background-position: -160px -96px; }
.ui-icon-trash { background-position: -176px -96px; }
.ui-icon-locked { background-position: -192px -96px; }
.ui-icon-unlocked { background-position: -208px -96px; }
.ui-icon-bookmark { background-position: -224px -96px; }
.ui-icon-tag { background-position: -240px -96px; }
.ui-icon-home { background-position: 0 -112px; }
.ui-icon-flag { background-position: -16px -112px; }
.ui-icon-calendar { background-position: -32px -112px; }
.ui-icon-cart { background-position: -48px -112px; }
.ui-icon-pencil { background-position: -64px -112px; }
.ui-icon-clock { background-position: -80px -112px; }
.ui-icon-disk { background-position: -96px -112px; }
.ui-icon-calculator { background-position: -112px -112px; }
.ui-icon-zoomin { background-position: -128px -112px; }
.ui-icon-zoomout { background-position: -144px -112px; }
.ui-icon-search { background-position: -160px -112px; }
.ui-icon-wrench { background-position: -176px -112px; }
.ui-icon-gear { background-position: -192px -112px; }
.ui-icon-heart { background-position: -208px -112px; }
.ui-icon-star { background-position: -224px -112px; }
.ui-icon-link { background-position: -240px -112px; }
.ui-icon-cancel { background-position: 0 -128px; }
.ui-icon-plus { background-position: -16px -128px; }
.ui-icon-plusthick { background-position: -32px -128px; }
.ui-icon-minus { background-position: -48px -128px; }
.ui-icon-minusthick { background-position: -64px -128px; }
.ui-icon-close { background-position: -80px -128px; }
.ui-icon-closethick { background-position: -96px -128px; }
.ui-icon-key { background-position: -112px -128px; }
.ui-icon-lightbulb { background-position: -128px -128px; }
.ui-icon-scissors { background-position: -144px -128px; }
.ui-icon-clipboard { background-position: -160px -128px; }
.ui-icon-copy { background-position: -176px -128px; }
.ui-icon-contact { background-position: -192px -128px; }
.ui-icon-image { background-position: -208px -128px; }
.ui-icon-video { background-position: -224px -128px; }
.ui-icon-script { background-position: -240px -128px; }
.ui-icon-alert { background-position: 0 -144px; }
.ui-icon-info { background-position: -16px -144px; }
.ui-icon-notice { background-position: -32px -144px; }
.ui-icon-help { background-position: -48px -144px; }
.ui-icon-check { background-position: -64px -144px; }
.ui-icon-bullet { background-position: -80px -144px; }
.ui-icon-radio-on { background-position: -96px -144px; }
.ui-icon-radio-off { background-position: -112px -144px; }
.ui-icon-pin-w { background-position: -128px -144px; }
.ui-icon-pin-s { background-position: -144px -144px; }
.ui-icon-play { background-position: 0 -160px; }
.ui-icon-pause { background-position: -16px -160px; }
.ui-icon-seek-next { background-position: -32px -160px; }
.ui-icon-seek-prev { background-position: -48px -160px; }
.ui-icon-seek-end { background-position: -64px -160px; }
.ui-icon-seek-start { background-position: -80px -160px; }

.ui-icon-seek-first { background-position: -80px -160px; }
.ui-icon-stop { background-position: -96px -160px; }
.ui-icon-eject { background-position: -112px -160px; }
.ui-icon-volume-off { background-position: -128px -160px; }
.ui-icon-volume-on { background-position: -144px -160px; }
.ui-icon-power { background-position: 0 -176px; }
.ui-icon-signal-diag { background-position: -16px -176px; }
.ui-icon-signal { background-position: -32px -176px; }
.ui-icon-battery-0 { background-position: -48px -176px; }
.ui-icon-battery-1 { background-position: -64px -176px; }
.ui-icon-battery-2 { background-position: -80px -176px; }
.ui-icon-battery-3 { background-position: -96px -176px; }
.ui-icon-circle-plus { background-position: 0 -192px; }
.ui-icon-circle-minus { background-position: -16px -192px; }
.ui-icon-circle-close { background-position: -32px -192px; }
.ui-icon-circle-triangle-e { background-position: -48px -192px; }
.ui-icon-circle-triangle-s { background-position: -64px -192px; }
.ui-icon-circle-triangle-w { background-position: -80px -192px; }
.ui-icon-circle-triangle-n { background-position: -96px -192px; }
.ui-icon-circle-arrow-e { background-position: -112px -192px; }
.ui-icon-circle-arrow-s { background-position: -128px -192px; }
.ui-icon-circle-arrow-w { background-position: -144px -192px; }
.ui-icon-circle-arrow-n { background-position: -160px -192px; }
.ui-icon-circle-zoomin { background-position: -176px -192px; }
.ui-icon-circle-zoomout { background-position: -192px -192px; }
.ui-icon-circle-check { background-position: -208px -192px; }
.ui-icon-circlesmall-plus { background-position: 0 -208px; }
.ui-icon-circlesmall-minus { background-position: -16px -208px; }
.ui-icon-circlesmall-close { background-position: -32px -208px; }
.ui-icon-squaresmall-plus { background-position: -48px -208px; }
.ui-icon-squaresmall-minus { background-position: -64px -208px; }
.ui-icon-squaresmall-close { background-position: -80px -208px; }
.ui-icon-grip-dotted-vertical { background-position: 0 -224px; }
.ui-icon-grip-dotted-horizontal { background-position: -16px -224px; }
.ui-icon-grip-solid-vertical { background-position: -32px -224px; }
.ui-icon-grip-solid-horizontal { background-position: -48px -224px; }
.ui-icon-gripsmall-diagonal-se { background-position: -64px -224px; }
.ui-icon-grip-diagonal-se { background-position: -80px -224px; }





.ui-corner-all,
.ui-corner-top,
.ui-corner-left,
.ui-corner-tl {
	border-top-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-top,
.ui-corner-right,
.ui-corner-tr {
	border-top-right-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-left,
.ui-corner-bl {
	border-bottom-left-radius: 3px;
}
.ui-corner-all,
.ui-corner-bottom,
.ui-corner-right,
.ui-corner-br {
	border-bottom-right-radius: 3px;
}


.ui-widget-overlay {
	background: #aaaaaa;
	opacity: .3;
	filter: Alpha(Opacity=30); 
}
.ui-widget-shadow {
	-webkit-box-shadow: 0px 0px 5px #666666;
	box-shadow: 0px 0px 5px #666666;
} 
</style>
<script src='js/jquery-1.12.4.js'></script>
<script src='js/jquery-ui.js'></script>

<script>
$( function() {
	$( '#tabs-collect-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-consensus-heatmap'>
<ul>
<li><a href='#tab-collect-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-consensus-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-1-1.png" alt="plot of chunk tab-collect-consensus-heatmap-1"/></p>

</div>
<div id='tab-collect-consensus-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-2-1.png" alt="plot of chunk tab-collect-consensus-heatmap-2"/></p>

</div>
<div id='tab-collect-consensus-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-3-1.png" alt="plot of chunk tab-collect-consensus-heatmap-3"/></p>

</div>
<div id='tab-collect-consensus-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-4-1.png" alt="plot of chunk tab-collect-consensus-heatmap-4"/></p>

</div>
<div id='tab-collect-consensus-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = consensus_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-consensus-heatmap-5-1.png" alt="plot of chunk tab-collect-consensus-heatmap-5"/></p>

</div>
</div>



### Membership heatmap

Membership heatmaps for all methods. ([What is a membership heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_12))


<script>
$( function() {
	$( '#tabs-collect-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-collect-membership-heatmap'>
<ul>
<li><a href='#tab-collect-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-collect-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-collect-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-collect-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-collect-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-collect-membership-heatmap-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-1-1.png" alt="plot of chunk tab-collect-membership-heatmap-1"/></p>

</div>
<div id='tab-collect-membership-heatmap-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-2-1.png" alt="plot of chunk tab-collect-membership-heatmap-2"/></p>

</div>
<div id='tab-collect-membership-heatmap-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-3-1.png" alt="plot of chunk tab-collect-membership-heatmap-3"/></p>

</div>
<div id='tab-collect-membership-heatmap-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-4-1.png" alt="plot of chunk tab-collect-membership-heatmap-4"/></p>

</div>
<div id='tab-collect-membership-heatmap-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = membership_heatmap, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-membership-heatmap-5-1.png" alt="plot of chunk tab-collect-membership-heatmap-5"/></p>

</div>
</div>



### Signature heatmap

Signature heatmaps for all methods. ([What is a signature heatmap?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_22))


Note in following heatmaps, rows are scaled.



<script>
$( function() {
	$( '#tabs-collect-get-signatures' ).tabs();
} );
</script>
<div id='tabs-collect-get-signatures'>
<ul>
<li><a href='#tab-collect-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-collect-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-collect-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-collect-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-collect-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-collect-get-signatures-1'>
<pre><code class="r">collect_plots(res_list, k = 2, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-1-1.png" alt="plot of chunk tab-collect-get-signatures-1"/></p>

</div>
<div id='tab-collect-get-signatures-2'>
<pre><code class="r">collect_plots(res_list, k = 3, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-2-1.png" alt="plot of chunk tab-collect-get-signatures-2"/></p>

</div>
<div id='tab-collect-get-signatures-3'>
<pre><code class="r">collect_plots(res_list, k = 4, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-3-1.png" alt="plot of chunk tab-collect-get-signatures-3"/></p>

</div>
<div id='tab-collect-get-signatures-4'>
<pre><code class="r">collect_plots(res_list, k = 5, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-4-1.png" alt="plot of chunk tab-collect-get-signatures-4"/></p>

</div>
<div id='tab-collect-get-signatures-5'>
<pre><code class="r">collect_plots(res_list, k = 6, fun = get_signatures, mc.cores = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-get-signatures-5-1.png" alt="plot of chunk tab-collect-get-signatures-5"/></p>

</div>
</div>



### Statistics table

The statistics used for measuring the stability of consensus partitioning.
([How are they
defined?](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13))


<script>
$( function() {
	$( '#tabs-get-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-get-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-get-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-get-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-get-stats-from-consensus-partition-list-1'>
<pre><code class="r">get_stats(res_list, k = 2)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      2 0.437           0.768       0.879          0.377 0.655   0.655
#&gt; CV:NMF      2 1.000           0.971       0.990          0.310 0.704   0.704
#&gt; MAD:NMF     2 0.590           0.831       0.921          0.479 0.514   0.514
#&gt; ATC:NMF     2 0.910           0.903       0.960          0.403 0.594   0.594
#&gt; SD:skmeans  2 0.546           0.774       0.893          0.501 0.490   0.490
#&gt; CV:skmeans  2 1.000           0.971       0.984          0.431 0.576   0.576
#&gt; MAD:skmeans 2 0.645           0.897       0.948          0.509 0.490   0.490
#&gt; ATC:skmeans 2 1.000           0.975       0.989          0.509 0.492   0.492
#&gt; SD:mclust   2 0.759           0.938       0.964          0.331 0.678   0.678
#&gt; CV:mclust   2 0.844           0.873       0.940          0.358 0.594   0.594
#&gt; MAD:mclust  2 0.813           0.928       0.943          0.331 0.678   0.678
#&gt; ATC:mclust  2 0.405           0.818       0.887          0.325 0.678   0.678
#&gt; SD:kmeans   2 0.349           0.585       0.824          0.377 0.576   0.576
#&gt; CV:kmeans   2 1.000           0.996       0.997          0.244 0.758   0.758
#&gt; MAD:kmeans  2 0.369           0.781       0.862          0.470 0.492   0.492
#&gt; ATC:kmeans  2 0.769           0.885       0.929          0.481 0.500   0.500
#&gt; SD:pam      2 0.433           0.836       0.867          0.452 0.492   0.492
#&gt; CV:pam      2 1.000           0.991       0.996          0.219 0.788   0.788
#&gt; MAD:pam     2 0.764           0.879       0.949          0.503 0.500   0.500
#&gt; ATC:pam     2 1.000           0.995       0.998          0.509 0.492   0.492
#&gt; SD:hclust   2 0.301           0.590       0.775          0.332 0.758   0.758
#&gt; CV:hclust   2 1.000           0.966       0.987          0.250 0.758   0.758
#&gt; MAD:hclust  2 0.327           0.618       0.837          0.427 0.547   0.547
#&gt; ATC:hclust  2 0.286           0.285       0.682          0.371 0.576   0.576
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-2'>
<pre><code class="r">get_stats(res_list, k = 3)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      3 0.869           0.902       0.959          0.636 0.707   0.559
#&gt; CV:NMF      3 0.779           0.812       0.929          1.057 0.655   0.509
#&gt; MAD:NMF     3 0.793           0.858       0.923          0.283 0.800   0.636
#&gt; ATC:NMF     3 0.571           0.817       0.901          0.467 0.802   0.669
#&gt; SD:skmeans  3 0.541           0.725       0.847          0.314 0.791   0.604
#&gt; CV:skmeans  3 0.679           0.838       0.891          0.560 0.721   0.527
#&gt; MAD:skmeans 3 0.630           0.808       0.878          0.322 0.773   0.567
#&gt; ATC:skmeans 3 0.881           0.934       0.962          0.311 0.711   0.479
#&gt; SD:mclust   3 0.434           0.588       0.799          0.643 0.730   0.630
#&gt; CV:mclust   3 0.401           0.603       0.741          0.589 0.755   0.588
#&gt; MAD:mclust  3 0.254           0.352       0.729          0.609 0.768   0.683
#&gt; ATC:mclust  3 0.365           0.574       0.730          0.698 0.773   0.675
#&gt; SD:kmeans   3 0.458           0.682       0.835          0.541 0.774   0.635
#&gt; CV:kmeans   3 0.441           0.592       0.812          1.383 0.624   0.504
#&gt; MAD:kmeans  3 0.490           0.542       0.758          0.322 0.775   0.582
#&gt; ATC:kmeans  3 0.526           0.624       0.818          0.292 0.818   0.658
#&gt; SD:pam      3 0.715           0.846       0.934          0.320 0.704   0.507
#&gt; CV:pam      3 0.528           0.723       0.885          1.708 0.613   0.508
#&gt; MAD:pam     3 0.648           0.801       0.903          0.201 0.884   0.771
#&gt; ATC:pam     3 0.779           0.893       0.934          0.224 0.901   0.799
#&gt; SD:hclust   3 0.525           0.649       0.858          0.601 0.685   0.589
#&gt; CV:hclust   3 0.433           0.746       0.794          0.595 0.961   0.948
#&gt; MAD:hclust  3 0.398           0.573       0.789          0.344 0.886   0.792
#&gt; ATC:hclust  3 0.466           0.641       0.806          0.547 0.784   0.626
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-3'>
<pre><code class="r">get_stats(res_list, k = 4)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      4 0.671           0.752       0.887         0.1894 0.735   0.418
#&gt; CV:NMF      4 0.775           0.688       0.876         0.1455 0.869   0.656
#&gt; MAD:NMF     4 0.608           0.625       0.826         0.2007 0.751   0.438
#&gt; ATC:NMF     4 0.531           0.642       0.807         0.1946 0.685   0.367
#&gt; SD:skmeans  4 0.660           0.723       0.855         0.1394 0.795   0.490
#&gt; CV:skmeans  4 0.625           0.541       0.753         0.1079 0.900   0.713
#&gt; MAD:skmeans 4 0.678           0.760       0.875         0.1128 0.833   0.552
#&gt; ATC:skmeans 4 0.874           0.912       0.958         0.1118 0.906   0.722
#&gt; SD:mclust   4 0.497           0.518       0.746         0.1570 0.899   0.806
#&gt; CV:mclust   4 0.337           0.460       0.672         0.1535 0.789   0.495
#&gt; MAD:mclust  4 0.336           0.428       0.669         0.1908 0.703   0.507
#&gt; ATC:mclust  4 0.490           0.699       0.801         0.2160 0.713   0.477
#&gt; SD:kmeans   4 0.493           0.489       0.650         0.2117 0.824   0.622
#&gt; CV:kmeans   4 0.499           0.649       0.778         0.2052 0.787   0.523
#&gt; MAD:kmeans  4 0.504           0.638       0.727         0.1544 0.780   0.477
#&gt; ATC:kmeans  4 0.647           0.838       0.874         0.1330 0.784   0.517
#&gt; SD:pam      4 0.822           0.880       0.949         0.1541 0.815   0.596
#&gt; CV:pam      4 0.516           0.660       0.808         0.1673 0.848   0.634
#&gt; MAD:pam     4 0.807           0.853       0.942         0.1405 0.911   0.774
#&gt; ATC:pam     4 0.957           0.925       0.971         0.1711 0.831   0.595
#&gt; SD:hclust   4 0.580           0.698       0.862         0.1842 0.871   0.729
#&gt; CV:hclust   4 0.377           0.778       0.818         0.0668 0.990   0.986
#&gt; MAD:hclust  4 0.496           0.651       0.802         0.1439 0.867   0.705
#&gt; ATC:hclust  4 0.583           0.603       0.769         0.1377 0.960   0.890
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-4'>
<pre><code class="r">get_stats(res_list, k = 5)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      5 0.749           0.725       0.854         0.0816 0.856   0.531
#&gt; CV:NMF      5 0.715           0.657       0.836         0.0737 0.934   0.768
#&gt; MAD:NMF     5 0.738           0.753       0.857         0.0788 0.801   0.391
#&gt; ATC:NMF     5 0.682           0.681       0.826         0.1021 0.841   0.508
#&gt; SD:skmeans  5 0.711           0.731       0.839         0.0702 0.892   0.605
#&gt; CV:skmeans  5 0.596           0.475       0.703         0.0701 0.865   0.556
#&gt; MAD:skmeans 5 0.758           0.659       0.799         0.0670 0.912   0.674
#&gt; ATC:skmeans 5 0.877           0.868       0.901         0.0560 0.929   0.740
#&gt; SD:mclust   5 0.481           0.647       0.750         0.1220 0.736   0.454
#&gt; CV:mclust   5 0.394           0.511       0.630         0.1028 0.869   0.628
#&gt; MAD:mclust  5 0.382           0.581       0.724         0.1357 0.701   0.371
#&gt; ATC:mclust  5 0.557           0.487       0.704         0.0812 0.769   0.414
#&gt; SD:kmeans   5 0.660           0.746       0.792         0.0903 0.873   0.622
#&gt; CV:kmeans   5 0.604           0.585       0.755         0.0859 0.920   0.747
#&gt; MAD:kmeans  5 0.705           0.787       0.859         0.0844 0.896   0.641
#&gt; ATC:kmeans  5 0.760           0.707       0.773         0.0832 0.993   0.976
#&gt; SD:pam      5 0.870           0.876       0.946         0.1248 0.887   0.656
#&gt; CV:pam      5 0.535           0.682       0.798         0.0514 0.918   0.745
#&gt; MAD:pam     5 0.831           0.864       0.941         0.1220 0.893   0.665
#&gt; ATC:pam     5 0.909           0.876       0.950         0.0449 0.965   0.873
#&gt; SD:hclust   5 0.559           0.658       0.813         0.0882 0.984   0.954
#&gt; CV:hclust   5 0.303           0.644       0.761         0.2433 0.845   0.784
#&gt; MAD:hclust  5 0.539           0.648       0.720         0.1380 0.842   0.558
#&gt; ATC:hclust  5 0.702           0.660       0.771         0.1119 0.934   0.806
</code></pre>

</div>
<div id='tab-get-stats-from-consensus-partition-list-5'>
<pre><code class="r">get_stats(res_list, k = 6)
</code></pre>

<pre><code>#&gt;             k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#&gt; SD:NMF      6 0.799           0.689       0.857         0.0389 0.928   0.678
#&gt; CV:NMF      6 0.764           0.707       0.794         0.0522 0.908   0.631
#&gt; MAD:NMF     6 0.768           0.669       0.837         0.0365 0.920   0.648
#&gt; ATC:NMF     6 0.658           0.529       0.772         0.0441 0.875   0.531
#&gt; SD:skmeans  6 0.730           0.647       0.804         0.0407 0.955   0.776
#&gt; CV:skmeans  6 0.644           0.518       0.686         0.0438 0.899   0.560
#&gt; MAD:skmeans 6 0.717           0.645       0.791         0.0420 0.945   0.745
#&gt; ATC:skmeans 6 0.909           0.896       0.921         0.0447 0.953   0.789
#&gt; SD:mclust   6 0.624           0.677       0.779         0.0959 0.970   0.890
#&gt; CV:mclust   6 0.549           0.421       0.629         0.0760 0.868   0.557
#&gt; MAD:mclust  6 0.545           0.665       0.721         0.1019 0.864   0.564
#&gt; ATC:mclust  6 0.696           0.706       0.823         0.1293 0.894   0.603
#&gt; SD:kmeans   6 0.677           0.614       0.742         0.0571 0.915   0.666
#&gt; CV:kmeans   6 0.676           0.634       0.731         0.0489 0.911   0.660
#&gt; MAD:kmeans  6 0.798           0.744       0.801         0.0466 0.967   0.857
#&gt; ATC:kmeans  6 0.787           0.827       0.863         0.0592 0.852   0.524
#&gt; SD:pam      6 0.872           0.845       0.932         0.0307 0.975   0.889
#&gt; CV:pam      6 0.621           0.578       0.809         0.0574 0.927   0.751
#&gt; MAD:pam     6 0.806           0.740       0.881         0.0473 0.976   0.895
#&gt; ATC:pam     6 0.934           0.906       0.954         0.0139 0.997   0.987
#&gt; SD:hclust   6 0.574           0.392       0.764         0.0598 0.936   0.821
#&gt; CV:hclust   6 0.357           0.538       0.741         0.1431 0.966   0.941
#&gt; MAD:hclust  6 0.579           0.591       0.707         0.0364 0.920   0.692
#&gt; ATC:hclust  6 0.736           0.550       0.746         0.0688 0.846   0.515
</code></pre>

</div>
</div>

Following heatmap plots the partition for each combination of methods and the
lightness correspond to the silhouette scores for samples in each method. On
top the consensus subgroup is inferred from all methods by taking the mean
silhouette scores as weight.


<script>
$( function() {
	$( '#tabs-collect-stats-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-stats-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-stats-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-stats-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-stats-from-consensus-partition-list-1'>
<pre><code class="r">collect_stats(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-2'>
<pre><code class="r">collect_stats(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-3'>
<pre><code class="r">collect_stats(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-4'>
<pre><code class="r">collect_stats(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-stats-from-consensus-partition-list-5'>
<pre><code class="r">collect_stats(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-stats-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-stats-from-consensus-partition-list-5"/></p>

</div>
</div>

### Partition from all methods



Collect partitions from all methods:


<script>
$( function() {
	$( '#tabs-collect-classes-from-consensus-partition-list' ).tabs();
} );
</script>
<div id='tabs-collect-classes-from-consensus-partition-list'>
<ul>
<li><a href='#tab-collect-classes-from-consensus-partition-list-1'>k = 2</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-2'>k = 3</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-3'>k = 4</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-4'>k = 5</a></li>
<li><a href='#tab-collect-classes-from-consensus-partition-list-5'>k = 6</a></li>
</ul>
<div id='tab-collect-classes-from-consensus-partition-list-1'>
<pre><code class="r">collect_classes(res_list, k = 2)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-1-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-1"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-2'>
<pre><code class="r">collect_classes(res_list, k = 3)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-2-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-2"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-3'>
<pre><code class="r">collect_classes(res_list, k = 4)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-3-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-3"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-4'>
<pre><code class="r">collect_classes(res_list, k = 5)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-4-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-4"/></p>

</div>
<div id='tab-collect-classes-from-consensus-partition-list-5'>
<pre><code class="r">collect_classes(res_list, k = 6)
</code></pre>

<p><img src="figure_cola/tab-collect-classes-from-consensus-partition-list-5-1.png" alt="plot of chunk tab-collect-classes-from-consensus-partition-list-5"/></p>

</div>
</div>



### Top rows overlap


Overlap of top rows from different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-euler' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-euler'>
<ul>
<li><a href='#tab-top-rows-overlap-by-euler-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-euler-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-euler-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-euler-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;euler&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-euler-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-euler-5"/></p>

</div>
</div>

Also visualize the correspondance of rankings between different top-row methods:


<script>
$( function() {
	$( '#tabs-top-rows-overlap-by-correspondance' ).tabs();
} );
</script>
<div id='tabs-top-rows-overlap-by-correspondance'>
<ul>
<li><a href='#tab-top-rows-overlap-by-correspondance-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-overlap-by-correspondance-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-overlap-by-correspondance-1'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 1000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-1-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-1"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-2'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 2000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-2-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-2"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-3'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 3000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-3-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-3"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-4'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 4000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-4-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-4"/></p>

</div>
<div id='tab-top-rows-overlap-by-correspondance-5'>
<pre><code class="r">top_rows_overlap(res_list, top_n = 5000, method = &quot;correspondance&quot;)
</code></pre>

<p><img src="figure_cola/tab-top-rows-overlap-by-correspondance-5-1.png" alt="plot of chunk tab-top-rows-overlap-by-correspondance-5"/></p>

</div>
</div>


Heatmaps of the top rows:



<script>
$( function() {
	$( '#tabs-top-rows-heatmap' ).tabs();
} );
</script>
<div id='tabs-top-rows-heatmap'>
<ul>
<li><a href='#tab-top-rows-heatmap-1'>top_n = 1000</a></li>
<li><a href='#tab-top-rows-heatmap-2'>top_n = 2000</a></li>
<li><a href='#tab-top-rows-heatmap-3'>top_n = 3000</a></li>
<li><a href='#tab-top-rows-heatmap-4'>top_n = 4000</a></li>
<li><a href='#tab-top-rows-heatmap-5'>top_n = 5000</a></li>
</ul>
<div id='tab-top-rows-heatmap-1'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 1000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-1-1.png" alt="plot of chunk tab-top-rows-heatmap-1"/></p>

</div>
<div id='tab-top-rows-heatmap-2'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 2000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-2-1.png" alt="plot of chunk tab-top-rows-heatmap-2"/></p>

</div>
<div id='tab-top-rows-heatmap-3'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 3000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-3-1.png" alt="plot of chunk tab-top-rows-heatmap-3"/></p>

</div>
<div id='tab-top-rows-heatmap-4'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 4000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-4-1.png" alt="plot of chunk tab-top-rows-heatmap-4"/></p>

</div>
<div id='tab-top-rows-heatmap-5'>
<pre><code class="r">top_rows_heatmap(res_list, top_n = 5000)
</code></pre>

<p><img src="figure_cola/tab-top-rows-heatmap-5-1.png" alt="plot of chunk tab-top-rows-heatmap-5"/></p>

</div>
</div>



 
## Results for each method


---------------------------------------------------




### SD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "hclust"]
# you can also extract it by
# res = res_list["SD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-hclust-collect-plots](figure_cola/SD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-hclust-select-partition-number](figure_cola/SD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.301           0.590       0.775         0.3319 0.758   0.758
#> 3 3 0.525           0.649       0.858         0.6013 0.685   0.589
#> 4 4 0.580           0.698       0.862         0.1842 0.871   0.729
#> 5 5 0.559           0.658       0.813         0.0882 0.984   0.954
#> 6 6 0.574           0.392       0.764         0.0598 0.936   0.821
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-classes'>
<ul>
<li><a href='#tab-SD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-hclust-get-classes-1'>
<p><a id='tab-SD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.9323     0.8356 0.652 0.348
#&gt; SRR1812753     1  0.9323     0.8356 0.652 0.348
#&gt; SRR1812751     1  0.9323     0.8356 0.652 0.348
#&gt; SRR1812750     1  0.9323     0.8356 0.652 0.348
#&gt; SRR1812748     2  0.9323     0.3765 0.348 0.652
#&gt; SRR1812749     1  0.9323     0.8356 0.652 0.348
#&gt; SRR1812746     1  0.9815    -0.0177 0.580 0.420
#&gt; SRR1812745     2  0.9323     0.3765 0.348 0.652
#&gt; SRR1812747     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812744     2  0.8861     0.4258 0.304 0.696
#&gt; SRR1812743     2  0.6247     0.5629 0.156 0.844
#&gt; SRR1812742     2  0.6247     0.5629 0.156 0.844
#&gt; SRR1812737     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812735     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812734     2  0.9209     0.3913 0.336 0.664
#&gt; SRR1812733     2  0.7139     0.6568 0.196 0.804
#&gt; SRR1812736     2  0.9323     0.3765 0.348 0.652
#&gt; SRR1812732     2  0.6712     0.5545 0.176 0.824
#&gt; SRR1812730     2  0.7950     0.6385 0.240 0.760
#&gt; SRR1812731     2  0.2423     0.6407 0.040 0.960
#&gt; SRR1812729     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812727     2  0.9996    -0.0719 0.488 0.512
#&gt; SRR1812726     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812728     2  0.6531     0.6636 0.168 0.832
#&gt; SRR1812724     2  0.4298     0.6143 0.088 0.912
#&gt; SRR1812725     2  0.7950     0.6385 0.240 0.760
#&gt; SRR1812723     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812722     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812721     2  0.5059     0.5962 0.112 0.888
#&gt; SRR1812718     2  0.7056     0.6562 0.192 0.808
#&gt; SRR1812717     2  0.3733     0.6239 0.072 0.928
#&gt; SRR1812716     2  0.7950     0.6385 0.240 0.760
#&gt; SRR1812715     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812714     2  0.7056     0.6562 0.192 0.808
#&gt; SRR1812719     2  0.9996    -0.0719 0.488 0.512
#&gt; SRR1812713     2  0.7056     0.6562 0.192 0.808
#&gt; SRR1812712     2  0.0672     0.6472 0.008 0.992
#&gt; SRR1812711     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812710     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812709     2  0.0672     0.6472 0.008 0.992
#&gt; SRR1812708     1  0.9460     0.8016 0.636 0.364
#&gt; SRR1812707     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812705     2  0.6973     0.6567 0.188 0.812
#&gt; SRR1812706     2  0.3879     0.6277 0.076 0.924
#&gt; SRR1812704     2  0.6623     0.6535 0.172 0.828
#&gt; SRR1812703     2  0.7056     0.6562 0.192 0.808
#&gt; SRR1812702     2  0.7950     0.6385 0.240 0.760
#&gt; SRR1812741     2  0.5842     0.5768 0.140 0.860
#&gt; SRR1812740     2  0.9323     0.3765 0.348 0.652
#&gt; SRR1812739     2  0.2423     0.6407 0.040 0.960
#&gt; SRR1812738     2  0.3733     0.6239 0.072 0.928
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-1-a').click(function(){
  $('#tab-SD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-2'>
<p><a id='tab-SD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0237     0.8882 0.996 0.004 0.000
#&gt; SRR1812753     1  0.0237     0.8882 0.996 0.004 0.000
#&gt; SRR1812751     1  0.0237     0.8882 0.996 0.004 0.000
#&gt; SRR1812750     1  0.0237     0.8882 0.996 0.004 0.000
#&gt; SRR1812748     3  0.0000     0.6132 0.000 0.000 1.000
#&gt; SRR1812749     1  0.0237     0.8882 0.996 0.004 0.000
#&gt; SRR1812746     3  0.4931     0.3931 0.232 0.000 0.768
#&gt; SRR1812745     3  0.0000     0.6132 0.000 0.000 1.000
#&gt; SRR1812747     2  0.0475     0.8443 0.004 0.992 0.004
#&gt; SRR1812744     3  0.2384     0.6228 0.008 0.056 0.936
#&gt; SRR1812743     3  0.6442     0.2164 0.004 0.432 0.564
#&gt; SRR1812742     3  0.6359     0.2895 0.004 0.404 0.592
#&gt; SRR1812737     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812735     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812734     3  0.1399     0.6222 0.004 0.028 0.968
#&gt; SRR1812733     2  0.0661     0.8418 0.004 0.988 0.008
#&gt; SRR1812736     3  0.0000     0.6132 0.000 0.000 1.000
#&gt; SRR1812732     3  0.6359     0.2858 0.004 0.404 0.592
#&gt; SRR1812730     2  0.2682     0.7975 0.004 0.920 0.076
#&gt; SRR1812731     2  0.6189     0.3772 0.004 0.632 0.364
#&gt; SRR1812729     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812727     3  0.9865     0.1205 0.268 0.324 0.408
#&gt; SRR1812726     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812728     2  0.2200     0.8229 0.004 0.940 0.056
#&gt; SRR1812724     2  0.6286     0.0517 0.000 0.536 0.464
#&gt; SRR1812725     2  0.2682     0.7975 0.004 0.920 0.076
#&gt; SRR1812723     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812722     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812721     2  0.6008     0.4586 0.004 0.664 0.332
#&gt; SRR1812718     2  0.0237     0.8431 0.004 0.996 0.000
#&gt; SRR1812717     2  0.5397     0.5608 0.000 0.720 0.280
#&gt; SRR1812716     2  0.2682     0.7975 0.004 0.920 0.076
#&gt; SRR1812715     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812714     2  0.0237     0.8431 0.004 0.996 0.000
#&gt; SRR1812719     3  0.9865     0.1205 0.268 0.324 0.408
#&gt; SRR1812713     2  0.0237     0.8431 0.004 0.996 0.000
#&gt; SRR1812712     2  0.4750     0.6565 0.000 0.784 0.216
#&gt; SRR1812711     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812710     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812709     2  0.4750     0.6565 0.000 0.784 0.216
#&gt; SRR1812708     1  0.6045     0.3473 0.620 0.380 0.000
#&gt; SRR1812707     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812705     2  0.0237     0.8453 0.004 0.996 0.000
#&gt; SRR1812706     2  0.5815     0.5670 0.004 0.692 0.304
#&gt; SRR1812704     2  0.4409     0.7386 0.004 0.824 0.172
#&gt; SRR1812703     2  0.0237     0.8431 0.004 0.996 0.000
#&gt; SRR1812702     2  0.2682     0.7975 0.004 0.920 0.076
#&gt; SRR1812741     3  0.6295     0.1064 0.000 0.472 0.528
#&gt; SRR1812740     3  0.0000     0.6132 0.000 0.000 1.000
#&gt; SRR1812739     2  0.5988     0.3671 0.000 0.632 0.368
#&gt; SRR1812738     2  0.6095     0.3027 0.000 0.608 0.392
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-2-a').click(function(){
  $('#tab-SD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-3'>
<p><a id='tab-SD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.1389      0.771 0.000 0.000 0.952 0.048
#&gt; SRR1812749     1  0.0000      0.868 1.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.3837      0.589 0.224 0.000 0.776 0.000
#&gt; SRR1812745     3  0.1211      0.771 0.000 0.000 0.960 0.040
#&gt; SRR1812747     2  0.0524      0.856 0.000 0.988 0.004 0.008
#&gt; SRR1812744     3  0.2730      0.736 0.000 0.016 0.896 0.088
#&gt; SRR1812743     4  0.1297      0.596 0.000 0.016 0.020 0.964
#&gt; SRR1812742     4  0.3659      0.523 0.000 0.024 0.136 0.840
#&gt; SRR1812737     2  0.2868      0.773 0.000 0.864 0.000 0.136
#&gt; SRR1812735     2  0.0469      0.856 0.000 0.988 0.000 0.012
#&gt; SRR1812734     3  0.1118      0.759 0.000 0.000 0.964 0.036
#&gt; SRR1812733     2  0.0336      0.855 0.000 0.992 0.008 0.000
#&gt; SRR1812736     3  0.1389      0.771 0.000 0.000 0.952 0.048
#&gt; SRR1812732     4  0.6310      0.640 0.000 0.188 0.152 0.660
#&gt; SRR1812730     2  0.1940      0.815 0.000 0.924 0.076 0.000
#&gt; SRR1812731     4  0.4761      0.569 0.000 0.372 0.000 0.628
#&gt; SRR1812729     2  0.1474      0.836 0.000 0.948 0.000 0.052
#&gt; SRR1812727     3  0.8677      0.218 0.260 0.288 0.412 0.040
#&gt; SRR1812726     2  0.0336      0.856 0.000 0.992 0.000 0.008
#&gt; SRR1812728     2  0.1807      0.834 0.000 0.940 0.052 0.008
#&gt; SRR1812724     4  0.4008      0.730 0.000 0.244 0.000 0.756
#&gt; SRR1812725     2  0.1940      0.815 0.000 0.924 0.076 0.000
#&gt; SRR1812723     2  0.0336      0.856 0.000 0.992 0.000 0.008
#&gt; SRR1812722     2  0.0469      0.856 0.000 0.988 0.000 0.012
#&gt; SRR1812721     4  0.4761      0.518 0.000 0.372 0.000 0.628
#&gt; SRR1812718     2  0.0000      0.855 0.000 1.000 0.000 0.000
#&gt; SRR1812717     2  0.4817      0.194 0.000 0.612 0.000 0.388
#&gt; SRR1812716     2  0.1940      0.815 0.000 0.924 0.076 0.000
#&gt; SRR1812715     2  0.0469      0.856 0.000 0.988 0.000 0.012
#&gt; SRR1812714     2  0.0592      0.850 0.000 0.984 0.000 0.016
#&gt; SRR1812719     3  0.8677      0.218 0.260 0.288 0.412 0.040
#&gt; SRR1812713     2  0.0469      0.855 0.000 0.988 0.000 0.012
#&gt; SRR1812712     2  0.4356      0.470 0.000 0.708 0.000 0.292
#&gt; SRR1812711     2  0.0336      0.856 0.000 0.992 0.000 0.008
#&gt; SRR1812710     2  0.2868      0.773 0.000 0.864 0.000 0.136
#&gt; SRR1812709     2  0.4356      0.470 0.000 0.708 0.000 0.292
#&gt; SRR1812708     1  0.5592      0.288 0.608 0.368 0.008 0.016
#&gt; SRR1812707     2  0.2868      0.773 0.000 0.864 0.000 0.136
#&gt; SRR1812705     2  0.0336      0.856 0.000 0.992 0.000 0.008
#&gt; SRR1812706     2  0.5693      0.506 0.000 0.688 0.072 0.240
#&gt; SRR1812704     2  0.4301      0.732 0.000 0.816 0.064 0.120
#&gt; SRR1812703     2  0.0592      0.850 0.000 0.984 0.000 0.016
#&gt; SRR1812702     2  0.1940      0.815 0.000 0.924 0.076 0.000
#&gt; SRR1812741     4  0.2530      0.694 0.000 0.112 0.000 0.888
#&gt; SRR1812740     3  0.1389      0.771 0.000 0.000 0.952 0.048
#&gt; SRR1812739     2  0.4994     -0.241 0.000 0.520 0.000 0.480
#&gt; SRR1812738     4  0.4713      0.608 0.000 0.360 0.000 0.640
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-3-a').click(function(){
  $('#tab-SD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-4'>
<p><a id='tab-SD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000      0.689 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.689 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.4088      0.797 0.632 0.000 0.000 0.000 0.368
#&gt; SRR1812750     1  0.4088      0.797 0.632 0.000 0.000 0.000 0.368
#&gt; SRR1812748     3  0.0880      0.879 0.000 0.000 0.968 0.032 0.000
#&gt; SRR1812749     1  0.4088      0.797 0.632 0.000 0.000 0.000 0.368
#&gt; SRR1812746     3  0.4394      0.574 0.100 0.000 0.764 0.000 0.136
#&gt; SRR1812745     3  0.0703      0.877 0.000 0.000 0.976 0.024 0.000
#&gt; SRR1812747     2  0.1124      0.784 0.000 0.960 0.004 0.000 0.036
#&gt; SRR1812744     3  0.3653      0.754 0.000 0.016 0.840 0.056 0.088
#&gt; SRR1812743     4  0.1300      0.569 0.000 0.000 0.028 0.956 0.016
#&gt; SRR1812742     4  0.3284      0.490 0.000 0.000 0.148 0.828 0.024
#&gt; SRR1812737     2  0.2377      0.713 0.000 0.872 0.000 0.128 0.000
#&gt; SRR1812735     2  0.0162      0.787 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812734     3  0.2077      0.816 0.000 0.000 0.908 0.008 0.084
#&gt; SRR1812733     2  0.1484      0.783 0.000 0.944 0.008 0.000 0.048
#&gt; SRR1812736     3  0.0880      0.879 0.000 0.000 0.968 0.032 0.000
#&gt; SRR1812732     4  0.5494      0.609 0.000 0.172 0.156 0.668 0.004
#&gt; SRR1812730     2  0.4155      0.705 0.000 0.780 0.076 0.000 0.144
#&gt; SRR1812731     4  0.4196      0.559 0.000 0.356 0.000 0.640 0.004
#&gt; SRR1812729     2  0.1408      0.772 0.000 0.948 0.000 0.044 0.008
#&gt; SRR1812727     5  0.6819      0.568 0.124 0.004 0.352 0.028 0.492
#&gt; SRR1812726     2  0.0000      0.787 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812728     2  0.4909      0.655 0.000 0.716 0.052 0.016 0.216
#&gt; SRR1812724     4  0.3663      0.699 0.000 0.208 0.000 0.776 0.016
#&gt; SRR1812725     2  0.4155      0.704 0.000 0.780 0.076 0.000 0.144
#&gt; SRR1812723     2  0.0290      0.787 0.000 0.992 0.000 0.000 0.008
#&gt; SRR1812722     2  0.0162      0.787 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812721     4  0.5289      0.469 0.000 0.340 0.000 0.596 0.064
#&gt; SRR1812718     2  0.1197      0.784 0.000 0.952 0.000 0.000 0.048
#&gt; SRR1812717     2  0.4630      0.223 0.000 0.588 0.000 0.396 0.016
#&gt; SRR1812716     2  0.4155      0.705 0.000 0.780 0.076 0.000 0.144
#&gt; SRR1812715     2  0.0162      0.787 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812714     2  0.3837      0.608 0.000 0.692 0.000 0.000 0.308
#&gt; SRR1812719     5  0.6819      0.568 0.124 0.004 0.352 0.028 0.492
#&gt; SRR1812713     2  0.1195      0.786 0.000 0.960 0.000 0.012 0.028
#&gt; SRR1812712     2  0.4923      0.457 0.000 0.680 0.000 0.252 0.068
#&gt; SRR1812711     2  0.0000      0.787 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812710     2  0.2377      0.713 0.000 0.872 0.000 0.128 0.000
#&gt; SRR1812709     2  0.4923      0.457 0.000 0.680 0.000 0.252 0.068
#&gt; SRR1812708     5  0.3691      0.107 0.104 0.076 0.000 0.000 0.820
#&gt; SRR1812707     2  0.2377      0.713 0.000 0.872 0.000 0.128 0.000
#&gt; SRR1812705     2  0.0290      0.787 0.000 0.992 0.000 0.000 0.008
#&gt; SRR1812706     2  0.7145      0.404 0.000 0.548 0.072 0.196 0.184
#&gt; SRR1812704     2  0.5683      0.659 0.000 0.708 0.064 0.124 0.104
#&gt; SRR1812703     2  0.3837      0.608 0.000 0.692 0.000 0.000 0.308
#&gt; SRR1812702     2  0.4155      0.704 0.000 0.780 0.076 0.000 0.144
#&gt; SRR1812741     4  0.1956      0.660 0.000 0.076 0.000 0.916 0.008
#&gt; SRR1812740     3  0.0880      0.879 0.000 0.000 0.968 0.032 0.000
#&gt; SRR1812739     2  0.4659     -0.203 0.000 0.500 0.000 0.488 0.012
#&gt; SRR1812738     4  0.4384      0.594 0.000 0.324 0.000 0.660 0.016
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-4-a').click(function(){
  $('#tab-SD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-hclust-get-classes-5'>
<p><a id='tab-SD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.4871    0.57359 0.580 0.000 0.000 0.000 0.072 0.348
#&gt; SRR1812753     1  0.4871    0.57359 0.580 0.000 0.000 0.000 0.072 0.348
#&gt; SRR1812751     1  0.0000    0.70467 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000    0.70467 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000    0.84397 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812749     1  0.0000    0.70467 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.4660    0.45370 0.024 0.000 0.644 0.000 0.304 0.028
#&gt; SRR1812745     3  0.2300    0.73813 0.000 0.000 0.856 0.000 0.144 0.000
#&gt; SRR1812747     2  0.2442    0.45610 0.000 0.852 0.004 0.000 0.000 0.144
#&gt; SRR1812744     5  0.5818    0.16326 0.000 0.016 0.388 0.040 0.512 0.044
#&gt; SRR1812743     4  0.2036    0.49503 0.000 0.000 0.048 0.916 0.008 0.028
#&gt; SRR1812742     4  0.3604    0.39199 0.000 0.000 0.168 0.788 0.008 0.036
#&gt; SRR1812737     2  0.2869    0.46618 0.000 0.832 0.000 0.148 0.000 0.020
#&gt; SRR1812735     2  0.0405    0.56990 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; SRR1812734     5  0.4526    0.04515 0.000 0.000 0.456 0.000 0.512 0.032
#&gt; SRR1812733     2  0.2700    0.46755 0.000 0.836 0.004 0.000 0.004 0.156
#&gt; SRR1812736     3  0.0000    0.84397 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812732     4  0.5617    0.57052 0.000 0.152 0.068 0.676 0.092 0.012
#&gt; SRR1812730     2  0.4177    0.00966 0.000 0.668 0.008 0.000 0.020 0.304
#&gt; SRR1812731     4  0.4031    0.52831 0.000 0.332 0.000 0.652 0.008 0.008
#&gt; SRR1812729     2  0.2511    0.54084 0.000 0.880 0.000 0.064 0.000 0.056
#&gt; SRR1812727     5  0.3309    0.50421 0.004 0.000 0.016 0.000 0.788 0.192
#&gt; SRR1812726     2  0.0547    0.56409 0.000 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1812728     2  0.5548   -0.19899 0.000 0.608 0.008 0.004 0.184 0.196
#&gt; SRR1812724     4  0.3536    0.63238 0.000 0.184 0.000 0.784 0.020 0.012
#&gt; SRR1812725     2  0.4103    0.05250 0.000 0.684 0.008 0.000 0.020 0.288
#&gt; SRR1812723     2  0.1204    0.54962 0.000 0.944 0.000 0.000 0.000 0.056
#&gt; SRR1812722     2  0.0291    0.56939 0.000 0.992 0.000 0.004 0.000 0.004
#&gt; SRR1812721     4  0.6011    0.19512 0.000 0.296 0.000 0.432 0.000 0.272
#&gt; SRR1812718     2  0.2178    0.48728 0.000 0.868 0.000 0.000 0.000 0.132
#&gt; SRR1812717     2  0.4943   -0.03657 0.000 0.544 0.000 0.404 0.020 0.032
#&gt; SRR1812716     2  0.4177    0.00966 0.000 0.668 0.008 0.000 0.020 0.304
#&gt; SRR1812715     2  0.0405    0.56990 0.000 0.988 0.000 0.004 0.000 0.008
#&gt; SRR1812714     2  0.4774    0.03159 0.000 0.672 0.000 0.000 0.192 0.136
#&gt; SRR1812719     5  0.3309    0.50421 0.004 0.000 0.016 0.000 0.788 0.192
#&gt; SRR1812713     2  0.1500    0.55828 0.000 0.936 0.000 0.012 0.000 0.052
#&gt; SRR1812712     2  0.4831    0.06501 0.000 0.636 0.000 0.096 0.000 0.268
#&gt; SRR1812711     2  0.0363    0.56913 0.000 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1812710     2  0.2869    0.46618 0.000 0.832 0.000 0.148 0.000 0.020
#&gt; SRR1812709     2  0.4831    0.06501 0.000 0.636 0.000 0.096 0.000 0.268
#&gt; SRR1812708     1  0.6567   -0.02550 0.428 0.076 0.000 0.000 0.380 0.116
#&gt; SRR1812707     2  0.2869    0.46618 0.000 0.832 0.000 0.148 0.000 0.020
#&gt; SRR1812705     2  0.1267    0.54685 0.000 0.940 0.000 0.000 0.000 0.060
#&gt; SRR1812706     6  0.4754    0.00000 0.000 0.432 0.004 0.012 0.020 0.532
#&gt; SRR1812704     2  0.5632   -0.17991 0.000 0.592 0.004 0.124 0.016 0.264
#&gt; SRR1812703     2  0.4774    0.03159 0.000 0.672 0.000 0.000 0.192 0.136
#&gt; SRR1812702     2  0.4177    0.00788 0.000 0.668 0.008 0.000 0.020 0.304
#&gt; SRR1812741     4  0.1921    0.58958 0.000 0.052 0.000 0.916 0.032 0.000
#&gt; SRR1812740     3  0.0000    0.84397 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812739     4  0.4336    0.17263 0.000 0.476 0.000 0.504 0.000 0.020
#&gt; SRR1812738     4  0.4590    0.55430 0.000 0.288 0.000 0.660 0.028 0.024
</code></pre>

<script>
$('#tab-SD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-hclust-get-classes-5-a').click(function(){
  $('#tab-SD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-hclust-signature_compare](figure_cola/SD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-hclust-collect-classes](figure_cola/SD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "kmeans"]
# you can also extract it by
# res = res_list["SD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-kmeans-collect-plots](figure_cola/SD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-kmeans-select-partition-number](figure_cola/SD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.349           0.585       0.824         0.3769 0.576   0.576
#> 3 3 0.458           0.682       0.835         0.5410 0.774   0.635
#> 4 4 0.493           0.489       0.650         0.2117 0.824   0.622
#> 5 5 0.660           0.746       0.792         0.0903 0.873   0.622
#> 6 6 0.677           0.614       0.742         0.0571 0.915   0.666
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-classes'>
<ul>
<li><a href='#tab-SD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-kmeans-get-classes-1'>
<p><a id='tab-SD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.2043     0.5276 0.968 0.032
#&gt; SRR1812753     1  0.2043     0.5276 0.968 0.032
#&gt; SRR1812751     1  0.9754     0.3627 0.592 0.408
#&gt; SRR1812750     1  0.9795     0.3543 0.584 0.416
#&gt; SRR1812748     1  0.9944     0.2466 0.544 0.456
#&gt; SRR1812749     1  0.9754     0.3627 0.592 0.408
#&gt; SRR1812746     1  0.2423     0.5317 0.960 0.040
#&gt; SRR1812745     1  0.9944     0.2466 0.544 0.456
#&gt; SRR1812747     2  0.0376     0.8150 0.004 0.996
#&gt; SRR1812744     2  0.9732     0.2161 0.404 0.596
#&gt; SRR1812743     2  0.9922    -0.0141 0.448 0.552
#&gt; SRR1812742     1  0.9998     0.1741 0.508 0.492
#&gt; SRR1812737     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812735     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812734     1  0.9944     0.2466 0.544 0.456
#&gt; SRR1812733     2  0.8499     0.5527 0.276 0.724
#&gt; SRR1812736     1  0.9944     0.2466 0.544 0.456
#&gt; SRR1812732     2  0.8207     0.5771 0.256 0.744
#&gt; SRR1812730     2  0.9815     0.1503 0.420 0.580
#&gt; SRR1812731     2  0.0938     0.8117 0.012 0.988
#&gt; SRR1812729     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812727     1  0.2948     0.5331 0.948 0.052
#&gt; SRR1812726     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812728     2  0.8144     0.5874 0.252 0.748
#&gt; SRR1812724     2  0.6623     0.6936 0.172 0.828
#&gt; SRR1812725     2  0.3733     0.7781 0.072 0.928
#&gt; SRR1812723     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812722     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812721     2  0.0376     0.8150 0.004 0.996
#&gt; SRR1812718     2  0.0376     0.8150 0.004 0.996
#&gt; SRR1812717     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812716     2  0.8661     0.5252 0.288 0.712
#&gt; SRR1812715     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812714     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812719     1  0.9635     0.3464 0.612 0.388
#&gt; SRR1812713     2  0.1414     0.8077 0.020 0.980
#&gt; SRR1812712     2  0.1414     0.8077 0.020 0.980
#&gt; SRR1812711     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812710     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812709     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812708     1  0.9833     0.3429 0.576 0.424
#&gt; SRR1812707     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812705     2  0.0376     0.8150 0.004 0.996
#&gt; SRR1812706     2  0.8608     0.5329 0.284 0.716
#&gt; SRR1812704     2  0.6343     0.7011 0.160 0.840
#&gt; SRR1812703     2  0.0000     0.8154 0.000 1.000
#&gt; SRR1812702     2  0.8661     0.5252 0.288 0.712
#&gt; SRR1812741     2  0.9686     0.1450 0.396 0.604
#&gt; SRR1812740     1  0.9944     0.2466 0.544 0.456
#&gt; SRR1812739     2  0.1633     0.8077 0.024 0.976
#&gt; SRR1812738     2  0.7139     0.6661 0.196 0.804
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-1-a').click(function(){
  $('#tab-SD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-2'>
<p><a id='tab-SD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.3412     0.8079 0.876 0.000 0.124
#&gt; SRR1812753     1  0.3412     0.8079 0.876 0.000 0.124
#&gt; SRR1812751     1  0.2066     0.9087 0.940 0.060 0.000
#&gt; SRR1812750     1  0.2066     0.9087 0.940 0.060 0.000
#&gt; SRR1812748     3  0.1163     0.6983 0.028 0.000 0.972
#&gt; SRR1812749     1  0.2066     0.9087 0.940 0.060 0.000
#&gt; SRR1812746     3  0.3816     0.6023 0.148 0.000 0.852
#&gt; SRR1812745     3  0.0592     0.7013 0.012 0.000 0.988
#&gt; SRR1812747     2  0.5137     0.8027 0.104 0.832 0.064
#&gt; SRR1812744     3  0.6195     0.5647 0.020 0.276 0.704
#&gt; SRR1812743     2  0.8045     0.0631 0.064 0.504 0.432
#&gt; SRR1812742     3  0.7015     0.5563 0.064 0.240 0.696
#&gt; SRR1812737     2  0.2796     0.8153 0.092 0.908 0.000
#&gt; SRR1812735     2  0.2796     0.8153 0.092 0.908 0.000
#&gt; SRR1812734     3  0.0747     0.7010 0.016 0.000 0.984
#&gt; SRR1812733     3  0.6688     0.4503 0.012 0.408 0.580
#&gt; SRR1812736     3  0.1163     0.6983 0.028 0.000 0.972
#&gt; SRR1812732     2  0.7029     0.1109 0.020 0.540 0.440
#&gt; SRR1812730     3  0.4805     0.6880 0.012 0.176 0.812
#&gt; SRR1812731     2  0.3369     0.7688 0.040 0.908 0.052
#&gt; SRR1812729     2  0.4137     0.8162 0.096 0.872 0.032
#&gt; SRR1812727     3  0.5171     0.5885 0.204 0.012 0.784
#&gt; SRR1812726     2  0.4446     0.8132 0.112 0.856 0.032
#&gt; SRR1812728     2  0.7278    -0.1744 0.028 0.516 0.456
#&gt; SRR1812724     2  0.3237     0.7651 0.032 0.912 0.056
#&gt; SRR1812725     2  0.6387     0.3899 0.020 0.680 0.300
#&gt; SRR1812723     2  0.5137     0.7999 0.104 0.832 0.064
#&gt; SRR1812722     2  0.4446     0.8132 0.112 0.856 0.032
#&gt; SRR1812721     2  0.2229     0.7880 0.044 0.944 0.012
#&gt; SRR1812718     2  0.5137     0.7999 0.104 0.832 0.064
#&gt; SRR1812717     2  0.0000     0.8058 0.000 1.000 0.000
#&gt; SRR1812716     3  0.7069     0.4431 0.024 0.408 0.568
#&gt; SRR1812715     2  0.2796     0.8153 0.092 0.908 0.000
#&gt; SRR1812714     2  0.4196     0.8142 0.112 0.864 0.024
#&gt; SRR1812719     3  0.5138     0.6722 0.120 0.052 0.828
#&gt; SRR1812713     2  0.0475     0.8034 0.004 0.992 0.004
#&gt; SRR1812712     2  0.0475     0.8034 0.004 0.992 0.004
#&gt; SRR1812711     2  0.4892     0.8083 0.112 0.840 0.048
#&gt; SRR1812710     2  0.2796     0.8153 0.092 0.908 0.000
#&gt; SRR1812709     2  0.0237     0.8047 0.004 0.996 0.000
#&gt; SRR1812708     1  0.2261     0.9028 0.932 0.068 0.000
#&gt; SRR1812707     2  0.2796     0.8153 0.092 0.908 0.000
#&gt; SRR1812705     2  0.5304     0.7979 0.108 0.824 0.068
#&gt; SRR1812706     3  0.7192     0.4311 0.028 0.412 0.560
#&gt; SRR1812704     2  0.3213     0.7585 0.008 0.900 0.092
#&gt; SRR1812703     2  0.4526     0.8116 0.104 0.856 0.040
#&gt; SRR1812702     3  0.6777     0.5275 0.020 0.364 0.616
#&gt; SRR1812741     2  0.7953     0.2236 0.068 0.564 0.368
#&gt; SRR1812740     3  0.1163     0.6983 0.028 0.000 0.972
#&gt; SRR1812739     2  0.2229     0.7854 0.012 0.944 0.044
#&gt; SRR1812738     2  0.4953     0.6421 0.016 0.808 0.176
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-2-a').click(function(){
  $('#tab-SD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-3'>
<p><a id='tab-SD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.1406     0.9039 0.960 0.000 0.024 0.016
#&gt; SRR1812753     1  0.1406     0.9039 0.960 0.000 0.024 0.016
#&gt; SRR1812751     1  0.1743     0.9396 0.940 0.056 0.000 0.004
#&gt; SRR1812750     1  0.1743     0.9396 0.940 0.056 0.000 0.004
#&gt; SRR1812748     3  0.4957     0.5744 0.012 0.000 0.668 0.320
#&gt; SRR1812749     1  0.1743     0.9396 0.940 0.056 0.000 0.004
#&gt; SRR1812746     3  0.6351     0.5150 0.080 0.000 0.588 0.332
#&gt; SRR1812745     3  0.5038     0.5650 0.012 0.000 0.652 0.336
#&gt; SRR1812747     2  0.5093     0.3787 0.012 0.640 0.000 0.348
#&gt; SRR1812744     3  0.6478     0.1415 0.000 0.088 0.576 0.336
#&gt; SRR1812743     3  0.7281    -0.0771 0.048 0.448 0.456 0.048
#&gt; SRR1812742     3  0.6573     0.3852 0.048 0.180 0.692 0.080
#&gt; SRR1812737     2  0.1211     0.5579 0.000 0.960 0.040 0.000
#&gt; SRR1812735     2  0.1902     0.5407 0.004 0.932 0.000 0.064
#&gt; SRR1812734     3  0.4978     0.5709 0.012 0.000 0.664 0.324
#&gt; SRR1812733     4  0.3501     0.7384 0.000 0.132 0.020 0.848
#&gt; SRR1812736     3  0.4978     0.5737 0.012 0.000 0.664 0.324
#&gt; SRR1812732     3  0.6483     0.1586 0.000 0.312 0.592 0.096
#&gt; SRR1812730     4  0.2441     0.6544 0.004 0.012 0.068 0.916
#&gt; SRR1812731     2  0.6637     0.2681 0.000 0.572 0.324 0.104
#&gt; SRR1812729     2  0.5040     0.3602 0.008 0.628 0.000 0.364
#&gt; SRR1812727     4  0.5923     0.3311 0.128 0.000 0.176 0.696
#&gt; SRR1812726     2  0.5093     0.3787 0.012 0.640 0.000 0.348
#&gt; SRR1812728     4  0.2530     0.7545 0.000 0.100 0.004 0.896
#&gt; SRR1812724     2  0.7038     0.2452 0.004 0.548 0.324 0.124
#&gt; SRR1812725     4  0.4382     0.4160 0.000 0.296 0.000 0.704
#&gt; SRR1812723     2  0.5300     0.2848 0.012 0.580 0.000 0.408
#&gt; SRR1812722     2  0.5038     0.3874 0.012 0.652 0.000 0.336
#&gt; SRR1812721     2  0.6820     0.3165 0.048 0.620 0.284 0.048
#&gt; SRR1812718     2  0.5279     0.2993 0.012 0.588 0.000 0.400
#&gt; SRR1812717     2  0.2844     0.5434 0.000 0.900 0.052 0.048
#&gt; SRR1812716     4  0.2542     0.7623 0.000 0.084 0.012 0.904
#&gt; SRR1812715     2  0.0188     0.5581 0.004 0.996 0.000 0.000
#&gt; SRR1812714     2  0.5285     0.3737 0.012 0.632 0.004 0.352
#&gt; SRR1812719     4  0.4595     0.4882 0.044 0.000 0.176 0.780
#&gt; SRR1812713     2  0.5302     0.4822 0.004 0.752 0.080 0.164
#&gt; SRR1812712     2  0.5736     0.4497 0.004 0.708 0.080 0.208
#&gt; SRR1812711     2  0.5189     0.3454 0.012 0.616 0.000 0.372
#&gt; SRR1812710     2  0.0188     0.5581 0.004 0.996 0.000 0.000
#&gt; SRR1812709     2  0.4219     0.5162 0.004 0.832 0.088 0.076
#&gt; SRR1812708     1  0.3791     0.8795 0.860 0.056 0.008 0.076
#&gt; SRR1812707     2  0.0921     0.5588 0.000 0.972 0.028 0.000
#&gt; SRR1812705     2  0.5244     0.3214 0.012 0.600 0.000 0.388
#&gt; SRR1812706     4  0.2708     0.7624 0.004 0.076 0.016 0.904
#&gt; SRR1812704     4  0.6446     0.2679 0.000 0.328 0.088 0.584
#&gt; SRR1812703     2  0.5553     0.1831 0.012 0.532 0.004 0.452
#&gt; SRR1812702     4  0.2473     0.7619 0.000 0.080 0.012 0.908
#&gt; SRR1812741     2  0.7496     0.1340 0.048 0.512 0.372 0.068
#&gt; SRR1812740     3  0.4978     0.5737 0.012 0.000 0.664 0.324
#&gt; SRR1812739     2  0.5995     0.3786 0.000 0.660 0.256 0.084
#&gt; SRR1812738     2  0.7671     0.1185 0.000 0.456 0.300 0.244
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-3-a').click(function(){
  $('#tab-SD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-4'>
<p><a id='tab-SD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.2162      0.916 0.916 0.000 0.008 0.012 0.064
#&gt; SRR1812753     1  0.2162      0.916 0.916 0.000 0.008 0.012 0.064
#&gt; SRR1812751     1  0.0510      0.934 0.984 0.016 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0671      0.934 0.980 0.016 0.000 0.000 0.004
#&gt; SRR1812748     3  0.1202      0.951 0.004 0.000 0.960 0.032 0.004
#&gt; SRR1812749     1  0.0671      0.934 0.980 0.016 0.000 0.000 0.004
#&gt; SRR1812746     3  0.2396      0.909 0.024 0.000 0.904 0.004 0.068
#&gt; SRR1812745     3  0.0566      0.957 0.000 0.000 0.984 0.004 0.012
#&gt; SRR1812747     2  0.2074      0.713 0.000 0.896 0.000 0.000 0.104
#&gt; SRR1812744     4  0.6581      0.528 0.000 0.028 0.148 0.560 0.264
#&gt; SRR1812743     4  0.2749      0.789 0.012 0.008 0.032 0.900 0.048
#&gt; SRR1812742     4  0.5436      0.578 0.012 0.008 0.240 0.676 0.064
#&gt; SRR1812737     2  0.3980      0.611 0.000 0.708 0.000 0.284 0.008
#&gt; SRR1812735     2  0.2806      0.703 0.000 0.844 0.000 0.152 0.004
#&gt; SRR1812734     3  0.1124      0.948 0.000 0.000 0.960 0.004 0.036
#&gt; SRR1812733     5  0.5811      0.824 0.000 0.144 0.076 0.084 0.696
#&gt; SRR1812736     3  0.0613      0.958 0.004 0.000 0.984 0.008 0.004
#&gt; SRR1812732     4  0.3755      0.722 0.000 0.012 0.140 0.816 0.032
#&gt; SRR1812730     5  0.5077      0.856 0.000 0.156 0.108 0.012 0.724
#&gt; SRR1812731     4  0.1443      0.803 0.004 0.044 0.000 0.948 0.004
#&gt; SRR1812729     2  0.1544      0.731 0.000 0.932 0.000 0.000 0.068
#&gt; SRR1812727     5  0.4418      0.644 0.048 0.012 0.148 0.008 0.784
#&gt; SRR1812726     2  0.1638      0.729 0.000 0.932 0.000 0.004 0.064
#&gt; SRR1812728     5  0.4784      0.863 0.000 0.192 0.056 0.016 0.736
#&gt; SRR1812724     4  0.1907      0.803 0.000 0.044 0.000 0.928 0.028
#&gt; SRR1812725     5  0.4130      0.762 0.000 0.292 0.000 0.012 0.696
#&gt; SRR1812723     2  0.2127      0.707 0.000 0.892 0.000 0.000 0.108
#&gt; SRR1812722     2  0.1877      0.731 0.000 0.924 0.000 0.012 0.064
#&gt; SRR1812721     4  0.4074      0.683 0.012 0.180 0.000 0.780 0.028
#&gt; SRR1812718     2  0.2230      0.703 0.000 0.884 0.000 0.000 0.116
#&gt; SRR1812717     2  0.4465      0.572 0.000 0.672 0.000 0.304 0.024
#&gt; SRR1812716     5  0.5027      0.864 0.000 0.184 0.084 0.012 0.720
#&gt; SRR1812715     2  0.3353      0.679 0.000 0.796 0.000 0.196 0.008
#&gt; SRR1812714     2  0.2563      0.720 0.000 0.872 0.000 0.008 0.120
#&gt; SRR1812719     5  0.3435      0.693 0.004 0.012 0.148 0.008 0.828
#&gt; SRR1812713     2  0.6469      0.302 0.000 0.468 0.000 0.336 0.196
#&gt; SRR1812712     2  0.6399      0.339 0.000 0.492 0.000 0.316 0.192
#&gt; SRR1812711     2  0.1792      0.719 0.000 0.916 0.000 0.000 0.084
#&gt; SRR1812710     2  0.3353      0.679 0.000 0.796 0.000 0.196 0.008
#&gt; SRR1812709     2  0.5435      0.263 0.000 0.512 0.000 0.428 0.060
#&gt; SRR1812708     1  0.4084      0.800 0.788 0.024 0.008 0.008 0.172
#&gt; SRR1812707     2  0.3835      0.632 0.000 0.732 0.000 0.260 0.008
#&gt; SRR1812705     2  0.2439      0.701 0.000 0.876 0.000 0.004 0.120
#&gt; SRR1812706     5  0.4903      0.862 0.000 0.196 0.068 0.012 0.724
#&gt; SRR1812704     5  0.4879      0.739 0.000 0.156 0.000 0.124 0.720
#&gt; SRR1812703     2  0.3642      0.620 0.000 0.760 0.000 0.008 0.232
#&gt; SRR1812702     5  0.5114      0.863 0.000 0.176 0.096 0.012 0.716
#&gt; SRR1812741     4  0.1903      0.804 0.012 0.020 0.008 0.940 0.020
#&gt; SRR1812740     3  0.1116      0.953 0.004 0.000 0.964 0.028 0.004
#&gt; SRR1812739     4  0.4059      0.658 0.000 0.172 0.000 0.776 0.052
#&gt; SRR1812738     4  0.3602      0.718 0.000 0.024 0.000 0.796 0.180
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-4-a').click(function(){
  $('#tab-SD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-kmeans-get-classes-5'>
<p><a id='tab-SD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.2213     0.8632 0.888 0.000 0.008 0.000 0.004 0.100
#&gt; SRR1812753     1  0.2213     0.8632 0.888 0.000 0.008 0.000 0.004 0.100
#&gt; SRR1812751     1  0.0260     0.8888 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0260     0.8888 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0951     0.9292 0.000 0.000 0.968 0.004 0.008 0.020
#&gt; SRR1812749     1  0.0260     0.8888 0.992 0.008 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.2930     0.8792 0.000 0.000 0.840 0.000 0.036 0.124
#&gt; SRR1812745     3  0.1633     0.9290 0.000 0.000 0.932 0.000 0.024 0.044
#&gt; SRR1812747     2  0.2772     0.7756 0.000 0.816 0.000 0.000 0.180 0.004
#&gt; SRR1812744     4  0.6516    -0.1420 0.000 0.016 0.052 0.436 0.092 0.404
#&gt; SRR1812743     6  0.4165     0.5835 0.008 0.004 0.000 0.420 0.000 0.568
#&gt; SRR1812742     6  0.6340     0.6448 0.008 0.000 0.160 0.276 0.032 0.524
#&gt; SRR1812737     2  0.3955     0.3337 0.000 0.608 0.000 0.384 0.008 0.000
#&gt; SRR1812735     2  0.2558     0.6901 0.000 0.840 0.000 0.156 0.004 0.000
#&gt; SRR1812734     3  0.2250     0.9091 0.000 0.000 0.888 0.000 0.020 0.092
#&gt; SRR1812733     5  0.4761     0.6377 0.000 0.036 0.028 0.220 0.704 0.012
#&gt; SRR1812736     3  0.0508     0.9341 0.000 0.000 0.984 0.000 0.012 0.004
#&gt; SRR1812732     4  0.4730    -0.3464 0.000 0.000 0.040 0.588 0.008 0.364
#&gt; SRR1812730     5  0.2186     0.8471 0.000 0.036 0.048 0.000 0.908 0.008
#&gt; SRR1812731     4  0.3606    -0.0275 0.000 0.004 0.000 0.724 0.008 0.264
#&gt; SRR1812729     2  0.2404     0.7964 0.000 0.872 0.000 0.016 0.112 0.000
#&gt; SRR1812727     5  0.5306     0.5947 0.016 0.008 0.060 0.008 0.640 0.268
#&gt; SRR1812726     2  0.1610     0.7939 0.000 0.916 0.000 0.000 0.084 0.000
#&gt; SRR1812728     5  0.2319     0.8412 0.000 0.060 0.008 0.008 0.904 0.020
#&gt; SRR1812724     4  0.2851     0.2425 0.000 0.004 0.000 0.844 0.020 0.132
#&gt; SRR1812725     5  0.2146     0.8086 0.000 0.116 0.000 0.004 0.880 0.000
#&gt; SRR1812723     2  0.2730     0.7699 0.000 0.808 0.000 0.000 0.192 0.000
#&gt; SRR1812722     2  0.2039     0.7935 0.000 0.904 0.000 0.020 0.076 0.000
#&gt; SRR1812721     4  0.5457     0.1373 0.004 0.140 0.000 0.624 0.012 0.220
#&gt; SRR1812718     2  0.2730     0.7699 0.000 0.808 0.000 0.000 0.192 0.000
#&gt; SRR1812717     4  0.3923     0.1455 0.000 0.416 0.000 0.580 0.004 0.000
#&gt; SRR1812716     5  0.2007     0.8482 0.000 0.036 0.044 0.000 0.916 0.004
#&gt; SRR1812715     2  0.2772     0.6713 0.000 0.816 0.000 0.180 0.004 0.000
#&gt; SRR1812714     2  0.2468     0.7613 0.000 0.888 0.000 0.004 0.048 0.060
#&gt; SRR1812719     5  0.4758     0.6325 0.000 0.008 0.060 0.008 0.680 0.244
#&gt; SRR1812713     4  0.5022     0.4321 0.000 0.204 0.000 0.640 0.156 0.000
#&gt; SRR1812712     4  0.5126     0.4282 0.000 0.216 0.000 0.624 0.160 0.000
#&gt; SRR1812711     2  0.1957     0.7922 0.000 0.888 0.000 0.000 0.112 0.000
#&gt; SRR1812710     2  0.3052     0.6356 0.000 0.780 0.000 0.216 0.004 0.000
#&gt; SRR1812709     4  0.4305     0.4349 0.000 0.260 0.000 0.684 0.056 0.000
#&gt; SRR1812708     1  0.5762     0.5834 0.616 0.056 0.004 0.012 0.048 0.264
#&gt; SRR1812707     2  0.3945     0.3442 0.000 0.612 0.000 0.380 0.008 0.000
#&gt; SRR1812705     2  0.2871     0.7705 0.000 0.804 0.000 0.004 0.192 0.000
#&gt; SRR1812706     5  0.2570     0.8429 0.000 0.064 0.024 0.024 0.888 0.000
#&gt; SRR1812704     5  0.2881     0.8080 0.000 0.040 0.000 0.084 0.864 0.012
#&gt; SRR1812703     2  0.3709     0.7181 0.000 0.808 0.000 0.016 0.100 0.076
#&gt; SRR1812702     5  0.2007     0.8482 0.000 0.036 0.044 0.004 0.916 0.000
#&gt; SRR1812741     4  0.4268    -0.4906 0.000 0.004 0.000 0.556 0.012 0.428
#&gt; SRR1812740     3  0.0964     0.9304 0.000 0.000 0.968 0.004 0.012 0.016
#&gt; SRR1812739     4  0.4235     0.3772 0.000 0.064 0.000 0.780 0.052 0.104
#&gt; SRR1812738     4  0.4313     0.2735 0.000 0.000 0.000 0.728 0.124 0.148
</code></pre>

<script>
$('#tab-SD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-kmeans-get-classes-5-a').click(function(){
  $('#tab-SD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-kmeans-signature_compare](figure_cola/SD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-kmeans-collect-classes](figure_cola/SD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "skmeans"]
# you can also extract it by
# res = res_list["SD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-skmeans-collect-plots](figure_cola/SD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-skmeans-select-partition-number](figure_cola/SD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.546           0.774       0.893         0.5006 0.490   0.490
#> 3 3 0.541           0.725       0.847         0.3137 0.791   0.604
#> 4 4 0.660           0.723       0.855         0.1394 0.795   0.490
#> 5 5 0.711           0.731       0.839         0.0702 0.892   0.605
#> 6 6 0.730           0.647       0.804         0.0407 0.955   0.776
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-classes'>
<ul>
<li><a href='#tab-SD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-skmeans-get-classes-1'>
<p><a id='tab-SD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000      0.816 1.000 0.000
#&gt; SRR1812753     1   0.000      0.816 1.000 0.000
#&gt; SRR1812751     2   0.985      0.338 0.428 0.572
#&gt; SRR1812750     2   0.985      0.338 0.428 0.572
#&gt; SRR1812748     1   0.000      0.816 1.000 0.000
#&gt; SRR1812749     2   0.985      0.338 0.428 0.572
#&gt; SRR1812746     1   0.000      0.816 1.000 0.000
#&gt; SRR1812745     1   0.000      0.816 1.000 0.000
#&gt; SRR1812747     2   0.000      0.914 0.000 1.000
#&gt; SRR1812744     1   0.000      0.816 1.000 0.000
#&gt; SRR1812743     1   0.644      0.754 0.836 0.164
#&gt; SRR1812742     1   0.000      0.816 1.000 0.000
#&gt; SRR1812737     2   0.000      0.914 0.000 1.000
#&gt; SRR1812735     2   0.000      0.914 0.000 1.000
#&gt; SRR1812734     1   0.000      0.816 1.000 0.000
#&gt; SRR1812733     1   0.985      0.508 0.572 0.428
#&gt; SRR1812736     1   0.000      0.816 1.000 0.000
#&gt; SRR1812732     1   0.295      0.799 0.948 0.052
#&gt; SRR1812730     1   0.595      0.767 0.856 0.144
#&gt; SRR1812731     2   0.000      0.914 0.000 1.000
#&gt; SRR1812729     2   0.000      0.914 0.000 1.000
#&gt; SRR1812727     1   0.000      0.816 1.000 0.000
#&gt; SRR1812726     2   0.000      0.914 0.000 1.000
#&gt; SRR1812728     1   0.921      0.622 0.664 0.336
#&gt; SRR1812724     1   0.985      0.508 0.572 0.428
#&gt; SRR1812725     1   0.985      0.508 0.572 0.428
#&gt; SRR1812723     2   0.000      0.914 0.000 1.000
#&gt; SRR1812722     2   0.000      0.914 0.000 1.000
#&gt; SRR1812721     2   0.000      0.914 0.000 1.000
#&gt; SRR1812718     2   0.000      0.914 0.000 1.000
#&gt; SRR1812717     2   0.000      0.914 0.000 1.000
#&gt; SRR1812716     1   0.932      0.609 0.652 0.348
#&gt; SRR1812715     2   0.000      0.914 0.000 1.000
#&gt; SRR1812714     2   0.000      0.914 0.000 1.000
#&gt; SRR1812719     1   0.000      0.816 1.000 0.000
#&gt; SRR1812713     2   0.000      0.914 0.000 1.000
#&gt; SRR1812712     2   0.000      0.914 0.000 1.000
#&gt; SRR1812711     2   0.000      0.914 0.000 1.000
#&gt; SRR1812710     2   0.000      0.914 0.000 1.000
#&gt; SRR1812709     2   0.000      0.914 0.000 1.000
#&gt; SRR1812708     2   0.985      0.338 0.428 0.572
#&gt; SRR1812707     2   0.000      0.914 0.000 1.000
#&gt; SRR1812705     2   0.000      0.914 0.000 1.000
#&gt; SRR1812706     1   0.952      0.583 0.628 0.372
#&gt; SRR1812704     1   0.985      0.508 0.572 0.428
#&gt; SRR1812703     2   0.000      0.914 0.000 1.000
#&gt; SRR1812702     1   0.706      0.735 0.808 0.192
#&gt; SRR1812741     1   0.000      0.816 1.000 0.000
#&gt; SRR1812740     1   0.000      0.816 1.000 0.000
#&gt; SRR1812739     2   0.000      0.914 0.000 1.000
#&gt; SRR1812738     1   0.985      0.508 0.572 0.428
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-1-a').click(function(){
  $('#tab-SD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-2'>
<p><a id='tab-SD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.2537      0.791 0.920 0.000 0.080
#&gt; SRR1812753     1  0.2537      0.791 0.920 0.000 0.080
#&gt; SRR1812751     1  0.2711      0.805 0.912 0.088 0.000
#&gt; SRR1812750     1  0.2711      0.805 0.912 0.088 0.000
#&gt; SRR1812748     3  0.5403      0.704 0.124 0.060 0.816
#&gt; SRR1812749     1  0.2711      0.805 0.912 0.088 0.000
#&gt; SRR1812746     1  0.6295      0.139 0.528 0.000 0.472
#&gt; SRR1812745     3  0.2066      0.766 0.060 0.000 0.940
#&gt; SRR1812747     2  0.5024      0.773 0.004 0.776 0.220
#&gt; SRR1812744     3  0.6111      0.383 0.396 0.000 0.604
#&gt; SRR1812743     3  0.9794      0.198 0.380 0.236 0.384
#&gt; SRR1812742     3  0.6111      0.388 0.396 0.000 0.604
#&gt; SRR1812737     2  0.1163      0.849 0.028 0.972 0.000
#&gt; SRR1812735     2  0.1031      0.852 0.000 0.976 0.024
#&gt; SRR1812734     3  0.4974      0.599 0.236 0.000 0.764
#&gt; SRR1812733     3  0.1643      0.777 0.000 0.044 0.956
#&gt; SRR1812736     3  0.2165      0.765 0.064 0.000 0.936
#&gt; SRR1812732     3  0.9263      0.449 0.220 0.252 0.528
#&gt; SRR1812730     3  0.0237      0.778 0.004 0.000 0.996
#&gt; SRR1812731     2  0.5414      0.721 0.212 0.772 0.016
#&gt; SRR1812729     2  0.2796      0.844 0.000 0.908 0.092
#&gt; SRR1812727     1  0.4121      0.735 0.832 0.000 0.168
#&gt; SRR1812726     2  0.3500      0.836 0.004 0.880 0.116
#&gt; SRR1812728     3  0.0661      0.777 0.008 0.004 0.988
#&gt; SRR1812724     2  0.7001      0.606 0.084 0.716 0.200
#&gt; SRR1812725     3  0.0892      0.771 0.000 0.020 0.980
#&gt; SRR1812723     2  0.5024      0.773 0.004 0.776 0.220
#&gt; SRR1812722     2  0.3644      0.833 0.004 0.872 0.124
#&gt; SRR1812721     2  0.5292      0.711 0.228 0.764 0.008
#&gt; SRR1812718     2  0.5024      0.773 0.004 0.776 0.220
#&gt; SRR1812717     2  0.2165      0.840 0.064 0.936 0.000
#&gt; SRR1812716     3  0.0475      0.778 0.004 0.004 0.992
#&gt; SRR1812715     2  0.0000      0.851 0.000 1.000 0.000
#&gt; SRR1812714     2  0.4527      0.812 0.088 0.860 0.052
#&gt; SRR1812719     1  0.5706      0.529 0.680 0.000 0.320
#&gt; SRR1812713     2  0.2866      0.834 0.076 0.916 0.008
#&gt; SRR1812712     2  0.2866      0.834 0.076 0.916 0.008
#&gt; SRR1812711     2  0.4978      0.777 0.004 0.780 0.216
#&gt; SRR1812710     2  0.0000      0.851 0.000 1.000 0.000
#&gt; SRR1812709     2  0.2866      0.834 0.076 0.916 0.008
#&gt; SRR1812708     1  0.2796      0.803 0.908 0.092 0.000
#&gt; SRR1812707     2  0.1163      0.849 0.028 0.972 0.000
#&gt; SRR1812705     2  0.5070      0.769 0.004 0.772 0.224
#&gt; SRR1812706     3  0.0475      0.778 0.004 0.004 0.992
#&gt; SRR1812704     3  0.6678      0.639 0.064 0.208 0.728
#&gt; SRR1812703     2  0.4821      0.809 0.088 0.848 0.064
#&gt; SRR1812702     3  0.0475      0.778 0.004 0.004 0.992
#&gt; SRR1812741     1  0.3983      0.631 0.852 0.144 0.004
#&gt; SRR1812740     3  0.2443      0.776 0.028 0.032 0.940
#&gt; SRR1812739     2  0.3461      0.827 0.076 0.900 0.024
#&gt; SRR1812738     3  0.7458      0.590 0.088 0.236 0.676
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-2-a').click(function(){
  $('#tab-SD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-3'>
<p><a id='tab-SD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     0.9760 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.9760 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0188     0.9767 0.996 0.004 0.000 0.000
#&gt; SRR1812750     1  0.0188     0.9767 0.996 0.004 0.000 0.000
#&gt; SRR1812748     3  0.1824     0.7593 0.004 0.000 0.936 0.060
#&gt; SRR1812749     1  0.0188     0.9767 0.996 0.004 0.000 0.000
#&gt; SRR1812746     3  0.5039     0.3291 0.404 0.000 0.592 0.004
#&gt; SRR1812745     3  0.1398     0.7644 0.004 0.000 0.956 0.040
#&gt; SRR1812747     2  0.0000     0.8527 0.000 1.000 0.000 0.000
#&gt; SRR1812744     3  0.5464     0.5696 0.064 0.000 0.708 0.228
#&gt; SRR1812743     4  0.3450     0.6815 0.008 0.000 0.156 0.836
#&gt; SRR1812742     3  0.6058     0.3813 0.060 0.000 0.604 0.336
#&gt; SRR1812737     4  0.4994     0.0276 0.000 0.480 0.000 0.520
#&gt; SRR1812735     2  0.3486     0.7201 0.000 0.812 0.000 0.188
#&gt; SRR1812734     3  0.1938     0.7611 0.012 0.000 0.936 0.052
#&gt; SRR1812733     3  0.2174     0.7615 0.000 0.020 0.928 0.052
#&gt; SRR1812736     3  0.1661     0.7626 0.004 0.000 0.944 0.052
#&gt; SRR1812732     4  0.4456     0.5205 0.004 0.000 0.280 0.716
#&gt; SRR1812730     3  0.4595     0.7534 0.000 0.184 0.776 0.040
#&gt; SRR1812731     4  0.1854     0.7738 0.008 0.024 0.020 0.948
#&gt; SRR1812729     2  0.1792     0.8325 0.000 0.932 0.000 0.068
#&gt; SRR1812727     1  0.0336     0.9722 0.992 0.000 0.008 0.000
#&gt; SRR1812726     2  0.0469     0.8553 0.000 0.988 0.000 0.012
#&gt; SRR1812728     3  0.5572     0.7029 0.008 0.260 0.692 0.040
#&gt; SRR1812724     4  0.0188     0.7726 0.000 0.000 0.004 0.996
#&gt; SRR1812725     3  0.5695     0.6120 0.000 0.336 0.624 0.040
#&gt; SRR1812723     2  0.0336     0.8487 0.000 0.992 0.008 0.000
#&gt; SRR1812722     2  0.0592     0.8552 0.000 0.984 0.000 0.016
#&gt; SRR1812721     4  0.1890     0.7685 0.008 0.056 0.000 0.936
#&gt; SRR1812718     2  0.0188     0.8510 0.000 0.996 0.004 0.000
#&gt; SRR1812717     4  0.4830     0.3250 0.000 0.392 0.000 0.608
#&gt; SRR1812716     3  0.4868     0.7423 0.000 0.212 0.748 0.040
#&gt; SRR1812715     2  0.4331     0.5780 0.000 0.712 0.000 0.288
#&gt; SRR1812714     2  0.2214     0.8337 0.044 0.928 0.000 0.028
#&gt; SRR1812719     1  0.2714     0.8511 0.884 0.000 0.112 0.004
#&gt; SRR1812713     4  0.4553     0.6710 0.000 0.180 0.040 0.780
#&gt; SRR1812712     4  0.4553     0.6710 0.000 0.180 0.040 0.780
#&gt; SRR1812711     2  0.0336     0.8550 0.000 0.992 0.000 0.008
#&gt; SRR1812710     2  0.4331     0.5780 0.000 0.712 0.000 0.288
#&gt; SRR1812709     4  0.3306     0.7148 0.000 0.156 0.004 0.840
#&gt; SRR1812708     1  0.0336     0.9739 0.992 0.008 0.000 0.000
#&gt; SRR1812707     2  0.4999    -0.0466 0.000 0.508 0.000 0.492
#&gt; SRR1812705     2  0.0336     0.8487 0.000 0.992 0.008 0.000
#&gt; SRR1812706     3  0.5090     0.7310 0.000 0.228 0.728 0.044
#&gt; SRR1812704     4  0.4964     0.5482 0.000 0.028 0.256 0.716
#&gt; SRR1812703     2  0.1598     0.8497 0.020 0.956 0.004 0.020
#&gt; SRR1812702     3  0.4755     0.7490 0.000 0.200 0.760 0.040
#&gt; SRR1812741     4  0.3638     0.7115 0.120 0.000 0.032 0.848
#&gt; SRR1812740     3  0.1661     0.7626 0.004 0.000 0.944 0.052
#&gt; SRR1812739     4  0.1706     0.7749 0.000 0.036 0.016 0.948
#&gt; SRR1812738     4  0.1389     0.7667 0.000 0.000 0.048 0.952
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-3-a').click(function(){
  $('#tab-SD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-4'>
<p><a id='tab-SD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000      0.966 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.966 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.966 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.966 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.3109      0.742 0.000 0.000 0.800 0.000 0.200
#&gt; SRR1812749     1  0.0000      0.966 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.6394      0.370 0.344 0.000 0.476 0.000 0.180
#&gt; SRR1812745     3  0.3395      0.729 0.000 0.000 0.764 0.000 0.236
#&gt; SRR1812747     2  0.2813      0.837 0.000 0.832 0.000 0.000 0.168
#&gt; SRR1812744     3  0.1369      0.716 0.008 0.008 0.956 0.000 0.028
#&gt; SRR1812743     3  0.4331      0.122 0.000 0.004 0.596 0.400 0.000
#&gt; SRR1812742     3  0.3607      0.623 0.008 0.000 0.820 0.144 0.028
#&gt; SRR1812737     4  0.4268      0.223 0.000 0.444 0.000 0.556 0.000
#&gt; SRR1812735     2  0.2806      0.764 0.000 0.844 0.000 0.152 0.004
#&gt; SRR1812734     3  0.3489      0.741 0.004 0.004 0.784 0.000 0.208
#&gt; SRR1812733     5  0.3281      0.778 0.000 0.000 0.092 0.060 0.848
#&gt; SRR1812736     3  0.3336      0.733 0.000 0.000 0.772 0.000 0.228
#&gt; SRR1812732     3  0.1965      0.660 0.000 0.000 0.904 0.096 0.000
#&gt; SRR1812730     5  0.0609      0.860 0.000 0.000 0.020 0.000 0.980
#&gt; SRR1812731     4  0.3160      0.669 0.000 0.004 0.188 0.808 0.000
#&gt; SRR1812729     2  0.1661      0.854 0.000 0.940 0.000 0.036 0.024
#&gt; SRR1812727     1  0.0968      0.953 0.972 0.012 0.004 0.000 0.012
#&gt; SRR1812726     2  0.0963      0.863 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812728     5  0.2499      0.857 0.004 0.052 0.008 0.028 0.908
#&gt; SRR1812724     4  0.1956      0.708 0.000 0.008 0.076 0.916 0.000
#&gt; SRR1812725     5  0.2124      0.829 0.000 0.096 0.000 0.004 0.900
#&gt; SRR1812723     2  0.2471      0.846 0.000 0.864 0.000 0.000 0.136
#&gt; SRR1812722     2  0.0963      0.863 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812721     4  0.2514      0.721 0.000 0.044 0.060 0.896 0.000
#&gt; SRR1812718     2  0.2690      0.841 0.000 0.844 0.000 0.000 0.156
#&gt; SRR1812717     4  0.3796      0.524 0.000 0.300 0.000 0.700 0.000
#&gt; SRR1812716     5  0.0693      0.866 0.000 0.008 0.012 0.000 0.980
#&gt; SRR1812715     2  0.3612      0.597 0.000 0.732 0.000 0.268 0.000
#&gt; SRR1812714     2  0.0693      0.852 0.008 0.980 0.000 0.012 0.000
#&gt; SRR1812719     1  0.3352      0.759 0.800 0.004 0.004 0.000 0.192
#&gt; SRR1812713     4  0.3944      0.625 0.000 0.052 0.000 0.788 0.160
#&gt; SRR1812712     4  0.4010      0.622 0.000 0.056 0.000 0.784 0.160
#&gt; SRR1812711     2  0.1270      0.862 0.000 0.948 0.000 0.000 0.052
#&gt; SRR1812710     2  0.3661      0.591 0.000 0.724 0.000 0.276 0.000
#&gt; SRR1812709     4  0.1638      0.721 0.000 0.064 0.000 0.932 0.004
#&gt; SRR1812708     1  0.0324      0.963 0.992 0.004 0.004 0.000 0.000
#&gt; SRR1812707     4  0.4297      0.134 0.000 0.472 0.000 0.528 0.000
#&gt; SRR1812705     2  0.2690      0.837 0.000 0.844 0.000 0.000 0.156
#&gt; SRR1812706     5  0.2067      0.858 0.000 0.032 0.000 0.048 0.920
#&gt; SRR1812704     5  0.4807      0.466 0.000 0.020 0.008 0.340 0.632
#&gt; SRR1812703     2  0.2654      0.823 0.016 0.904 0.004 0.032 0.044
#&gt; SRR1812702     5  0.0566      0.864 0.000 0.004 0.012 0.000 0.984
#&gt; SRR1812741     4  0.5439      0.501 0.076 0.008 0.276 0.640 0.000
#&gt; SRR1812740     3  0.3395      0.729 0.000 0.000 0.764 0.000 0.236
#&gt; SRR1812739     4  0.3170      0.702 0.000 0.016 0.120 0.852 0.012
#&gt; SRR1812738     4  0.4033      0.637 0.000 0.004 0.212 0.760 0.024
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-4-a').click(function(){
  $('#tab-SD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-skmeans-get-classes-5'>
<p><a id='tab-SD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0551      0.921 0.984 0.000 0.000 0.004 0.008 0.004
#&gt; SRR1812753     1  0.0665      0.920 0.980 0.000 0.000 0.004 0.008 0.008
#&gt; SRR1812751     1  0.0000      0.923 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.923 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.1003      0.866 0.000 0.000 0.964 0.000 0.016 0.020
#&gt; SRR1812749     1  0.0000      0.923 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.3633      0.599 0.252 0.000 0.732 0.000 0.012 0.004
#&gt; SRR1812745     3  0.0937      0.863 0.000 0.000 0.960 0.000 0.040 0.000
#&gt; SRR1812747     2  0.3745      0.775 0.000 0.796 0.000 0.028 0.144 0.032
#&gt; SRR1812744     3  0.3954      0.608 0.008 0.008 0.764 0.012 0.012 0.196
#&gt; SRR1812743     6  0.3125      0.613 0.000 0.000 0.084 0.080 0.000 0.836
#&gt; SRR1812742     6  0.4490      0.356 0.000 0.000 0.372 0.008 0.024 0.596
#&gt; SRR1812737     4  0.4344      0.275 0.000 0.336 0.000 0.628 0.000 0.036
#&gt; SRR1812735     2  0.3652      0.628 0.000 0.720 0.000 0.264 0.000 0.016
#&gt; SRR1812734     3  0.0653      0.852 0.000 0.004 0.980 0.000 0.004 0.012
#&gt; SRR1812733     5  0.6557      0.454 0.000 0.000 0.264 0.156 0.508 0.072
#&gt; SRR1812736     3  0.0972      0.868 0.000 0.000 0.964 0.000 0.028 0.008
#&gt; SRR1812732     6  0.4274      0.242 0.000 0.000 0.432 0.012 0.004 0.552
#&gt; SRR1812730     5  0.2714      0.806 0.000 0.004 0.136 0.000 0.848 0.012
#&gt; SRR1812731     6  0.3482      0.325 0.000 0.000 0.000 0.316 0.000 0.684
#&gt; SRR1812729     2  0.3394      0.779 0.000 0.832 0.000 0.104 0.036 0.028
#&gt; SRR1812727     1  0.3164      0.857 0.868 0.004 0.028 0.008 0.048 0.044
#&gt; SRR1812726     2  0.1622      0.794 0.000 0.940 0.000 0.016 0.028 0.016
#&gt; SRR1812728     5  0.3195      0.790 0.000 0.048 0.044 0.004 0.860 0.044
#&gt; SRR1812724     4  0.4338     -0.034 0.000 0.000 0.000 0.492 0.020 0.488
#&gt; SRR1812725     5  0.3356      0.798 0.000 0.076 0.044 0.008 0.848 0.024
#&gt; SRR1812723     2  0.2553      0.778 0.000 0.848 0.000 0.000 0.144 0.008
#&gt; SRR1812722     2  0.2056      0.782 0.000 0.904 0.000 0.080 0.012 0.004
#&gt; SRR1812721     4  0.4599      0.222 0.000 0.016 0.000 0.556 0.016 0.412
#&gt; SRR1812718     2  0.3525      0.770 0.000 0.800 0.000 0.012 0.156 0.032
#&gt; SRR1812717     4  0.4038      0.542 0.000 0.156 0.000 0.764 0.008 0.072
#&gt; SRR1812716     5  0.2163      0.823 0.000 0.000 0.092 0.000 0.892 0.016
#&gt; SRR1812715     2  0.4493      0.460 0.000 0.612 0.000 0.344 0.000 0.044
#&gt; SRR1812714     2  0.2325      0.777 0.012 0.916 0.016 0.028 0.008 0.020
#&gt; SRR1812719     1  0.5718      0.485 0.600 0.004 0.056 0.008 0.288 0.044
#&gt; SRR1812713     4  0.2390      0.565 0.000 0.000 0.000 0.888 0.056 0.056
#&gt; SRR1812712     4  0.2046      0.568 0.000 0.000 0.000 0.908 0.060 0.032
#&gt; SRR1812711     2  0.1511      0.795 0.000 0.940 0.000 0.004 0.044 0.012
#&gt; SRR1812710     2  0.4047      0.439 0.000 0.604 0.000 0.384 0.000 0.012
#&gt; SRR1812709     4  0.2122      0.566 0.000 0.000 0.000 0.900 0.024 0.076
#&gt; SRR1812708     1  0.0291      0.921 0.992 0.004 0.000 0.000 0.000 0.004
#&gt; SRR1812707     4  0.4167      0.201 0.000 0.368 0.000 0.612 0.000 0.020
#&gt; SRR1812705     2  0.3309      0.758 0.000 0.800 0.000 0.004 0.172 0.024
#&gt; SRR1812706     5  0.3658      0.807 0.000 0.044 0.052 0.056 0.836 0.012
#&gt; SRR1812704     5  0.4581      0.558 0.000 0.000 0.004 0.256 0.672 0.068
#&gt; SRR1812703     2  0.4719      0.683 0.020 0.780 0.028 0.092 0.052 0.028
#&gt; SRR1812702     5  0.2476      0.823 0.000 0.012 0.096 0.000 0.880 0.012
#&gt; SRR1812741     6  0.3119      0.592 0.036 0.000 0.032 0.076 0.000 0.856
#&gt; SRR1812740     3  0.1049      0.867 0.000 0.000 0.960 0.000 0.032 0.008
#&gt; SRR1812739     4  0.4327      0.169 0.000 0.004 0.008 0.596 0.008 0.384
#&gt; SRR1812738     6  0.4817      0.344 0.000 0.000 0.032 0.292 0.032 0.644
</code></pre>

<script>
$('#tab-SD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-skmeans-get-classes-5-a').click(function(){
  $('#tab-SD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-SD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-skmeans-signature_compare](figure_cola/SD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-SD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-skmeans-collect-classes](figure_cola/SD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "pam"]
# you can also extract it by
# res = res_list["SD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-pam-collect-plots](figure_cola/SD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-pam-select-partition-number](figure_cola/SD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.433           0.836       0.867         0.4524 0.492   0.492
#> 3 3 0.715           0.846       0.934         0.3197 0.704   0.507
#> 4 4 0.822           0.880       0.949         0.1541 0.815   0.596
#> 5 5 0.870           0.876       0.946         0.1248 0.887   0.656
#> 6 6 0.872           0.845       0.932         0.0307 0.975   0.889
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-classes'>
<ul>
<li><a href='#tab-SD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-pam-get-classes-1'>
<p><a id='tab-SD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.722     0.7383 0.800 0.200
#&gt; SRR1812753     1   0.722     0.7383 0.800 0.200
#&gt; SRR1812751     2   0.541     0.5925 0.124 0.876
#&gt; SRR1812750     2   0.184     0.6948 0.028 0.972
#&gt; SRR1812748     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812749     1   0.855     0.7268 0.720 0.280
#&gt; SRR1812746     1   0.722     0.7383 0.800 0.200
#&gt; SRR1812745     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812747     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812744     1   0.358     0.8410 0.932 0.068
#&gt; SRR1812743     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812742     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812737     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812735     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812734     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812733     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812736     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812732     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812730     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812731     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812729     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812727     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812726     1   0.416     0.8277 0.916 0.084
#&gt; SRR1812728     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812724     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812725     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812723     1   0.402     0.8309 0.920 0.080
#&gt; SRR1812722     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812721     1   0.697     0.6818 0.812 0.188
#&gt; SRR1812718     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812717     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812716     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812715     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812714     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812719     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812713     2   0.833     0.8737 0.264 0.736
#&gt; SRR1812712     1   0.971    -0.0634 0.600 0.400
#&gt; SRR1812711     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812710     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812709     2   0.969     0.6378 0.396 0.604
#&gt; SRR1812708     1   0.855     0.7268 0.720 0.280
#&gt; SRR1812707     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812705     1   0.402     0.8309 0.920 0.080
#&gt; SRR1812706     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812704     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812703     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812702     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812741     2   0.987     0.5501 0.432 0.568
#&gt; SRR1812740     1   0.000     0.8866 1.000 0.000
#&gt; SRR1812739     2   0.722     0.9419 0.200 0.800
#&gt; SRR1812738     1   0.850     0.4907 0.724 0.276
</code></pre>

<script>
$('#tab-SD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-1-a').click(function(){
  $('#tab-SD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-2'>
<p><a id='tab-SD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1812748     3  0.2959      0.833 0.000 0.100 0.900
#&gt; SRR1812749     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1812746     3  0.2878      0.851 0.096 0.000 0.904
#&gt; SRR1812745     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812747     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812744     3  0.6168      0.125 0.000 0.412 0.588
#&gt; SRR1812743     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812742     2  0.4555      0.759 0.000 0.800 0.200
#&gt; SRR1812737     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812735     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812734     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812733     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812736     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812732     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812730     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812729     2  0.0747      0.877 0.000 0.984 0.016
#&gt; SRR1812727     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812726     2  0.6154      0.426 0.000 0.592 0.408
#&gt; SRR1812728     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812724     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812725     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812723     2  0.6180      0.407 0.000 0.584 0.416
#&gt; SRR1812722     2  0.1753      0.864 0.000 0.952 0.048
#&gt; SRR1812721     2  0.3116      0.823 0.000 0.892 0.108
#&gt; SRR1812718     2  0.4178      0.786 0.000 0.828 0.172
#&gt; SRR1812717     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812716     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812719     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812713     2  0.4887      0.668 0.000 0.772 0.228
#&gt; SRR1812712     2  0.6062      0.354 0.000 0.616 0.384
#&gt; SRR1812711     2  0.4504      0.763 0.000 0.804 0.196
#&gt; SRR1812710     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812709     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812708     1  0.0000      1.000 1.000 0.000 0.000
#&gt; SRR1812707     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812705     2  0.6154      0.426 0.000 0.592 0.408
#&gt; SRR1812706     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812704     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812703     2  0.3551      0.818 0.000 0.868 0.132
#&gt; SRR1812702     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812741     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812740     3  0.0000      0.950 0.000 0.000 1.000
#&gt; SRR1812739     2  0.0000      0.881 0.000 1.000 0.000
#&gt; SRR1812738     2  0.0892      0.875 0.000 0.980 0.020
</code></pre>

<script>
$('#tab-SD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-2-a').click(function(){
  $('#tab-SD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-3'>
<p><a id='tab-SD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1812749     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1812745     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1812747     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812744     4  0.0469      0.920  0 0.012 0.000 0.988
#&gt; SRR1812743     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812742     2  0.4701      0.729  0 0.780 0.164 0.056
#&gt; SRR1812737     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1812733     4  0.2973      0.798  0 0.000 0.144 0.856
#&gt; SRR1812736     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1812732     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812730     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812729     2  0.0592      0.902  0 0.984 0.000 0.016
#&gt; SRR1812727     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812726     4  0.0188      0.926  0 0.004 0.000 0.996
#&gt; SRR1812728     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812724     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812725     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812723     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812722     2  0.1389      0.884  0 0.952 0.000 0.048
#&gt; SRR1812721     4  0.4454      0.551  0 0.308 0.000 0.692
#&gt; SRR1812718     2  0.3311      0.785  0 0.828 0.000 0.172
#&gt; SRR1812717     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812716     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812719     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812713     2  0.0469      0.902  0 0.988 0.000 0.012
#&gt; SRR1812712     2  0.4843      0.323  0 0.604 0.000 0.396
#&gt; SRR1812711     2  0.3569      0.756  0 0.804 0.000 0.196
#&gt; SRR1812710     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812709     2  0.4072      0.647  0 0.748 0.000 0.252
#&gt; SRR1812708     1  0.0000      1.000  1 0.000 0.000 0.000
#&gt; SRR1812707     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812705     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812706     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812704     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812703     2  0.2814      0.823  0 0.868 0.000 0.132
#&gt; SRR1812702     4  0.0000      0.929  0 0.000 0.000 1.000
#&gt; SRR1812741     2  0.4356      0.575  0 0.708 0.000 0.292
#&gt; SRR1812740     3  0.0000      1.000  0 0.000 1.000 0.000
#&gt; SRR1812739     2  0.0000      0.908  0 1.000 0.000 0.000
#&gt; SRR1812738     4  0.4843      0.344  0 0.396 0.000 0.604
</code></pre>

<script>
$('#tab-SD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-3-a').click(function(){
  $('#tab-SD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-4'>
<p><a id='tab-SD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1812749     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1812745     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1812747     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812744     5  0.0404      0.926  0 0.012 0.000 0.000 0.988
#&gt; SRR1812743     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812742     2  0.4272      0.724  0 0.752 0.196 0.000 0.052
#&gt; SRR1812737     2  0.3074      0.767  0 0.804 0.000 0.196 0.000
#&gt; SRR1812735     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1812733     4  0.0510      0.870  0 0.000 0.000 0.984 0.016
#&gt; SRR1812736     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1812732     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812730     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0290      0.904  0 0.992 0.000 0.008 0.000
#&gt; SRR1812729     2  0.1331      0.889  0 0.952 0.000 0.040 0.008
#&gt; SRR1812727     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812726     5  0.0162      0.932  0 0.004 0.000 0.000 0.996
#&gt; SRR1812728     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812724     4  0.4182      0.360  0 0.400 0.000 0.600 0.000
#&gt; SRR1812725     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812723     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812722     2  0.1197      0.887  0 0.952 0.000 0.000 0.048
#&gt; SRR1812721     4  0.0000      0.875  0 0.000 0.000 1.000 0.000
#&gt; SRR1812718     2  0.2852      0.793  0 0.828 0.000 0.000 0.172
#&gt; SRR1812717     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812716     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812719     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812713     4  0.0000      0.875  0 0.000 0.000 1.000 0.000
#&gt; SRR1812712     4  0.0000      0.875  0 0.000 0.000 1.000 0.000
#&gt; SRR1812711     2  0.3074      0.768  0 0.804 0.000 0.000 0.196
#&gt; SRR1812710     2  0.0404      0.904  0 0.988 0.000 0.012 0.000
#&gt; SRR1812709     4  0.0000      0.875  0 0.000 0.000 1.000 0.000
#&gt; SRR1812708     1  0.0000      1.000  1 0.000 0.000 0.000 0.000
#&gt; SRR1812707     2  0.3109      0.763  0 0.800 0.000 0.200 0.000
#&gt; SRR1812705     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812706     4  0.3561      0.617  0 0.000 0.000 0.740 0.260
#&gt; SRR1812704     5  0.3424      0.652  0 0.000 0.000 0.240 0.760
#&gt; SRR1812703     4  0.0404      0.872  0 0.000 0.000 0.988 0.012
#&gt; SRR1812702     5  0.0000      0.935  0 0.000 0.000 0.000 1.000
#&gt; SRR1812741     2  0.3752      0.547  0 0.708 0.000 0.000 0.292
#&gt; SRR1812740     3  0.0000      1.000  0 0.000 1.000 0.000 0.000
#&gt; SRR1812739     2  0.0000      0.906  0 1.000 0.000 0.000 0.000
#&gt; SRR1812738     5  0.4171      0.354  0 0.396 0.000 0.000 0.604
</code></pre>

<script>
$('#tab-SD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-4-a').click(function(){
  $('#tab-SD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-pam-get-classes-5'>
<p><a id='tab-SD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0632      0.965  0 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1812749     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.1814      0.896  0 0.000 0.900 0.000 0.000 0.100
#&gt; SRR1812745     3  0.0000      0.968  0 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812747     2  0.1500      0.869  0 0.936 0.000 0.000 0.012 0.052
#&gt; SRR1812744     5  0.1003      0.893  0 0.020 0.000 0.000 0.964 0.016
#&gt; SRR1812743     6  0.2793      0.697  0 0.200 0.000 0.000 0.000 0.800
#&gt; SRR1812742     6  0.3332      0.649  0 0.000 0.144 0.000 0.048 0.808
#&gt; SRR1812737     2  0.2762      0.747  0 0.804 0.000 0.196 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.887  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0363      0.964  0 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1812733     4  0.0458      0.849  0 0.000 0.000 0.984 0.016 0.000
#&gt; SRR1812736     3  0.0146      0.968  0 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1812732     2  0.0000      0.887  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812730     5  0.0000      0.908  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812731     2  0.0260      0.887  0 0.992 0.000 0.008 0.000 0.000
#&gt; SRR1812729     2  0.2342      0.858  0 0.904 0.000 0.032 0.024 0.040
#&gt; SRR1812727     5  0.0632      0.901  0 0.000 0.000 0.000 0.976 0.024
#&gt; SRR1812726     5  0.1349      0.887  0 0.004 0.000 0.000 0.940 0.056
#&gt; SRR1812728     5  0.0000      0.908  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812724     4  0.3756      0.276  0 0.400 0.000 0.600 0.000 0.000
#&gt; SRR1812725     5  0.0146      0.908  0 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812723     5  0.1204      0.890  0 0.000 0.000 0.000 0.944 0.056
#&gt; SRR1812722     2  0.1285      0.870  0 0.944 0.000 0.000 0.052 0.004
#&gt; SRR1812721     4  0.0000      0.856  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812718     2  0.3254      0.776  0 0.820 0.000 0.000 0.124 0.056
#&gt; SRR1812717     2  0.0000      0.887  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812716     5  0.0146      0.908  0 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812715     2  0.0000      0.887  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.887  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812719     5  0.0547      0.903  0 0.000 0.000 0.000 0.980 0.020
#&gt; SRR1812713     4  0.0000      0.856  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812712     4  0.0000      0.856  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812711     2  0.3394      0.756  0 0.804 0.000 0.000 0.144 0.052
#&gt; SRR1812710     2  0.0363      0.886  0 0.988 0.000 0.012 0.000 0.000
#&gt; SRR1812709     4  0.0000      0.856  0 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812708     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812707     2  0.2793      0.742  0 0.800 0.000 0.200 0.000 0.000
#&gt; SRR1812705     5  0.1204      0.890  0 0.000 0.000 0.000 0.944 0.056
#&gt; SRR1812706     4  0.3470      0.555  0 0.000 0.000 0.740 0.248 0.012
#&gt; SRR1812704     5  0.3076      0.655  0 0.000 0.000 0.240 0.760 0.000
#&gt; SRR1812703     4  0.0363      0.852  0 0.000 0.000 0.988 0.012 0.000
#&gt; SRR1812702     5  0.0000      0.908  0 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812741     2  0.3371      0.477  0 0.708 0.000 0.000 0.292 0.000
#&gt; SRR1812740     3  0.0632      0.965  0 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1812739     2  0.0000      0.887  0 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812738     5  0.3747      0.330  0 0.396 0.000 0.000 0.604 0.000
</code></pre>

<script>
$('#tab-SD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-pam-get-classes-5-a').click(function(){
  $('#tab-SD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-SD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-pam-signature_compare](figure_cola/SD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-SD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-pam-collect-classes](figure_cola/SD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "mclust"]
# you can also extract it by
# res = res_list["SD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-mclust-collect-plots](figure_cola/SD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-mclust-select-partition-number](figure_cola/SD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.759           0.938       0.964         0.3308 0.678   0.678
#> 3 3 0.434           0.588       0.799         0.6428 0.730   0.630
#> 4 4 0.497           0.518       0.746         0.1570 0.899   0.806
#> 5 5 0.481           0.647       0.750         0.1220 0.736   0.454
#> 6 6 0.624           0.677       0.779         0.0959 0.970   0.890
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-classes'>
<ul>
<li><a href='#tab-SD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-mclust-get-classes-1'>
<p><a id='tab-SD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.0000      0.940 1.000 0.000
#&gt; SRR1812753     1  0.0000      0.940 1.000 0.000
#&gt; SRR1812751     1  0.0000      0.940 1.000 0.000
#&gt; SRR1812750     1  0.0000      0.940 1.000 0.000
#&gt; SRR1812748     2  0.5408      0.887 0.124 0.876
#&gt; SRR1812749     1  0.0000      0.940 1.000 0.000
#&gt; SRR1812746     1  0.4431      0.912 0.908 0.092
#&gt; SRR1812745     2  0.6247      0.852 0.156 0.844
#&gt; SRR1812747     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812744     2  0.3274      0.934 0.060 0.940
#&gt; SRR1812743     2  0.5178      0.894 0.116 0.884
#&gt; SRR1812742     2  0.5408      0.887 0.124 0.876
#&gt; SRR1812737     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812735     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812734     1  0.7453      0.757 0.788 0.212
#&gt; SRR1812733     2  0.4562      0.910 0.096 0.904
#&gt; SRR1812736     2  0.6623      0.832 0.172 0.828
#&gt; SRR1812732     2  0.0376      0.963 0.004 0.996
#&gt; SRR1812730     2  0.0672      0.962 0.008 0.992
#&gt; SRR1812731     2  0.0376      0.963 0.004 0.996
#&gt; SRR1812729     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812727     1  0.3431      0.928 0.936 0.064
#&gt; SRR1812726     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812728     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812724     2  0.0376      0.963 0.004 0.996
#&gt; SRR1812725     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812723     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812722     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812721     2  0.2423      0.946 0.040 0.960
#&gt; SRR1812718     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812717     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812716     2  0.0376      0.963 0.004 0.996
#&gt; SRR1812715     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812714     2  0.2603      0.944 0.044 0.956
#&gt; SRR1812719     1  0.4690      0.905 0.900 0.100
#&gt; SRR1812713     2  0.4939      0.899 0.108 0.892
#&gt; SRR1812712     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812711     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812710     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812709     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812708     1  0.2236      0.937 0.964 0.036
#&gt; SRR1812707     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812705     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812706     2  0.0376      0.963 0.004 0.996
#&gt; SRR1812704     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812703     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812702     2  0.2423      0.946 0.040 0.960
#&gt; SRR1812741     2  0.5408      0.887 0.124 0.876
#&gt; SRR1812740     2  0.5408      0.887 0.124 0.876
#&gt; SRR1812739     2  0.0000      0.964 0.000 1.000
#&gt; SRR1812738     2  0.0000      0.964 0.000 1.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-1-a').click(function(){
  $('#tab-SD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-2'>
<p><a id='tab-SD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     3  0.6309     -0.172 0.500 0.000 0.500
#&gt; SRR1812753     3  0.6309     -0.172 0.500 0.000 0.500
#&gt; SRR1812751     1  0.6309     -0.434 0.500 0.000 0.500
#&gt; SRR1812750     3  0.6309     -0.172 0.500 0.000 0.500
#&gt; SRR1812748     3  0.7983      0.362 0.264 0.104 0.632
#&gt; SRR1812749     3  0.6309     -0.172 0.500 0.000 0.500
#&gt; SRR1812746     3  0.0424      0.407 0.000 0.008 0.992
#&gt; SRR1812745     3  0.8950      0.243 0.212 0.220 0.568
#&gt; SRR1812747     2  0.0000      0.876 0.000 1.000 0.000
#&gt; SRR1812744     1  0.9996     -0.302 0.348 0.328 0.324
#&gt; SRR1812743     3  0.8995      0.173 0.372 0.136 0.492
#&gt; SRR1812742     3  0.8853      0.310 0.252 0.176 0.572
#&gt; SRR1812737     2  0.3941      0.845 0.156 0.844 0.000
#&gt; SRR1812735     2  0.2537      0.860 0.080 0.920 0.000
#&gt; SRR1812734     3  0.1453      0.410 0.024 0.008 0.968
#&gt; SRR1812733     2  0.2527      0.866 0.020 0.936 0.044
#&gt; SRR1812736     3  0.8113      0.367 0.212 0.144 0.644
#&gt; SRR1812732     2  0.6228      0.527 0.372 0.624 0.004
#&gt; SRR1812730     2  0.6056      0.640 0.032 0.744 0.224
#&gt; SRR1812731     2  0.6652      0.773 0.172 0.744 0.084
#&gt; SRR1812729     2  0.0000      0.876 0.000 1.000 0.000
#&gt; SRR1812727     3  0.2599      0.396 0.016 0.052 0.932
#&gt; SRR1812726     2  0.1015      0.872 0.008 0.980 0.012
#&gt; SRR1812728     2  0.2269      0.871 0.040 0.944 0.016
#&gt; SRR1812724     2  0.6138      0.797 0.172 0.768 0.060
#&gt; SRR1812725     2  0.1289      0.875 0.032 0.968 0.000
#&gt; SRR1812723     2  0.1774      0.866 0.024 0.960 0.016
#&gt; SRR1812722     2  0.0237      0.876 0.004 0.996 0.000
#&gt; SRR1812721     2  0.7657      0.635 0.116 0.676 0.208
#&gt; SRR1812718     2  0.0237      0.876 0.004 0.996 0.000
#&gt; SRR1812717     2  0.3116      0.859 0.108 0.892 0.000
#&gt; SRR1812716     2  0.1525      0.874 0.032 0.964 0.004
#&gt; SRR1812715     2  0.3941      0.845 0.156 0.844 0.000
#&gt; SRR1812714     2  0.5731      0.807 0.108 0.804 0.088
#&gt; SRR1812719     3  0.2599      0.396 0.016 0.052 0.932
#&gt; SRR1812713     2  0.4121      0.837 0.168 0.832 0.000
#&gt; SRR1812712     2  0.2711      0.865 0.088 0.912 0.000
#&gt; SRR1812711     2  0.1905      0.865 0.028 0.956 0.016
#&gt; SRR1812710     2  0.3941      0.845 0.156 0.844 0.000
#&gt; SRR1812709     2  0.2711      0.863 0.088 0.912 0.000
#&gt; SRR1812708     3  0.2651      0.383 0.060 0.012 0.928
#&gt; SRR1812707     2  0.3941      0.845 0.156 0.844 0.000
#&gt; SRR1812705     2  0.0592      0.874 0.012 0.988 0.000
#&gt; SRR1812706     2  0.1525      0.874 0.032 0.964 0.004
#&gt; SRR1812704     2  0.1529      0.878 0.040 0.960 0.000
#&gt; SRR1812703     2  0.3434      0.838 0.032 0.904 0.064
#&gt; SRR1812702     2  0.4662      0.784 0.032 0.844 0.124
#&gt; SRR1812741     3  0.8107      0.241 0.424 0.068 0.508
#&gt; SRR1812740     3  0.8399      0.351 0.220 0.160 0.620
#&gt; SRR1812739     2  0.4178      0.834 0.172 0.828 0.000
#&gt; SRR1812738     2  0.4121      0.837 0.168 0.832 0.000
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-2-a').click(function(){
  $('#tab-SD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-3'>
<p><a id='tab-SD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.6832     0.4243 0.000 0.132 0.572 0.296
#&gt; SRR1812749     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.5364     0.4584 0.016 0.000 0.592 0.392
#&gt; SRR1812745     3  0.7450     0.2326 0.000 0.228 0.508 0.264
#&gt; SRR1812747     2  0.4830     0.6856 0.000 0.608 0.392 0.000
#&gt; SRR1812744     4  0.6471     0.0501 0.000 0.416 0.072 0.512
#&gt; SRR1812743     4  0.3266     0.4512 0.000 0.168 0.000 0.832
#&gt; SRR1812742     4  0.3658     0.3888 0.000 0.020 0.144 0.836
#&gt; SRR1812737     2  0.0188     0.5640 0.000 0.996 0.004 0.000
#&gt; SRR1812735     2  0.2888     0.6105 0.000 0.872 0.124 0.004
#&gt; SRR1812734     3  0.4804     0.4712 0.000 0.000 0.616 0.384
#&gt; SRR1812733     2  0.5592     0.6565 0.000 0.572 0.404 0.024
#&gt; SRR1812736     3  0.6120     0.4551 0.000 0.076 0.628 0.296
#&gt; SRR1812732     2  0.4888     0.0170 0.000 0.588 0.000 0.412
#&gt; SRR1812730     3  0.5543    -0.5299 0.000 0.424 0.556 0.020
#&gt; SRR1812731     2  0.3688     0.3266 0.000 0.792 0.000 0.208
#&gt; SRR1812729     2  0.4830     0.6860 0.000 0.608 0.392 0.000
#&gt; SRR1812727     3  0.5626     0.4592 0.020 0.004 0.588 0.388
#&gt; SRR1812726     2  0.4866     0.6836 0.000 0.596 0.404 0.000
#&gt; SRR1812728     2  0.5643     0.6674 0.000 0.548 0.428 0.024
#&gt; SRR1812724     2  0.3610     0.3402 0.000 0.800 0.000 0.200
#&gt; SRR1812725     2  0.5161     0.6837 0.000 0.592 0.400 0.008
#&gt; SRR1812723     2  0.4907     0.6770 0.000 0.580 0.420 0.000
#&gt; SRR1812722     2  0.4843     0.6850 0.000 0.604 0.396 0.000
#&gt; SRR1812721     2  0.4994    -0.3313 0.000 0.520 0.000 0.480
#&gt; SRR1812718     2  0.4855     0.6844 0.000 0.600 0.400 0.000
#&gt; SRR1812717     2  0.0000     0.5659 0.000 1.000 0.000 0.000
#&gt; SRR1812716     2  0.5212     0.6760 0.000 0.572 0.420 0.008
#&gt; SRR1812715     2  0.0188     0.5640 0.000 0.996 0.004 0.000
#&gt; SRR1812714     2  0.7091     0.5179 0.000 0.568 0.224 0.208
#&gt; SRR1812719     3  0.5441     0.4622 0.012 0.004 0.588 0.396
#&gt; SRR1812713     2  0.3768     0.3518 0.000 0.808 0.008 0.184
#&gt; SRR1812712     2  0.3448     0.6309 0.000 0.828 0.168 0.004
#&gt; SRR1812711     2  0.4925     0.6716 0.000 0.572 0.428 0.000
#&gt; SRR1812710     2  0.0188     0.5640 0.000 0.996 0.004 0.000
#&gt; SRR1812709     2  0.0000     0.5659 0.000 1.000 0.000 0.000
#&gt; SRR1812708     4  0.9419    -0.2249 0.196 0.120 0.316 0.368
#&gt; SRR1812707     2  0.0188     0.5640 0.000 0.996 0.004 0.000
#&gt; SRR1812705     2  0.4855     0.6844 0.000 0.600 0.400 0.000
#&gt; SRR1812706     2  0.5203     0.6783 0.000 0.576 0.416 0.008
#&gt; SRR1812704     2  0.5600     0.6804 0.000 0.596 0.376 0.028
#&gt; SRR1812703     2  0.4888     0.6808 0.000 0.588 0.412 0.000
#&gt; SRR1812702     2  0.5535     0.6621 0.000 0.560 0.420 0.020
#&gt; SRR1812741     4  0.1716     0.4334 0.000 0.064 0.000 0.936
#&gt; SRR1812740     3  0.6371     0.4576 0.000 0.092 0.608 0.300
#&gt; SRR1812739     2  0.3356     0.3781 0.000 0.824 0.000 0.176
#&gt; SRR1812738     2  0.3528     0.3539 0.000 0.808 0.000 0.192
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-3-a').click(function(){
  $('#tab-SD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-4'>
<p><a id='tab-SD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.7878     0.3599 0.000 0.088 0.404 0.300 0.208
#&gt; SRR1812749     1  0.0000     1.0000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0290     0.5884 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1812745     5  0.7635    -0.4198 0.000 0.104 0.376 0.124 0.396
#&gt; SRR1812747     5  0.2852     0.7617 0.000 0.172 0.000 0.000 0.828
#&gt; SRR1812744     5  0.3597     0.6539 0.000 0.008 0.116 0.044 0.832
#&gt; SRR1812743     4  0.3055     0.7496 0.000 0.144 0.016 0.840 0.000
#&gt; SRR1812742     4  0.5877     0.6444 0.000 0.056 0.092 0.680 0.172
#&gt; SRR1812737     2  0.1851     0.6217 0.000 0.912 0.000 0.000 0.088
#&gt; SRR1812735     2  0.2929     0.6126 0.000 0.820 0.000 0.000 0.180
#&gt; SRR1812734     3  0.3051     0.5759 0.000 0.060 0.864 0.076 0.000
#&gt; SRR1812733     5  0.4881     0.7402 0.000 0.124 0.032 0.084 0.760
#&gt; SRR1812736     3  0.7616     0.3758 0.000 0.084 0.392 0.148 0.376
#&gt; SRR1812732     2  0.7366     0.5670 0.000 0.376 0.028 0.264 0.332
#&gt; SRR1812730     5  0.3667     0.7231 0.000 0.040 0.032 0.084 0.844
#&gt; SRR1812731     2  0.6638     0.5526 0.000 0.440 0.000 0.320 0.240
#&gt; SRR1812729     5  0.2813     0.7595 0.000 0.168 0.000 0.000 0.832
#&gt; SRR1812727     3  0.1356     0.5890 0.012 0.000 0.956 0.004 0.028
#&gt; SRR1812726     5  0.2471     0.7712 0.000 0.136 0.000 0.000 0.864
#&gt; SRR1812728     5  0.1280     0.7676 0.000 0.024 0.008 0.008 0.960
#&gt; SRR1812724     2  0.6636     0.6343 0.000 0.432 0.000 0.232 0.336
#&gt; SRR1812725     5  0.4083     0.7580 0.000 0.132 0.000 0.080 0.788
#&gt; SRR1812723     5  0.0963     0.7684 0.000 0.036 0.000 0.000 0.964
#&gt; SRR1812722     5  0.3661     0.5686 0.000 0.276 0.000 0.000 0.724
#&gt; SRR1812721     2  0.4321     0.0601 0.000 0.600 0.000 0.396 0.004
#&gt; SRR1812718     5  0.2732     0.7731 0.000 0.160 0.000 0.000 0.840
#&gt; SRR1812717     2  0.3966     0.5965 0.000 0.664 0.000 0.000 0.336
#&gt; SRR1812716     5  0.4503     0.7496 0.000 0.140 0.008 0.084 0.768
#&gt; SRR1812715     2  0.1851     0.6217 0.000 0.912 0.000 0.000 0.088
#&gt; SRR1812714     5  0.1525     0.7642 0.000 0.036 0.012 0.004 0.948
#&gt; SRR1812719     3  0.1356     0.5909 0.012 0.000 0.956 0.004 0.028
#&gt; SRR1812713     2  0.5822     0.6542 0.000 0.548 0.000 0.108 0.344
#&gt; SRR1812712     5  0.4171     0.2329 0.000 0.396 0.000 0.000 0.604
#&gt; SRR1812711     5  0.0963     0.7684 0.000 0.036 0.000 0.000 0.964
#&gt; SRR1812710     2  0.1851     0.6217 0.000 0.912 0.000 0.000 0.088
#&gt; SRR1812709     2  0.3966     0.5965 0.000 0.664 0.000 0.000 0.336
#&gt; SRR1812708     3  0.3662     0.3895 0.252 0.000 0.744 0.000 0.004
#&gt; SRR1812707     2  0.1851     0.6217 0.000 0.912 0.000 0.000 0.088
#&gt; SRR1812705     5  0.2813     0.7619 0.000 0.168 0.000 0.000 0.832
#&gt; SRR1812706     5  0.4416     0.7557 0.000 0.132 0.008 0.084 0.776
#&gt; SRR1812704     5  0.2852     0.7626 0.000 0.172 0.000 0.000 0.828
#&gt; SRR1812703     5  0.0290     0.7669 0.000 0.008 0.000 0.000 0.992
#&gt; SRR1812702     5  0.4820     0.7084 0.000 0.080 0.060 0.084 0.776
#&gt; SRR1812741     4  0.4111     0.7843 0.000 0.120 0.092 0.788 0.000
#&gt; SRR1812740     3  0.7469     0.4381 0.000 0.088 0.488 0.148 0.276
#&gt; SRR1812739     2  0.6485     0.6336 0.000 0.460 0.000 0.196 0.344
#&gt; SRR1812738     2  0.6244     0.6314 0.000 0.504 0.000 0.160 0.336
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-4-a').click(function(){
  $('#tab-SD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-mclust-get-classes-5'>
<p><a id='tab-SD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0790      0.978 0.968 0.000 0.000 0.000 0.032 0.000
#&gt; SRR1812753     1  0.0790      0.978 0.968 0.000 0.000 0.000 0.032 0.000
#&gt; SRR1812751     1  0.0000      0.986 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.986 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.3390      0.744 0.000 0.000 0.780 0.008 0.012 0.200
#&gt; SRR1812749     1  0.0000      0.986 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     5  0.4700      0.789 0.040 0.032 0.016 0.000 0.732 0.180
#&gt; SRR1812745     3  0.6098      0.371 0.000 0.380 0.428 0.000 0.012 0.180
#&gt; SRR1812747     2  0.3290      0.708 0.000 0.776 0.016 0.208 0.000 0.000
#&gt; SRR1812744     2  0.6656      0.445 0.000 0.492 0.048 0.172 0.008 0.280
#&gt; SRR1812743     6  0.2831      0.741 0.000 0.000 0.000 0.024 0.136 0.840
#&gt; SRR1812742     6  0.0603      0.835 0.000 0.016 0.004 0.000 0.000 0.980
#&gt; SRR1812737     4  0.0000      0.782 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812735     4  0.3136      0.643 0.000 0.188 0.016 0.796 0.000 0.000
#&gt; SRR1812734     5  0.6117      0.418 0.000 0.024 0.280 0.000 0.516 0.180
#&gt; SRR1812733     2  0.5054      0.405 0.000 0.696 0.064 0.000 0.060 0.180
#&gt; SRR1812736     3  0.3419      0.750 0.000 0.016 0.792 0.000 0.012 0.180
#&gt; SRR1812732     4  0.5492      0.495 0.000 0.028 0.000 0.576 0.080 0.316
#&gt; SRR1812730     2  0.4167      0.519 0.000 0.708 0.236 0.000 0.056 0.000
#&gt; SRR1812731     4  0.5031      0.691 0.000 0.028 0.000 0.688 0.180 0.104
#&gt; SRR1812729     2  0.3261      0.710 0.000 0.780 0.016 0.204 0.000 0.000
#&gt; SRR1812727     5  0.4455      0.797 0.052 0.032 0.000 0.000 0.736 0.180
#&gt; SRR1812726     2  0.3201      0.709 0.000 0.780 0.012 0.208 0.000 0.000
#&gt; SRR1812728     2  0.6097      0.650 0.000 0.584 0.188 0.172 0.056 0.000
#&gt; SRR1812724     4  0.4988      0.694 0.000 0.028 0.000 0.692 0.180 0.100
#&gt; SRR1812725     2  0.4239      0.565 0.000 0.736 0.196 0.012 0.056 0.000
#&gt; SRR1812723     2  0.2854      0.711 0.000 0.792 0.000 0.208 0.000 0.000
#&gt; SRR1812722     2  0.4129      0.342 0.000 0.564 0.012 0.424 0.000 0.000
#&gt; SRR1812721     4  0.3862      0.223 0.000 0.000 0.004 0.608 0.000 0.388
#&gt; SRR1812718     2  0.0865      0.658 0.000 0.964 0.000 0.036 0.000 0.000
#&gt; SRR1812717     4  0.0790      0.780 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1812716     2  0.4142      0.523 0.000 0.712 0.232 0.000 0.056 0.000
#&gt; SRR1812715     4  0.0000      0.782 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.3341      0.710 0.000 0.776 0.012 0.208 0.004 0.000
#&gt; SRR1812719     5  0.4455      0.797 0.052 0.032 0.000 0.000 0.736 0.180
#&gt; SRR1812713     4  0.2883      0.623 0.000 0.212 0.000 0.788 0.000 0.000
#&gt; SRR1812712     2  0.3668      0.423 0.000 0.668 0.004 0.328 0.000 0.000
#&gt; SRR1812711     2  0.2854      0.711 0.000 0.792 0.000 0.208 0.000 0.000
#&gt; SRR1812710     4  0.0000      0.782 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812709     4  0.0790      0.780 0.000 0.032 0.000 0.968 0.000 0.000
#&gt; SRR1812708     5  0.3330      0.532 0.284 0.000 0.000 0.000 0.716 0.000
#&gt; SRR1812707     4  0.0000      0.782 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812705     2  0.3240      0.684 0.000 0.752 0.004 0.244 0.000 0.000
#&gt; SRR1812706     2  0.5076      0.572 0.000 0.672 0.224 0.048 0.056 0.000
#&gt; SRR1812704     2  0.4656      0.617 0.000 0.660 0.016 0.280 0.000 0.044
#&gt; SRR1812703     2  0.2915      0.715 0.000 0.808 0.008 0.184 0.000 0.000
#&gt; SRR1812702     2  0.4494      0.514 0.000 0.708 0.220 0.000 0.056 0.016
#&gt; SRR1812741     6  0.0000      0.838 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812740     3  0.3071      0.746 0.000 0.000 0.804 0.000 0.016 0.180
#&gt; SRR1812739     4  0.6177      0.579 0.000 0.184 0.000 0.576 0.180 0.060
#&gt; SRR1812738     4  0.4730      0.707 0.000 0.032 0.000 0.716 0.180 0.072
</code></pre>

<script>
$('#tab-SD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-mclust-get-classes-5-a').click(function(){
  $('#tab-SD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-SD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-mclust-signature_compare](figure_cola/SD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-SD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-mclust-collect-classes](figure_cola/SD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### SD:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["SD", "NMF"]
# you can also extract it by
# res = res_list["SD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'SD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk SD-NMF-collect-plots](figure_cola/SD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk SD-NMF-select-partition-number](figure_cola/SD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.437           0.768       0.879         0.3772 0.655   0.655
#> 3 3 0.869           0.902       0.959         0.6361 0.707   0.559
#> 4 4 0.671           0.752       0.887         0.1894 0.735   0.418
#> 5 5 0.749           0.725       0.854         0.0816 0.856   0.531
#> 6 6 0.799           0.689       0.857         0.0389 0.928   0.678
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-SD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-classes'>
<ul>
<li><a href='#tab-SD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-SD-NMF-get-classes-1'>
<p><a id='tab-SD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.5629      0.774 0.868 0.132
#&gt; SRR1812753     1  0.2948      0.776 0.948 0.052
#&gt; SRR1812751     1  0.8443      0.733 0.728 0.272
#&gt; SRR1812750     1  0.9427      0.641 0.640 0.360
#&gt; SRR1812748     2  0.9522      0.559 0.372 0.628
#&gt; SRR1812749     1  0.8661      0.722 0.712 0.288
#&gt; SRR1812746     1  0.0000      0.761 1.000 0.000
#&gt; SRR1812745     2  0.9491      0.565 0.368 0.632
#&gt; SRR1812747     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812744     2  0.9393      0.581 0.356 0.644
#&gt; SRR1812743     2  0.6531      0.778 0.168 0.832
#&gt; SRR1812742     2  0.9522      0.559 0.372 0.628
#&gt; SRR1812737     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812735     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812734     1  0.6343      0.656 0.840 0.160
#&gt; SRR1812733     2  0.6801      0.768 0.180 0.820
#&gt; SRR1812736     2  0.9522      0.559 0.372 0.628
#&gt; SRR1812732     2  0.6623      0.775 0.172 0.828
#&gt; SRR1812730     2  0.9460      0.571 0.364 0.636
#&gt; SRR1812731     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812729     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812727     1  0.0672      0.764 0.992 0.008
#&gt; SRR1812726     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812728     2  0.2423      0.851 0.040 0.960
#&gt; SRR1812724     2  0.0938      0.863 0.012 0.988
#&gt; SRR1812725     2  0.3431      0.840 0.064 0.936
#&gt; SRR1812723     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812722     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812721     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812718     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812717     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812716     2  0.8499      0.676 0.276 0.724
#&gt; SRR1812715     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812714     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812719     1  0.0000      0.761 1.000 0.000
#&gt; SRR1812713     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812712     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812711     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812710     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812709     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812708     1  0.9209      0.674 0.664 0.336
#&gt; SRR1812707     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812705     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812706     2  0.5842      0.795 0.140 0.860
#&gt; SRR1812704     2  0.0376      0.866 0.004 0.996
#&gt; SRR1812703     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812702     2  0.9427      0.577 0.360 0.640
#&gt; SRR1812741     1  0.9944      0.282 0.544 0.456
#&gt; SRR1812740     2  0.9491      0.565 0.368 0.632
#&gt; SRR1812739     2  0.0000      0.867 0.000 1.000
#&gt; SRR1812738     2  0.4815      0.818 0.104 0.896
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-1-a').click(function(){
  $('#tab-SD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-2'>
<p><a id='tab-SD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812749     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812746     1  0.5216      0.635 0.740 0.000 0.260
#&gt; SRR1812745     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812747     2  0.0592      0.951 0.000 0.988 0.012
#&gt; SRR1812744     3  0.0237      0.914 0.000 0.004 0.996
#&gt; SRR1812743     3  0.5760      0.552 0.000 0.328 0.672
#&gt; SRR1812742     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812737     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812735     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812734     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812733     3  0.3752      0.801 0.000 0.144 0.856
#&gt; SRR1812736     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812732     3  0.2356      0.868 0.000 0.072 0.928
#&gt; SRR1812730     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812729     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812727     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812726     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812728     2  0.5926      0.457 0.000 0.644 0.356
#&gt; SRR1812724     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812725     2  0.5178      0.661 0.000 0.744 0.256
#&gt; SRR1812723     2  0.0237      0.956 0.000 0.996 0.004
#&gt; SRR1812722     2  0.0237      0.956 0.000 0.996 0.004
#&gt; SRR1812721     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812718     2  0.0892      0.945 0.000 0.980 0.020
#&gt; SRR1812717     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812716     3  0.0592      0.911 0.000 0.012 0.988
#&gt; SRR1812715     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812719     1  0.0424      0.962 0.992 0.000 0.008
#&gt; SRR1812713     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812712     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812711     2  0.0237      0.956 0.000 0.996 0.004
#&gt; SRR1812710     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812709     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812708     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812707     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812705     2  0.2878      0.872 0.000 0.904 0.096
#&gt; SRR1812706     3  0.5216      0.659 0.000 0.260 0.740
#&gt; SRR1812704     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812703     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812702     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812741     1  0.0829      0.955 0.984 0.012 0.004
#&gt; SRR1812740     3  0.0000      0.916 0.000 0.000 1.000
#&gt; SRR1812739     2  0.0000      0.959 0.000 1.000 0.000
#&gt; SRR1812738     2  0.5098      0.645 0.000 0.752 0.248
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-2-a').click(function(){
  $('#tab-SD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-3'>
<p><a id='tab-SD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000     0.9002 0.000 0.000 1.000 0.000
#&gt; SRR1812749     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.5167     0.0737 0.508 0.004 0.488 0.000
#&gt; SRR1812745     3  0.2704     0.7961 0.000 0.124 0.876 0.000
#&gt; SRR1812747     2  0.4477     0.6025 0.000 0.688 0.000 0.312
#&gt; SRR1812744     3  0.0524     0.8977 0.000 0.004 0.988 0.008
#&gt; SRR1812743     4  0.3219     0.7045 0.000 0.000 0.164 0.836
#&gt; SRR1812742     3  0.2760     0.8161 0.000 0.000 0.872 0.128
#&gt; SRR1812737     4  0.0469     0.8572 0.000 0.012 0.000 0.988
#&gt; SRR1812735     4  0.4304     0.5258 0.000 0.284 0.000 0.716
#&gt; SRR1812734     3  0.0188     0.8991 0.000 0.004 0.996 0.000
#&gt; SRR1812733     2  0.5579     0.5325 0.000 0.688 0.252 0.060
#&gt; SRR1812736     3  0.0000     0.9002 0.000 0.000 1.000 0.000
#&gt; SRR1812732     3  0.4382     0.5811 0.000 0.000 0.704 0.296
#&gt; SRR1812730     2  0.2868     0.7257 0.000 0.864 0.136 0.000
#&gt; SRR1812731     4  0.0000     0.8564 0.000 0.000 0.000 1.000
#&gt; SRR1812729     2  0.3356     0.7609 0.000 0.824 0.000 0.176
#&gt; SRR1812727     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812726     2  0.4522     0.5909 0.000 0.680 0.000 0.320
#&gt; SRR1812728     2  0.0188     0.8131 0.000 0.996 0.004 0.000
#&gt; SRR1812724     4  0.0000     0.8564 0.000 0.000 0.000 1.000
#&gt; SRR1812725     2  0.0000     0.8134 0.000 1.000 0.000 0.000
#&gt; SRR1812723     2  0.2081     0.8073 0.000 0.916 0.000 0.084
#&gt; SRR1812722     2  0.4746     0.5017 0.000 0.632 0.000 0.368
#&gt; SRR1812721     4  0.0000     0.8564 0.000 0.000 0.000 1.000
#&gt; SRR1812718     2  0.1474     0.8130 0.000 0.948 0.000 0.052
#&gt; SRR1812717     4  0.0469     0.8572 0.000 0.012 0.000 0.988
#&gt; SRR1812716     2  0.1716     0.7830 0.000 0.936 0.064 0.000
#&gt; SRR1812715     4  0.0188     0.8571 0.000 0.004 0.000 0.996
#&gt; SRR1812714     4  0.4933     0.0887 0.000 0.432 0.000 0.568
#&gt; SRR1812719     1  0.3333     0.8111 0.872 0.088 0.040 0.000
#&gt; SRR1812713     4  0.4356     0.5521 0.000 0.292 0.000 0.708
#&gt; SRR1812712     2  0.4972     0.1441 0.000 0.544 0.000 0.456
#&gt; SRR1812711     2  0.2814     0.7900 0.000 0.868 0.000 0.132
#&gt; SRR1812710     4  0.1302     0.8449 0.000 0.044 0.000 0.956
#&gt; SRR1812709     4  0.1557     0.8381 0.000 0.056 0.000 0.944
#&gt; SRR1812708     1  0.0000     0.9190 1.000 0.000 0.000 0.000
#&gt; SRR1812707     4  0.1474     0.8408 0.000 0.052 0.000 0.948
#&gt; SRR1812705     2  0.2973     0.7839 0.000 0.856 0.000 0.144
#&gt; SRR1812706     2  0.0336     0.8112 0.000 0.992 0.008 0.000
#&gt; SRR1812704     2  0.2921     0.7655 0.000 0.860 0.000 0.140
#&gt; SRR1812703     2  0.0000     0.8134 0.000 1.000 0.000 0.000
#&gt; SRR1812702     2  0.1792     0.7805 0.000 0.932 0.068 0.000
#&gt; SRR1812741     4  0.4669     0.6590 0.052 0.000 0.168 0.780
#&gt; SRR1812740     3  0.0000     0.9002 0.000 0.000 1.000 0.000
#&gt; SRR1812739     4  0.0000     0.8564 0.000 0.000 0.000 1.000
#&gt; SRR1812738     4  0.3982     0.6447 0.000 0.004 0.220 0.776
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-3-a').click(function(){
  $('#tab-SD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-4'>
<p><a id='tab-SD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000    0.94447 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000    0.94447 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000    0.94447 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000    0.94447 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0162    0.82271 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812749     1  0.0000    0.94447 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.2519    0.85963 0.884 0.000 0.100 0.000 0.016
#&gt; SRR1812745     5  0.4307   -0.19941 0.000 0.000 0.500 0.000 0.500
#&gt; SRR1812747     2  0.2426    0.81899 0.000 0.900 0.000 0.036 0.064
#&gt; SRR1812744     3  0.6031    0.60079 0.000 0.000 0.568 0.164 0.268
#&gt; SRR1812743     4  0.4937    0.12189 0.000 0.000 0.428 0.544 0.028
#&gt; SRR1812742     3  0.1731    0.81242 0.000 0.008 0.940 0.012 0.040
#&gt; SRR1812737     4  0.0290    0.83869 0.000 0.008 0.000 0.992 0.000
#&gt; SRR1812735     2  0.2248    0.79371 0.000 0.900 0.000 0.088 0.012
#&gt; SRR1812734     3  0.3452    0.72931 0.000 0.000 0.756 0.000 0.244
#&gt; SRR1812733     5  0.3232    0.65948 0.000 0.036 0.056 0.036 0.872
#&gt; SRR1812736     3  0.0290    0.82249 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1812732     3  0.3910    0.62796 0.000 0.000 0.720 0.272 0.008
#&gt; SRR1812730     5  0.3690    0.75621 0.000 0.200 0.020 0.000 0.780
#&gt; SRR1812731     4  0.0000    0.83858 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812729     2  0.2920    0.71887 0.000 0.852 0.000 0.016 0.132
#&gt; SRR1812727     1  0.2339    0.88839 0.892 0.004 0.004 0.000 0.100
#&gt; SRR1812726     2  0.1216    0.83246 0.000 0.960 0.000 0.020 0.020
#&gt; SRR1812728     5  0.3913    0.70939 0.000 0.324 0.000 0.000 0.676
#&gt; SRR1812724     4  0.0290    0.83906 0.000 0.000 0.000 0.992 0.008
#&gt; SRR1812725     2  0.4304   -0.34364 0.000 0.516 0.000 0.000 0.484
#&gt; SRR1812723     2  0.1768    0.81593 0.000 0.924 0.000 0.004 0.072
#&gt; SRR1812722     2  0.0290    0.84253 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1812721     4  0.0000    0.83858 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812718     2  0.0703    0.83943 0.000 0.976 0.000 0.000 0.024
#&gt; SRR1812717     4  0.1310    0.83193 0.000 0.020 0.000 0.956 0.024
#&gt; SRR1812716     5  0.3809    0.74887 0.000 0.256 0.008 0.000 0.736
#&gt; SRR1812715     4  0.4088    0.54442 0.000 0.304 0.000 0.688 0.008
#&gt; SRR1812714     2  0.4304    0.67141 0.012 0.792 0.000 0.104 0.092
#&gt; SRR1812719     1  0.4011    0.78737 0.804 0.040 0.016 0.000 0.140
#&gt; SRR1812713     4  0.2519    0.79424 0.000 0.016 0.000 0.884 0.100
#&gt; SRR1812712     4  0.3550    0.72851 0.000 0.020 0.000 0.796 0.184
#&gt; SRR1812711     2  0.0162    0.84179 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812710     4  0.3913    0.52579 0.000 0.324 0.000 0.676 0.000
#&gt; SRR1812709     4  0.0324    0.84002 0.000 0.004 0.000 0.992 0.004
#&gt; SRR1812708     1  0.0794    0.93453 0.972 0.000 0.000 0.000 0.028
#&gt; SRR1812707     4  0.0771    0.83702 0.000 0.020 0.000 0.976 0.004
#&gt; SRR1812705     2  0.0865    0.83975 0.000 0.972 0.000 0.004 0.024
#&gt; SRR1812706     5  0.3816    0.73404 0.000 0.304 0.000 0.000 0.696
#&gt; SRR1812704     4  0.6304   -0.00922 0.000 0.156 0.000 0.460 0.384
#&gt; SRR1812703     5  0.3274    0.68473 0.000 0.220 0.000 0.000 0.780
#&gt; SRR1812702     5  0.3752    0.74392 0.000 0.292 0.000 0.000 0.708
#&gt; SRR1812741     4  0.1651    0.82068 0.036 0.000 0.012 0.944 0.008
#&gt; SRR1812740     3  0.1410    0.80777 0.000 0.000 0.940 0.000 0.060
#&gt; SRR1812739     4  0.0566    0.83649 0.000 0.000 0.012 0.984 0.004
#&gt; SRR1812738     4  0.0865    0.83758 0.000 0.004 0.000 0.972 0.024
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-4-a').click(function(){
  $('#tab-SD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-SD-NMF-get-classes-5'>
<p><a id='tab-SD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000     0.8327 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.8327 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.8327 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.8327 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.1196     0.6447 0.000 0.000 0.952 0.000 0.008 0.040
#&gt; SRR1812749     1  0.0000     0.8327 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.4469     0.5260 0.700 0.004 0.076 0.000 0.000 0.220
#&gt; SRR1812745     3  0.5887    -0.1637 0.000 0.004 0.428 0.000 0.172 0.396
#&gt; SRR1812747     2  0.0508     0.7879 0.000 0.984 0.004 0.000 0.012 0.000
#&gt; SRR1812744     6  0.2146     0.6613 0.000 0.004 0.116 0.000 0.000 0.880
#&gt; SRR1812743     3  0.5108     0.1232 0.000 0.000 0.484 0.436 0.000 0.080
#&gt; SRR1812742     3  0.2145     0.6232 0.000 0.012 0.904 0.004 0.004 0.076
#&gt; SRR1812737     4  0.0146     0.9086 0.000 0.004 0.000 0.996 0.000 0.000
#&gt; SRR1812735     2  0.0551     0.7879 0.000 0.984 0.004 0.008 0.004 0.000
#&gt; SRR1812734     6  0.2482     0.6426 0.000 0.000 0.148 0.000 0.004 0.848
#&gt; SRR1812733     6  0.4553     0.2669 0.000 0.000 0.020 0.008 0.452 0.520
#&gt; SRR1812736     3  0.1370     0.6423 0.000 0.004 0.948 0.000 0.012 0.036
#&gt; SRR1812732     3  0.3417     0.5826 0.000 0.000 0.796 0.160 0.000 0.044
#&gt; SRR1812730     5  0.0436     0.8221 0.000 0.004 0.004 0.000 0.988 0.004
#&gt; SRR1812731     4  0.0405     0.9059 0.000 0.000 0.008 0.988 0.000 0.004
#&gt; SRR1812729     2  0.4325     0.0802 0.000 0.524 0.000 0.020 0.456 0.000
#&gt; SRR1812727     1  0.4028     0.5125 0.668 0.000 0.000 0.000 0.024 0.308
#&gt; SRR1812726     2  0.0405     0.7890 0.000 0.988 0.000 0.000 0.004 0.008
#&gt; SRR1812728     5  0.1958     0.8516 0.000 0.100 0.000 0.000 0.896 0.004
#&gt; SRR1812724     4  0.0551     0.9082 0.000 0.000 0.008 0.984 0.004 0.004
#&gt; SRR1812725     5  0.3081     0.7599 0.000 0.220 0.004 0.000 0.776 0.000
#&gt; SRR1812723     2  0.3756     0.3201 0.000 0.600 0.000 0.000 0.400 0.000
#&gt; SRR1812722     2  0.1349     0.7799 0.000 0.940 0.000 0.004 0.056 0.000
#&gt; SRR1812721     4  0.0146     0.9087 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; SRR1812718     2  0.0937     0.7871 0.000 0.960 0.000 0.000 0.040 0.000
#&gt; SRR1812717     4  0.1267     0.8872 0.000 0.000 0.000 0.940 0.060 0.000
#&gt; SRR1812716     5  0.0363     0.8308 0.000 0.012 0.000 0.000 0.988 0.000
#&gt; SRR1812715     4  0.3878     0.5616 0.000 0.320 0.008 0.668 0.000 0.004
#&gt; SRR1812714     2  0.3468     0.4569 0.004 0.712 0.000 0.000 0.000 0.284
#&gt; SRR1812719     1  0.4531     0.2406 0.556 0.000 0.000 0.000 0.408 0.036
#&gt; SRR1812713     4  0.1411     0.8857 0.000 0.000 0.000 0.936 0.060 0.004
#&gt; SRR1812712     4  0.1524     0.8845 0.000 0.000 0.000 0.932 0.060 0.008
#&gt; SRR1812711     2  0.0363     0.7909 0.000 0.988 0.000 0.000 0.012 0.000
#&gt; SRR1812710     4  0.3607     0.5203 0.000 0.348 0.000 0.652 0.000 0.000
#&gt; SRR1812709     4  0.0260     0.9089 0.000 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1812708     1  0.1806     0.7851 0.908 0.004 0.000 0.000 0.000 0.088
#&gt; SRR1812707     4  0.0260     0.9089 0.000 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1812705     2  0.3371     0.5438 0.000 0.708 0.000 0.000 0.292 0.000
#&gt; SRR1812706     5  0.4166     0.7791 0.000 0.196 0.000 0.000 0.728 0.076
#&gt; SRR1812704     5  0.0937     0.8017 0.000 0.000 0.000 0.040 0.960 0.000
#&gt; SRR1812703     6  0.2907     0.6371 0.000 0.020 0.000 0.000 0.152 0.828
#&gt; SRR1812702     5  0.3172     0.8386 0.000 0.128 0.000 0.000 0.824 0.048
#&gt; SRR1812741     4  0.1679     0.8807 0.036 0.000 0.012 0.936 0.000 0.016
#&gt; SRR1812740     3  0.2730     0.5418 0.000 0.000 0.808 0.000 0.192 0.000
#&gt; SRR1812739     4  0.0909     0.8995 0.000 0.000 0.012 0.968 0.000 0.020
#&gt; SRR1812738     4  0.0405     0.9077 0.000 0.000 0.004 0.988 0.000 0.008
</code></pre>

<script>
$('#tab-SD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-SD-NMF-get-classes-5-a').click(function(){
  $('#tab-SD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-SD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-SD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-SD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-SD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-SD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-SD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-SD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-SD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk SD-NMF-signature_compare](figure_cola/SD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-SD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-SD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-SD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-SD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-SD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-SD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-SD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-SD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk SD-NMF-collect-classes](figure_cola/SD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:hclust**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "hclust"]
# you can also extract it by
# res = res_list["CV:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-hclust-collect-plots](figure_cola/CV-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-hclust-select-partition-number](figure_cola/CV-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.966       0.987         0.2499 0.758   0.758
#> 3 3 0.433           0.746       0.794         0.5953 0.961   0.948
#> 4 4 0.377           0.778       0.818         0.0668 0.990   0.986
#> 5 5 0.303           0.644       0.761         0.2433 0.845   0.784
#> 6 6 0.357           0.538       0.741         0.1431 0.966   0.941
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-classes'>
<ul>
<li><a href='#tab-CV-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-hclust-get-classes-1'>
<p><a id='tab-CV-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000     0.9695 1.000 0.000
#&gt; SRR1812753     1   0.000     0.9695 1.000 0.000
#&gt; SRR1812751     1   0.000     0.9695 1.000 0.000
#&gt; SRR1812750     1   0.000     0.9695 1.000 0.000
#&gt; SRR1812748     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812749     1   0.000     0.9695 1.000 0.000
#&gt; SRR1812746     2   0.995     0.0998 0.460 0.540
#&gt; SRR1812745     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812747     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812744     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812743     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812742     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812737     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812735     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812734     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812733     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812736     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812732     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812730     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812731     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812729     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812727     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812726     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812728     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812724     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812725     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812723     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812722     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812721     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812718     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812717     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812716     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812715     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812714     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812719     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812713     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812712     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812711     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812710     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812709     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812708     1   0.260     0.9444 0.956 0.044
#&gt; SRR1812707     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812705     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812706     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812704     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812703     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812702     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812741     1   0.574     0.8496 0.864 0.136
#&gt; SRR1812740     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812739     2   0.000     0.9889 0.000 1.000
#&gt; SRR1812738     2   0.000     0.9889 0.000 1.000
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-1-a').click(function(){
  $('#tab-CV-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-2'>
<p><a id='tab-CV-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.6008     0.6120 0.628 0.000 0.372
#&gt; SRR1812753     1  0.6008     0.6120 0.628 0.000 0.372
#&gt; SRR1812751     1  0.6008     0.6120 0.628 0.000 0.372
#&gt; SRR1812750     1  0.0000     0.5975 1.000 0.000 0.000
#&gt; SRR1812748     2  0.5678     0.7317 0.000 0.684 0.316
#&gt; SRR1812749     1  0.0000     0.5975 1.000 0.000 0.000
#&gt; SRR1812746     3  0.8848     0.0489 0.372 0.124 0.504
#&gt; SRR1812745     2  0.5621     0.7400 0.000 0.692 0.308
#&gt; SRR1812747     2  0.1753     0.8495 0.000 0.952 0.048
#&gt; SRR1812744     2  0.3340     0.8473 0.000 0.880 0.120
#&gt; SRR1812743     2  0.2959     0.8121 0.000 0.900 0.100
#&gt; SRR1812742     2  0.2959     0.8121 0.000 0.900 0.100
#&gt; SRR1812737     2  0.2878     0.8148 0.000 0.904 0.096
#&gt; SRR1812735     2  0.2165     0.8333 0.000 0.936 0.064
#&gt; SRR1812734     2  0.5591     0.7361 0.000 0.696 0.304
#&gt; SRR1812733     2  0.4235     0.8284 0.000 0.824 0.176
#&gt; SRR1812736     2  0.5678     0.7317 0.000 0.684 0.316
#&gt; SRR1812732     2  0.2959     0.8524 0.000 0.900 0.100
#&gt; SRR1812730     2  0.5178     0.7835 0.000 0.744 0.256
#&gt; SRR1812731     2  0.2878     0.8148 0.000 0.904 0.096
#&gt; SRR1812729     2  0.1529     0.8563 0.000 0.960 0.040
#&gt; SRR1812727     2  0.5497     0.7485 0.000 0.708 0.292
#&gt; SRR1812726     2  0.1163     0.8469 0.000 0.972 0.028
#&gt; SRR1812728     2  0.2261     0.8574 0.000 0.932 0.068
#&gt; SRR1812724     2  0.2356     0.8557 0.000 0.928 0.072
#&gt; SRR1812725     2  0.4452     0.8205 0.000 0.808 0.192
#&gt; SRR1812723     2  0.0892     0.8507 0.000 0.980 0.020
#&gt; SRR1812722     2  0.1964     0.8375 0.000 0.944 0.056
#&gt; SRR1812721     2  0.2959     0.8121 0.000 0.900 0.100
#&gt; SRR1812718     2  0.1031     0.8529 0.000 0.976 0.024
#&gt; SRR1812717     2  0.1529     0.8565 0.000 0.960 0.040
#&gt; SRR1812716     2  0.5178     0.7835 0.000 0.744 0.256
#&gt; SRR1812715     2  0.2165     0.8333 0.000 0.936 0.064
#&gt; SRR1812714     2  0.4702     0.8094 0.000 0.788 0.212
#&gt; SRR1812719     2  0.5497     0.7485 0.000 0.708 0.292
#&gt; SRR1812713     2  0.3267     0.8482 0.000 0.884 0.116
#&gt; SRR1812712     2  0.2356     0.8557 0.000 0.928 0.072
#&gt; SRR1812711     2  0.1163     0.8469 0.000 0.972 0.028
#&gt; SRR1812710     2  0.2878     0.8148 0.000 0.904 0.096
#&gt; SRR1812709     2  0.2356     0.8557 0.000 0.928 0.072
#&gt; SRR1812708     1  0.3619     0.4740 0.864 0.000 0.136
#&gt; SRR1812707     2  0.2878     0.8148 0.000 0.904 0.096
#&gt; SRR1812705     2  0.0892     0.8507 0.000 0.980 0.020
#&gt; SRR1812706     2  0.4931     0.7984 0.000 0.768 0.232
#&gt; SRR1812704     2  0.2448     0.8570 0.000 0.924 0.076
#&gt; SRR1812703     2  0.4702     0.8094 0.000 0.788 0.212
#&gt; SRR1812702     2  0.5178     0.7835 0.000 0.744 0.256
#&gt; SRR1812741     3  0.7043    -0.6169 0.400 0.024 0.576
#&gt; SRR1812740     2  0.5678     0.7317 0.000 0.684 0.316
#&gt; SRR1812739     2  0.3116     0.8519 0.000 0.892 0.108
#&gt; SRR1812738     2  0.2356     0.8378 0.000 0.928 0.072
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-2-a').click(function(){
  $('#tab-CV-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-3'>
<p><a id='tab-CV-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     1.0000 1.000 0.000 0.000 0.000
#&gt; SRR1812750     3  0.4925     0.4617 0.428 0.000 0.572 0.000
#&gt; SRR1812748     4  0.4936     0.7297 0.000 0.372 0.004 0.624
#&gt; SRR1812749     3  0.4925     0.4617 0.428 0.000 0.572 0.000
#&gt; SRR1812746     3  0.6376     0.0798 0.000 0.432 0.504 0.064
#&gt; SRR1812745     4  0.5007     0.7427 0.000 0.356 0.008 0.636
#&gt; SRR1812747     4  0.2282     0.8504 0.000 0.024 0.052 0.924
#&gt; SRR1812744     4  0.3606     0.8469 0.000 0.140 0.020 0.840
#&gt; SRR1812743     4  0.3606     0.8035 0.000 0.020 0.140 0.840
#&gt; SRR1812742     4  0.3606     0.8035 0.000 0.020 0.140 0.840
#&gt; SRR1812737     4  0.2706     0.8258 0.000 0.020 0.080 0.900
#&gt; SRR1812735     4  0.1970     0.8385 0.000 0.008 0.060 0.932
#&gt; SRR1812734     4  0.5339     0.7191 0.000 0.356 0.020 0.624
#&gt; SRR1812733     4  0.3969     0.8329 0.000 0.180 0.016 0.804
#&gt; SRR1812736     4  0.4936     0.7297 0.000 0.372 0.004 0.624
#&gt; SRR1812732     4  0.3335     0.8522 0.000 0.120 0.020 0.860
#&gt; SRR1812730     4  0.4576     0.7958 0.000 0.260 0.012 0.728
#&gt; SRR1812731     4  0.2706     0.8258 0.000 0.020 0.080 0.900
#&gt; SRR1812729     4  0.1302     0.8577 0.000 0.044 0.000 0.956
#&gt; SRR1812727     4  0.4955     0.7382 0.000 0.344 0.008 0.648
#&gt; SRR1812726     4  0.0927     0.8488 0.000 0.008 0.016 0.976
#&gt; SRR1812728     4  0.2048     0.8609 0.000 0.064 0.008 0.928
#&gt; SRR1812724     4  0.2402     0.8563 0.000 0.076 0.012 0.912
#&gt; SRR1812725     4  0.3893     0.8287 0.000 0.196 0.008 0.796
#&gt; SRR1812723     4  0.0524     0.8521 0.000 0.004 0.008 0.988
#&gt; SRR1812722     4  0.2489     0.8386 0.000 0.020 0.068 0.912
#&gt; SRR1812721     4  0.3554     0.8057 0.000 0.020 0.136 0.844
#&gt; SRR1812718     4  0.0657     0.8545 0.000 0.012 0.004 0.984
#&gt; SRR1812717     4  0.1661     0.8571 0.000 0.052 0.004 0.944
#&gt; SRR1812716     4  0.4576     0.7958 0.000 0.260 0.012 0.728
#&gt; SRR1812715     4  0.1970     0.8385 0.000 0.008 0.060 0.932
#&gt; SRR1812714     4  0.4194     0.8138 0.000 0.228 0.008 0.764
#&gt; SRR1812719     4  0.4936     0.7422 0.000 0.340 0.008 0.652
#&gt; SRR1812713     4  0.3166     0.8511 0.000 0.116 0.016 0.868
#&gt; SRR1812712     4  0.2402     0.8563 0.000 0.076 0.012 0.912
#&gt; SRR1812711     4  0.0927     0.8488 0.000 0.008 0.016 0.976
#&gt; SRR1812710     4  0.2706     0.8258 0.000 0.020 0.080 0.900
#&gt; SRR1812709     4  0.2402     0.8563 0.000 0.076 0.012 0.912
#&gt; SRR1812708     3  0.4980     0.4733 0.304 0.016 0.680 0.000
#&gt; SRR1812707     4  0.2706     0.8258 0.000 0.020 0.080 0.900
#&gt; SRR1812705     4  0.0524     0.8521 0.000 0.004 0.008 0.988
#&gt; SRR1812706     4  0.4353     0.8103 0.000 0.232 0.012 0.756
#&gt; SRR1812704     4  0.2342     0.8582 0.000 0.080 0.008 0.912
#&gt; SRR1812703     4  0.4194     0.8138 0.000 0.228 0.008 0.764
#&gt; SRR1812702     4  0.4576     0.7958 0.000 0.260 0.012 0.728
#&gt; SRR1812741     2  0.7149     0.0000 0.264 0.552 0.184 0.000
#&gt; SRR1812740     4  0.4936     0.7297 0.000 0.372 0.004 0.624
#&gt; SRR1812739     4  0.3108     0.8543 0.000 0.112 0.016 0.872
#&gt; SRR1812738     4  0.2450     0.8448 0.000 0.016 0.072 0.912
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-3-a').click(function(){
  $('#tab-CV-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-4'>
<p><a id='tab-CV-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1   0.000      1.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     3   0.304      0.904 0.192 0.000 0.808 0.000 0.000
#&gt; SRR1812748     2   0.550      0.343 0.000 0.580 0.000 0.080 0.340
#&gt; SRR1812749     3   0.304      0.904 0.192 0.000 0.808 0.000 0.000
#&gt; SRR1812746     5   0.580     -0.322 0.000 0.008 0.372 0.076 0.544
#&gt; SRR1812745     2   0.571      0.374 0.000 0.564 0.000 0.100 0.336
#&gt; SRR1812747     2   0.271      0.743 0.000 0.880 0.000 0.088 0.032
#&gt; SRR1812744     2   0.505      0.329 0.000 0.652 0.000 0.064 0.284
#&gt; SRR1812743     2   0.346      0.670 0.000 0.792 0.000 0.196 0.012
#&gt; SRR1812742     2   0.413      0.664 0.000 0.760 0.000 0.196 0.044
#&gt; SRR1812737     2   0.247      0.711 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1812735     2   0.267      0.720 0.000 0.876 0.000 0.104 0.020
#&gt; SRR1812734     5   0.391      0.640 0.000 0.240 0.000 0.016 0.744
#&gt; SRR1812733     2   0.410      0.658 0.000 0.764 0.000 0.192 0.044
#&gt; SRR1812736     2   0.560      0.307 0.000 0.544 0.000 0.080 0.376
#&gt; SRR1812732     2   0.370      0.693 0.000 0.816 0.000 0.064 0.120
#&gt; SRR1812730     2   0.524      0.570 0.000 0.680 0.000 0.132 0.188
#&gt; SRR1812731     2   0.247      0.711 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1812729     2   0.170      0.741 0.000 0.932 0.000 0.060 0.008
#&gt; SRR1812727     5   0.397      0.667 0.000 0.264 0.000 0.012 0.724
#&gt; SRR1812726     2   0.311      0.658 0.000 0.852 0.000 0.036 0.112
#&gt; SRR1812728     2   0.293      0.727 0.000 0.864 0.000 0.032 0.104
#&gt; SRR1812724     2   0.257      0.728 0.000 0.880 0.000 0.104 0.016
#&gt; SRR1812725     2   0.459      0.640 0.000 0.748 0.000 0.116 0.136
#&gt; SRR1812723     2   0.201      0.738 0.000 0.920 0.000 0.020 0.060
#&gt; SRR1812722     2   0.412      0.629 0.000 0.788 0.000 0.104 0.108
#&gt; SRR1812721     2   0.353      0.670 0.000 0.792 0.000 0.192 0.016
#&gt; SRR1812718     2   0.174      0.744 0.000 0.936 0.000 0.024 0.040
#&gt; SRR1812717     2   0.194      0.736 0.000 0.920 0.000 0.068 0.012
#&gt; SRR1812716     2   0.524      0.570 0.000 0.680 0.000 0.132 0.188
#&gt; SRR1812715     2   0.267      0.720 0.000 0.876 0.000 0.104 0.020
#&gt; SRR1812714     5   0.472      0.556 0.000 0.448 0.000 0.016 0.536
#&gt; SRR1812719     5   0.443      0.617 0.000 0.360 0.000 0.012 0.628
#&gt; SRR1812713     2   0.311      0.710 0.000 0.840 0.000 0.140 0.020
#&gt; SRR1812712     2   0.247      0.727 0.000 0.884 0.000 0.104 0.012
#&gt; SRR1812711     2   0.230      0.738 0.000 0.908 0.000 0.040 0.052
#&gt; SRR1812710     2   0.247      0.711 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1812709     2   0.247      0.727 0.000 0.884 0.000 0.104 0.012
#&gt; SRR1812708     3   0.500      0.803 0.140 0.000 0.720 0.004 0.136
#&gt; SRR1812707     2   0.247      0.711 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1812705     2   0.201      0.738 0.000 0.920 0.000 0.020 0.060
#&gt; SRR1812706     2   0.500      0.600 0.000 0.708 0.000 0.128 0.164
#&gt; SRR1812704     2   0.267      0.733 0.000 0.876 0.000 0.104 0.020
#&gt; SRR1812703     5   0.472      0.556 0.000 0.448 0.000 0.016 0.536
#&gt; SRR1812702     2   0.524      0.570 0.000 0.680 0.000 0.132 0.188
#&gt; SRR1812741     4   0.605      0.000 0.260 0.000 0.140 0.592 0.008
#&gt; SRR1812740     2   0.559      0.318 0.000 0.548 0.000 0.080 0.372
#&gt; SRR1812739     2   0.311      0.720 0.000 0.844 0.000 0.132 0.024
#&gt; SRR1812738     2   0.254      0.726 0.000 0.868 0.000 0.128 0.004
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-4-a').click(function(){
  $('#tab-CV-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-hclust-get-classes-5'>
<p><a id='tab-CV-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1   0.000     1.0000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     4   0.176     0.9059 0.096 0.000 0.000 0.904 0.000 0.000
#&gt; SRR1812748     2   0.664     0.0764 0.000 0.472 0.328 0.004 0.124 0.072
#&gt; SRR1812749     4   0.176     0.9059 0.096 0.000 0.000 0.904 0.000 0.000
#&gt; SRR1812746     3   0.584    -0.4480 0.000 0.000 0.632 0.160 0.132 0.076
#&gt; SRR1812745     3   0.584    -0.4638 0.000 0.440 0.452 0.004 0.064 0.040
#&gt; SRR1812747     2   0.410     0.6342 0.000 0.768 0.160 0.008 0.008 0.056
#&gt; SRR1812744     2   0.620     0.2246 0.000 0.516 0.116 0.012 0.328 0.028
#&gt; SRR1812743     2   0.528     0.5903 0.000 0.712 0.124 0.036 0.024 0.104
#&gt; SRR1812742     2   0.579     0.5743 0.000 0.656 0.176 0.036 0.028 0.104
#&gt; SRR1812737     2   0.342     0.6309 0.000 0.844 0.068 0.036 0.004 0.048
#&gt; SRR1812735     2   0.370     0.6525 0.000 0.832 0.080 0.032 0.036 0.020
#&gt; SRR1812734     5   0.271     0.6241 0.000 0.064 0.024 0.004 0.884 0.024
#&gt; SRR1812733     2   0.514     0.5339 0.000 0.688 0.212 0.032 0.020 0.048
#&gt; SRR1812736     2   0.674    -0.0418 0.000 0.408 0.388 0.004 0.128 0.072
#&gt; SRR1812732     2   0.531     0.5705 0.000 0.688 0.152 0.012 0.120 0.028
#&gt; SRR1812730     2   0.414     0.3349 0.000 0.556 0.432 0.000 0.012 0.000
#&gt; SRR1812731     2   0.342     0.6309 0.000 0.844 0.068 0.036 0.004 0.048
#&gt; SRR1812729     2   0.285     0.6573 0.000 0.876 0.068 0.028 0.024 0.004
#&gt; SRR1812727     5   0.150     0.6588 0.000 0.076 0.000 0.000 0.924 0.000
#&gt; SRR1812726     2   0.437     0.5881 0.000 0.740 0.052 0.012 0.188 0.008
#&gt; SRR1812728     2   0.392     0.6090 0.000 0.748 0.192 0.000 0.060 0.000
#&gt; SRR1812724     2   0.345     0.6354 0.000 0.828 0.116 0.036 0.008 0.012
#&gt; SRR1812725     2   0.431     0.4665 0.000 0.624 0.344 0.000 0.032 0.000
#&gt; SRR1812723     2   0.341     0.6426 0.000 0.808 0.128 0.000 0.064 0.000
#&gt; SRR1812722     2   0.551     0.5625 0.000 0.680 0.056 0.020 0.180 0.064
#&gt; SRR1812721     2   0.506     0.6039 0.000 0.736 0.100 0.036 0.028 0.100
#&gt; SRR1812718     2   0.299     0.6494 0.000 0.828 0.144 0.000 0.028 0.000
#&gt; SRR1812717     2   0.277     0.6472 0.000 0.880 0.068 0.032 0.008 0.012
#&gt; SRR1812716     2   0.415     0.3299 0.000 0.552 0.436 0.000 0.012 0.000
#&gt; SRR1812715     2   0.370     0.6525 0.000 0.832 0.080 0.032 0.036 0.020
#&gt; SRR1812714     5   0.376     0.7275 0.000 0.256 0.012 0.008 0.724 0.000
#&gt; SRR1812719     5   0.362     0.5565 0.000 0.244 0.020 0.000 0.736 0.000
#&gt; SRR1812713     2   0.416     0.6032 0.000 0.788 0.120 0.036 0.008 0.048
#&gt; SRR1812712     2   0.326     0.6322 0.000 0.844 0.100 0.036 0.008 0.012
#&gt; SRR1812711     2   0.405     0.6468 0.000 0.780 0.144 0.012 0.056 0.008
#&gt; SRR1812710     2   0.337     0.6325 0.000 0.848 0.064 0.036 0.004 0.048
#&gt; SRR1812709     2   0.326     0.6322 0.000 0.844 0.100 0.036 0.008 0.012
#&gt; SRR1812708     4   0.501     0.8031 0.092 0.000 0.140 0.712 0.056 0.000
#&gt; SRR1812707     2   0.337     0.6325 0.000 0.848 0.064 0.036 0.004 0.048
#&gt; SRR1812705     2   0.341     0.6426 0.000 0.808 0.128 0.000 0.064 0.000
#&gt; SRR1812706     2   0.425     0.3804 0.000 0.580 0.400 0.000 0.020 0.000
#&gt; SRR1812704     2   0.340     0.6474 0.000 0.832 0.116 0.028 0.012 0.012
#&gt; SRR1812703     5   0.376     0.7275 0.000 0.256 0.012 0.008 0.724 0.000
#&gt; SRR1812702     2   0.415     0.3299 0.000 0.552 0.436 0.000 0.012 0.000
#&gt; SRR1812741     6   0.305     0.0000 0.236 0.000 0.000 0.000 0.000 0.764
#&gt; SRR1812740     2   0.671    -0.0323 0.000 0.412 0.388 0.004 0.124 0.072
#&gt; SRR1812739     2   0.427     0.6191 0.000 0.760 0.172 0.028 0.012 0.028
#&gt; SRR1812738     2   0.328     0.6410 0.000 0.852 0.072 0.040 0.004 0.032
</code></pre>

<script>
$('#tab-CV-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-hclust-get-classes-5-a').click(function(){
  $('#tab-CV-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-hclust-signature_compare](figure_cola/CV-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-hclust-collect-classes](figure_cola/CV-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:kmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "kmeans"]
# you can also extract it by
# res = res_list["CV:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-kmeans-collect-plots](figure_cola/CV-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-kmeans-select-partition-number](figure_cola/CV-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.996       0.997         0.2441 0.758   0.758
#> 3 3 0.441           0.592       0.812         1.3832 0.624   0.504
#> 4 4 0.499           0.649       0.778         0.2052 0.787   0.523
#> 5 5 0.604           0.585       0.755         0.0859 0.920   0.747
#> 6 6 0.676           0.634       0.731         0.0489 0.911   0.660
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-classes'>
<ul>
<li><a href='#tab-CV-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-kmeans-get-classes-1'>
<p><a id='tab-CV-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.0000      0.998 1.000 0.000
#&gt; SRR1812753     1  0.0000      0.998 1.000 0.000
#&gt; SRR1812751     1  0.0000      0.998 1.000 0.000
#&gt; SRR1812750     1  0.0000      0.998 1.000 0.000
#&gt; SRR1812748     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812749     1  0.0000      0.998 1.000 0.000
#&gt; SRR1812746     2  0.2778      0.953 0.048 0.952
#&gt; SRR1812745     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812747     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812744     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812743     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812742     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812737     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812735     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812734     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812733     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812736     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812732     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812730     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812731     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812729     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812727     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812726     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812728     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812724     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812725     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812723     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812722     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812721     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812718     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812717     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812716     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812715     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812714     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812719     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812713     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812712     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812711     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812710     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812709     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812708     1  0.0000      0.998 1.000 0.000
#&gt; SRR1812707     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812705     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812706     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812704     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812703     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812702     2  0.0376      0.997 0.004 0.996
#&gt; SRR1812741     1  0.0938      0.990 0.988 0.012
#&gt; SRR1812740     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812739     2  0.0000      0.997 0.000 1.000
#&gt; SRR1812738     2  0.0000      0.997 0.000 1.000
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-1-a').click(function(){
  $('#tab-CV-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-2'>
<p><a id='tab-CV-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000     0.9178 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.9178 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.9178 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0747     0.9174 0.984 0.000 0.016
#&gt; SRR1812748     3  0.6225     0.1460 0.000 0.432 0.568
#&gt; SRR1812749     1  0.0747     0.9174 0.984 0.000 0.016
#&gt; SRR1812746     3  0.1129     0.6032 0.004 0.020 0.976
#&gt; SRR1812745     3  0.2537     0.6726 0.000 0.080 0.920
#&gt; SRR1812747     3  0.6274     0.4344 0.000 0.456 0.544
#&gt; SRR1812744     2  0.6192     0.2581 0.000 0.580 0.420
#&gt; SRR1812743     2  0.1163     0.7899 0.000 0.972 0.028
#&gt; SRR1812742     3  0.6302     0.3910 0.000 0.480 0.520
#&gt; SRR1812737     2  0.0892     0.7908 0.000 0.980 0.020
#&gt; SRR1812735     2  0.5859     0.0938 0.000 0.656 0.344
#&gt; SRR1812734     3  0.5882     0.2394 0.000 0.348 0.652
#&gt; SRR1812733     2  0.6291    -0.0796 0.000 0.532 0.468
#&gt; SRR1812736     3  0.2537     0.6726 0.000 0.080 0.920
#&gt; SRR1812732     2  0.3192     0.7170 0.000 0.888 0.112
#&gt; SRR1812730     3  0.3686     0.6928 0.000 0.140 0.860
#&gt; SRR1812731     2  0.0892     0.7908 0.000 0.980 0.020
#&gt; SRR1812729     2  0.1163     0.7898 0.000 0.972 0.028
#&gt; SRR1812727     3  0.6305    -0.0495 0.000 0.484 0.516
#&gt; SRR1812726     2  0.6267    -0.2709 0.000 0.548 0.452
#&gt; SRR1812728     3  0.4796     0.6685 0.000 0.220 0.780
#&gt; SRR1812724     2  0.0592     0.7913 0.000 0.988 0.012
#&gt; SRR1812725     3  0.4931     0.6614 0.000 0.232 0.768
#&gt; SRR1812723     3  0.6307     0.3849 0.000 0.488 0.512
#&gt; SRR1812722     2  0.6267    -0.2709 0.000 0.548 0.452
#&gt; SRR1812721     2  0.0892     0.7908 0.000 0.980 0.020
#&gt; SRR1812718     3  0.6307     0.3849 0.000 0.488 0.512
#&gt; SRR1812717     2  0.0592     0.7913 0.000 0.988 0.012
#&gt; SRR1812716     3  0.3686     0.6928 0.000 0.140 0.860
#&gt; SRR1812715     2  0.0892     0.7908 0.000 0.980 0.020
#&gt; SRR1812714     2  0.3116     0.7196 0.000 0.892 0.108
#&gt; SRR1812719     3  0.1753     0.6508 0.000 0.048 0.952
#&gt; SRR1812713     2  0.2356     0.7592 0.000 0.928 0.072
#&gt; SRR1812712     2  0.2356     0.7592 0.000 0.928 0.072
#&gt; SRR1812711     3  0.6308     0.3752 0.000 0.492 0.508
#&gt; SRR1812710     2  0.0892     0.7908 0.000 0.980 0.020
#&gt; SRR1812709     2  0.0592     0.7913 0.000 0.988 0.012
#&gt; SRR1812708     1  0.1411     0.9108 0.964 0.000 0.036
#&gt; SRR1812707     2  0.0892     0.7908 0.000 0.980 0.020
#&gt; SRR1812705     3  0.6307     0.3849 0.000 0.488 0.512
#&gt; SRR1812706     3  0.3816     0.6914 0.000 0.148 0.852
#&gt; SRR1812704     2  0.2537     0.7693 0.000 0.920 0.080
#&gt; SRR1812703     2  0.4750     0.6485 0.000 0.784 0.216
#&gt; SRR1812702     3  0.3686     0.6928 0.000 0.140 0.860
#&gt; SRR1812741     1  0.8257     0.3507 0.544 0.372 0.084
#&gt; SRR1812740     3  0.3551     0.6902 0.000 0.132 0.868
#&gt; SRR1812739     2  0.4002     0.6754 0.000 0.840 0.160
#&gt; SRR1812738     2  0.0424     0.7909 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-2-a').click(function(){
  $('#tab-CV-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-3'>
<p><a id='tab-CV-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette   p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000    0.94746 1.00 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000    0.94746 1.00 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000    0.94746 1.00 0.000 0.000 0.000
#&gt; SRR1812750     1  0.2255    0.94248 0.92 0.012 0.068 0.000
#&gt; SRR1812748     4  0.7902   -0.29224 0.00 0.296 0.352 0.352
#&gt; SRR1812749     1  0.2255    0.94248 0.92 0.012 0.068 0.000
#&gt; SRR1812746     3  0.4605    0.50056 0.00 0.336 0.664 0.000
#&gt; SRR1812745     2  0.4707    0.52328 0.00 0.760 0.204 0.036
#&gt; SRR1812747     2  0.5751    0.71854 0.00 0.712 0.164 0.124
#&gt; SRR1812744     3  0.5131    0.60794 0.00 0.028 0.692 0.280
#&gt; SRR1812743     4  0.4104    0.74093 0.00 0.028 0.164 0.808
#&gt; SRR1812742     2  0.6193    0.68672 0.00 0.672 0.180 0.148
#&gt; SRR1812737     4  0.3962    0.74740 0.00 0.028 0.152 0.820
#&gt; SRR1812735     2  0.7253    0.38025 0.00 0.484 0.152 0.364
#&gt; SRR1812734     3  0.4868    0.61867 0.00 0.212 0.748 0.040
#&gt; SRR1812733     4  0.6187    0.39081 0.00 0.236 0.108 0.656
#&gt; SRR1812736     2  0.4655    0.52051 0.00 0.760 0.208 0.032
#&gt; SRR1812732     4  0.5466   -0.00562 0.00 0.016 0.436 0.548
#&gt; SRR1812730     2  0.2830    0.66648 0.00 0.900 0.040 0.060
#&gt; SRR1812731     4  0.3910    0.74596 0.00 0.024 0.156 0.820
#&gt; SRR1812729     4  0.3052    0.68379 0.00 0.136 0.004 0.860
#&gt; SRR1812727     3  0.5793    0.59124 0.00 0.324 0.628 0.048
#&gt; SRR1812726     2  0.5993    0.70151 0.00 0.692 0.160 0.148
#&gt; SRR1812728     2  0.2676    0.70690 0.00 0.896 0.012 0.092
#&gt; SRR1812724     4  0.0657    0.75044 0.00 0.004 0.012 0.984
#&gt; SRR1812725     2  0.3037    0.70112 0.00 0.880 0.020 0.100
#&gt; SRR1812723     2  0.5758    0.71826 0.00 0.712 0.160 0.128
#&gt; SRR1812722     2  0.5990    0.70291 0.00 0.692 0.164 0.144
#&gt; SRR1812721     4  0.4105    0.74341 0.00 0.032 0.156 0.812
#&gt; SRR1812718     2  0.5758    0.71830 0.00 0.712 0.160 0.128
#&gt; SRR1812717     4  0.0376    0.75470 0.00 0.004 0.004 0.992
#&gt; SRR1812716     2  0.2919    0.66652 0.00 0.896 0.044 0.060
#&gt; SRR1812715     4  0.3913    0.74567 0.00 0.028 0.148 0.824
#&gt; SRR1812714     3  0.6100    0.51686 0.00 0.072 0.624 0.304
#&gt; SRR1812719     2  0.5510    0.06677 0.00 0.600 0.376 0.024
#&gt; SRR1812713     4  0.1767    0.74463 0.00 0.044 0.012 0.944
#&gt; SRR1812712     4  0.1635    0.74571 0.00 0.044 0.008 0.948
#&gt; SRR1812711     2  0.5763    0.71636 0.00 0.712 0.156 0.132
#&gt; SRR1812710     4  0.3913    0.74567 0.00 0.028 0.148 0.824
#&gt; SRR1812709     4  0.0376    0.75470 0.00 0.004 0.004 0.992
#&gt; SRR1812708     1  0.3707    0.88626 0.84 0.028 0.132 0.000
#&gt; SRR1812707     4  0.3863    0.74758 0.00 0.028 0.144 0.828
#&gt; SRR1812705     2  0.5758    0.71826 0.00 0.712 0.160 0.128
#&gt; SRR1812706     2  0.1890    0.68141 0.00 0.936 0.008 0.056
#&gt; SRR1812704     4  0.2647    0.69659 0.00 0.120 0.000 0.880
#&gt; SRR1812703     3  0.7058    0.59562 0.00 0.168 0.560 0.272
#&gt; SRR1812702     2  0.2919    0.66652 0.00 0.896 0.044 0.060
#&gt; SRR1812741     3  0.7938    0.39350 0.18 0.020 0.488 0.312
#&gt; SRR1812740     2  0.4595    0.55719 0.00 0.780 0.176 0.044
#&gt; SRR1812739     4  0.3164    0.67158 0.00 0.064 0.052 0.884
#&gt; SRR1812738     4  0.1297    0.75275 0.00 0.016 0.020 0.964
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-3-a').click(function(){
  $('#tab-CV-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-4'>
<p><a id='tab-CV-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1   0.000     0.8876 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1   0.000     0.8876 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1   0.000     0.8876 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1   0.330     0.8763 0.852 0.004 0.108 0.004 0.032
#&gt; SRR1812748     3   0.614     0.2372 0.000 0.040 0.612 0.264 0.084
#&gt; SRR1812749     1   0.330     0.8763 0.852 0.004 0.108 0.004 0.032
#&gt; SRR1812746     3   0.385     0.1692 0.000 0.036 0.784 0.000 0.180
#&gt; SRR1812745     3   0.473     0.4638 0.000 0.400 0.580 0.000 0.020
#&gt; SRR1812747     2   0.258     0.6763 0.000 0.900 0.064 0.020 0.016
#&gt; SRR1812744     5   0.345     0.7807 0.000 0.004 0.064 0.088 0.844
#&gt; SRR1812743     4   0.476     0.6712 0.000 0.144 0.036 0.764 0.056
#&gt; SRR1812742     2   0.469     0.5749 0.000 0.776 0.080 0.112 0.032
#&gt; SRR1812737     4   0.432     0.6782 0.000 0.144 0.028 0.788 0.040
#&gt; SRR1812735     2   0.591     0.3646 0.000 0.624 0.100 0.256 0.020
#&gt; SRR1812734     5   0.374     0.7109 0.000 0.004 0.224 0.008 0.764
#&gt; SRR1812733     4   0.701     0.4421 0.000 0.088 0.228 0.564 0.120
#&gt; SRR1812736     3   0.477     0.4710 0.000 0.384 0.592 0.000 0.024
#&gt; SRR1812732     4   0.566     0.0157 0.000 0.016 0.044 0.500 0.440
#&gt; SRR1812730     2   0.521     0.2115 0.000 0.620 0.332 0.016 0.032
#&gt; SRR1812731     4   0.461     0.6748 0.000 0.136 0.036 0.776 0.052
#&gt; SRR1812729     4   0.556     0.6538 0.000 0.108 0.056 0.716 0.120
#&gt; SRR1812727     5   0.389     0.7584 0.000 0.112 0.064 0.008 0.816
#&gt; SRR1812726     2   0.173     0.6817 0.000 0.940 0.008 0.040 0.012
#&gt; SRR1812728     2   0.306     0.6379 0.000 0.860 0.112 0.020 0.008
#&gt; SRR1812724     4   0.387     0.6958 0.000 0.016 0.056 0.824 0.104
#&gt; SRR1812725     2   0.355     0.5873 0.000 0.836 0.120 0.024 0.020
#&gt; SRR1812723     2   0.147     0.6848 0.000 0.952 0.020 0.024 0.004
#&gt; SRR1812722     2   0.277     0.6576 0.000 0.896 0.036 0.044 0.024
#&gt; SRR1812721     4   0.495     0.6534 0.000 0.196 0.048 0.728 0.028
#&gt; SRR1812718     2   0.226     0.6790 0.000 0.912 0.060 0.024 0.004
#&gt; SRR1812717     4   0.360     0.6983 0.000 0.020 0.036 0.840 0.104
#&gt; SRR1812716     2   0.503     0.2540 0.000 0.648 0.308 0.016 0.028
#&gt; SRR1812715     4   0.493     0.6523 0.000 0.204 0.048 0.724 0.024
#&gt; SRR1812714     5   0.400     0.7523 0.000 0.084 0.000 0.120 0.796
#&gt; SRR1812719     2   0.618     0.1066 0.000 0.544 0.180 0.000 0.276
#&gt; SRR1812713     4   0.446     0.6815 0.000 0.040 0.044 0.788 0.128
#&gt; SRR1812712     4   0.453     0.6864 0.000 0.040 0.048 0.784 0.128
#&gt; SRR1812711     2   0.243     0.6817 0.000 0.908 0.048 0.036 0.008
#&gt; SRR1812710     4   0.456     0.6587 0.000 0.204 0.028 0.744 0.024
#&gt; SRR1812709     4   0.330     0.7033 0.000 0.020 0.024 0.856 0.100
#&gt; SRR1812708     1   0.584     0.6891 0.616 0.004 0.264 0.004 0.112
#&gt; SRR1812707     4   0.440     0.6758 0.000 0.168 0.036 0.772 0.024
#&gt; SRR1812705     2   0.146     0.6861 0.000 0.952 0.016 0.028 0.004
#&gt; SRR1812706     2   0.462     0.4281 0.000 0.712 0.248 0.016 0.024
#&gt; SRR1812704     4   0.497     0.6732 0.000 0.064 0.056 0.760 0.120
#&gt; SRR1812703     5   0.274     0.7784 0.000 0.044 0.004 0.064 0.888
#&gt; SRR1812702     2   0.505     0.2505 0.000 0.644 0.312 0.016 0.028
#&gt; SRR1812741     4   0.806    -0.1363 0.096 0.000 0.300 0.364 0.240
#&gt; SRR1812740     3   0.441     0.3339 0.000 0.440 0.556 0.000 0.004
#&gt; SRR1812739     4   0.464     0.6647 0.000 0.024 0.072 0.772 0.132
#&gt; SRR1812738     4   0.223     0.6951 0.000 0.008 0.020 0.916 0.056
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-4-a').click(function(){
  $('#tab-CV-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-kmeans-get-classes-5'>
<p><a id='tab-CV-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000      0.837 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.837 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.837 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.4128      0.818 0.788 0.072 0.096 0.000 0.000 0.044
#&gt; SRR1812748     3  0.5941      0.401 0.000 0.180 0.636 0.124 0.024 0.036
#&gt; SRR1812749     1  0.4128      0.818 0.788 0.072 0.096 0.000 0.000 0.044
#&gt; SRR1812746     3  0.3351      0.394 0.000 0.108 0.832 0.000 0.040 0.020
#&gt; SRR1812745     3  0.3707      0.607 0.000 0.000 0.680 0.000 0.312 0.008
#&gt; SRR1812747     5  0.3766      0.740 0.000 0.080 0.080 0.008 0.816 0.016
#&gt; SRR1812744     6  0.4621      0.804 0.000 0.116 0.028 0.096 0.008 0.752
#&gt; SRR1812743     2  0.4453      0.589 0.000 0.552 0.000 0.424 0.012 0.012
#&gt; SRR1812742     5  0.5698      0.391 0.000 0.336 0.076 0.016 0.556 0.016
#&gt; SRR1812737     2  0.4179      0.578 0.000 0.516 0.000 0.472 0.012 0.000
#&gt; SRR1812735     2  0.6274      0.127 0.000 0.516 0.080 0.064 0.332 0.008
#&gt; SRR1812734     6  0.3558      0.782 0.000 0.052 0.132 0.004 0.004 0.808
#&gt; SRR1812733     4  0.4377      0.604 0.000 0.028 0.116 0.772 0.076 0.008
#&gt; SRR1812736     3  0.3915      0.618 0.000 0.004 0.692 0.000 0.288 0.016
#&gt; SRR1812732     2  0.5945      0.283 0.000 0.548 0.020 0.184 0.000 0.248
#&gt; SRR1812730     5  0.4576      0.544 0.000 0.008 0.228 0.024 0.708 0.032
#&gt; SRR1812731     2  0.4411      0.597 0.000 0.544 0.004 0.436 0.012 0.004
#&gt; SRR1812729     4  0.2345      0.736 0.000 0.016 0.016 0.896 0.072 0.000
#&gt; SRR1812727     6  0.1925      0.836 0.000 0.004 0.008 0.008 0.060 0.920
#&gt; SRR1812726     5  0.2833      0.751 0.000 0.088 0.008 0.004 0.868 0.032
#&gt; SRR1812728     5  0.1729      0.751 0.000 0.012 0.012 0.004 0.936 0.036
#&gt; SRR1812724     4  0.1387      0.764 0.000 0.068 0.000 0.932 0.000 0.000
#&gt; SRR1812725     5  0.1680      0.742 0.000 0.004 0.012 0.020 0.940 0.024
#&gt; SRR1812723     5  0.1707      0.765 0.000 0.056 0.000 0.004 0.928 0.012
#&gt; SRR1812722     5  0.3515      0.729 0.000 0.116 0.008 0.012 0.824 0.040
#&gt; SRR1812721     2  0.4513      0.604 0.000 0.572 0.000 0.396 0.028 0.004
#&gt; SRR1812718     5  0.3346      0.748 0.000 0.064 0.080 0.012 0.840 0.004
#&gt; SRR1812717     4  0.1075      0.760 0.000 0.048 0.000 0.952 0.000 0.000
#&gt; SRR1812716     5  0.3986      0.584 0.000 0.004 0.172 0.024 0.772 0.028
#&gt; SRR1812715     2  0.4952      0.600 0.000 0.568 0.004 0.376 0.044 0.008
#&gt; SRR1812714     6  0.3475      0.845 0.000 0.068 0.000 0.040 0.056 0.836
#&gt; SRR1812719     5  0.4931      0.497 0.000 0.024 0.096 0.000 0.692 0.188
#&gt; SRR1812713     4  0.0603      0.771 0.000 0.004 0.000 0.980 0.016 0.000
#&gt; SRR1812712     4  0.2112      0.764 0.000 0.088 0.000 0.896 0.016 0.000
#&gt; SRR1812711     5  0.3471      0.750 0.000 0.076 0.064 0.004 0.836 0.020
#&gt; SRR1812710     2  0.4778      0.589 0.000 0.492 0.000 0.464 0.040 0.004
#&gt; SRR1812709     4  0.2260      0.701 0.000 0.140 0.000 0.860 0.000 0.000
#&gt; SRR1812708     1  0.7060      0.526 0.440 0.192 0.264 0.000 0.000 0.104
#&gt; SRR1812707     2  0.4654      0.599 0.000 0.512 0.000 0.452 0.032 0.004
#&gt; SRR1812705     5  0.1707      0.765 0.000 0.056 0.000 0.004 0.928 0.012
#&gt; SRR1812706     5  0.4089      0.659 0.000 0.012 0.160 0.024 0.776 0.028
#&gt; SRR1812704     4  0.2959      0.739 0.000 0.032 0.012 0.876 0.056 0.024
#&gt; SRR1812703     6  0.3164      0.849 0.000 0.012 0.000 0.096 0.048 0.844
#&gt; SRR1812702     5  0.3986      0.584 0.000 0.004 0.172 0.024 0.772 0.028
#&gt; SRR1812741     2  0.7034      0.084 0.052 0.516 0.244 0.140 0.000 0.048
#&gt; SRR1812740     3  0.4631      0.494 0.000 0.020 0.596 0.004 0.368 0.012
#&gt; SRR1812739     4  0.3590      0.687 0.000 0.152 0.028 0.800 0.000 0.020
#&gt; SRR1812738     4  0.3955     -0.142 0.000 0.340 0.004 0.648 0.000 0.008
</code></pre>

<script>
$('#tab-CV-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-kmeans-get-classes-5-a').click(function(){
  $('#tab-CV-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-kmeans-signature_compare](figure_cola/CV-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-kmeans-collect-classes](figure_cola/CV-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:skmeans**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "skmeans"]
# you can also extract it by
# res = res_list["CV:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-skmeans-collect-plots](figure_cola/CV-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-skmeans-select-partition-number](figure_cola/CV-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.971       0.984         0.4312 0.576   0.576
#> 3 3 0.679           0.838       0.891         0.5598 0.721   0.527
#> 4 4 0.625           0.541       0.753         0.1079 0.900   0.713
#> 5 5 0.596           0.475       0.703         0.0701 0.865   0.556
#> 6 6 0.644           0.518       0.686         0.0438 0.899   0.560
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-classes'>
<ul>
<li><a href='#tab-CV-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-skmeans-get-classes-1'>
<p><a id='tab-CV-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812753     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812751     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812750     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812748     2  0.7528      0.735 0.216 0.784
#&gt; SRR1812749     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812746     1  0.0672      0.988 0.992 0.008
#&gt; SRR1812745     2  0.1633      0.968 0.024 0.976
#&gt; SRR1812747     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812744     1  0.0938      0.988 0.988 0.012
#&gt; SRR1812743     2  0.1843      0.967 0.028 0.972
#&gt; SRR1812742     2  0.2778      0.952 0.048 0.952
#&gt; SRR1812737     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812735     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812734     1  0.0672      0.988 0.992 0.008
#&gt; SRR1812733     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812736     2  0.7602      0.733 0.220 0.780
#&gt; SRR1812732     1  0.3274      0.942 0.940 0.060
#&gt; SRR1812730     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812731     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812729     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812727     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812726     2  0.0938      0.980 0.012 0.988
#&gt; SRR1812728     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812724     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812725     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812723     2  0.0672      0.981 0.008 0.992
#&gt; SRR1812722     2  0.0938      0.980 0.012 0.988
#&gt; SRR1812721     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812718     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812717     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812716     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812715     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812714     1  0.0376      0.990 0.996 0.004
#&gt; SRR1812719     1  0.0376      0.990 0.996 0.004
#&gt; SRR1812713     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812712     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812711     2  0.0938      0.980 0.012 0.988
#&gt; SRR1812710     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812709     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812708     1  0.0000      0.991 1.000 0.000
#&gt; SRR1812707     2  0.0672      0.980 0.008 0.992
#&gt; SRR1812705     2  0.0938      0.980 0.012 0.988
#&gt; SRR1812706     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812704     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812703     1  0.2043      0.971 0.968 0.032
#&gt; SRR1812702     2  0.0376      0.981 0.004 0.996
#&gt; SRR1812741     1  0.0376      0.990 0.996 0.004
#&gt; SRR1812740     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812739     2  0.0000      0.981 0.000 1.000
#&gt; SRR1812738     2  0.0376      0.981 0.004 0.996
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-1-a').click(function(){
  $('#tab-CV-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-2'>
<p><a id='tab-CV-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812748     2  0.6111      0.629 0.000 0.604 0.396
#&gt; SRR1812749     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812746     1  0.1031      0.957 0.976 0.000 0.024
#&gt; SRR1812745     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812747     3  0.4654      0.826 0.000 0.208 0.792
#&gt; SRR1812744     1  0.5554      0.806 0.812 0.076 0.112
#&gt; SRR1812743     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812742     3  0.5016      0.813 0.000 0.240 0.760
#&gt; SRR1812737     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812735     3  0.6305      0.444 0.000 0.484 0.516
#&gt; SRR1812734     1  0.1753      0.941 0.952 0.000 0.048
#&gt; SRR1812733     2  0.6286      0.500 0.000 0.536 0.464
#&gt; SRR1812736     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812732     2  0.2446      0.846 0.012 0.936 0.052
#&gt; SRR1812730     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812729     2  0.4002      0.834 0.000 0.840 0.160
#&gt; SRR1812727     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812726     3  0.5431      0.780 0.000 0.284 0.716
#&gt; SRR1812728     3  0.3030      0.834 0.004 0.092 0.904
#&gt; SRR1812724     2  0.4654      0.816 0.000 0.792 0.208
#&gt; SRR1812725     3  0.0424      0.828 0.000 0.008 0.992
#&gt; SRR1812723     3  0.4887      0.819 0.000 0.228 0.772
#&gt; SRR1812722     3  0.5754      0.766 0.004 0.296 0.700
#&gt; SRR1812721     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812718     3  0.4654      0.826 0.000 0.208 0.792
#&gt; SRR1812717     2  0.2625      0.851 0.000 0.916 0.084
#&gt; SRR1812716     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812714     1  0.0237      0.965 0.996 0.004 0.000
#&gt; SRR1812719     1  0.1031      0.953 0.976 0.000 0.024
#&gt; SRR1812713     2  0.5016      0.799 0.000 0.760 0.240
#&gt; SRR1812712     2  0.5016      0.799 0.000 0.760 0.240
#&gt; SRR1812711     3  0.5016      0.813 0.000 0.240 0.760
#&gt; SRR1812710     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812709     2  0.3619      0.843 0.000 0.864 0.136
#&gt; SRR1812708     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812707     2  0.0000      0.836 0.000 1.000 0.000
#&gt; SRR1812705     3  0.4974      0.816 0.000 0.236 0.764
#&gt; SRR1812706     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812704     2  0.5291      0.782 0.000 0.732 0.268
#&gt; SRR1812703     1  0.4261      0.846 0.848 0.012 0.140
#&gt; SRR1812702     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812741     1  0.0000      0.967 1.000 0.000 0.000
#&gt; SRR1812740     3  0.0000      0.827 0.000 0.000 1.000
#&gt; SRR1812739     2  0.5016      0.799 0.000 0.760 0.240
#&gt; SRR1812738     2  0.2537      0.851 0.000 0.920 0.080
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-2-a').click(function(){
  $('#tab-CV-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-3'>
<p><a id='tab-CV-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.7541    -0.3146 0.000 0.388 0.424 0.188
#&gt; SRR1812749     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.4711     0.5934 0.740 0.024 0.236 0.000
#&gt; SRR1812745     3  0.2271     0.5614 0.000 0.076 0.916 0.008
#&gt; SRR1812747     3  0.4624     0.5904 0.000 0.340 0.660 0.000
#&gt; SRR1812744     2  0.8943     0.2520 0.152 0.492 0.136 0.220
#&gt; SRR1812743     4  0.4454     0.6862 0.000 0.308 0.000 0.692
#&gt; SRR1812742     3  0.5396     0.4882 0.000 0.464 0.524 0.012
#&gt; SRR1812737     4  0.4164     0.6995 0.000 0.264 0.000 0.736
#&gt; SRR1812735     2  0.7559    -0.2914 0.000 0.460 0.336 0.204
#&gt; SRR1812734     2  0.8007     0.0867 0.224 0.480 0.280 0.016
#&gt; SRR1812733     4  0.6585     0.2543 0.000 0.104 0.312 0.584
#&gt; SRR1812736     3  0.2198     0.5660 0.000 0.072 0.920 0.008
#&gt; SRR1812732     2  0.6717    -0.0269 0.028 0.532 0.040 0.400
#&gt; SRR1812730     3  0.0672     0.6149 0.000 0.008 0.984 0.008
#&gt; SRR1812731     4  0.4193     0.7009 0.000 0.268 0.000 0.732
#&gt; SRR1812729     4  0.4966     0.5790 0.000 0.156 0.076 0.768
#&gt; SRR1812727     1  0.4643     0.5262 0.656 0.344 0.000 0.000
#&gt; SRR1812726     3  0.5273     0.5023 0.000 0.456 0.536 0.008
#&gt; SRR1812728     3  0.3710     0.6301 0.000 0.192 0.804 0.004
#&gt; SRR1812724     4  0.1302     0.7229 0.000 0.044 0.000 0.956
#&gt; SRR1812725     3  0.3636     0.6316 0.000 0.172 0.820 0.008
#&gt; SRR1812723     3  0.4898     0.5463 0.000 0.416 0.584 0.000
#&gt; SRR1812722     2  0.5512    -0.5595 0.000 0.496 0.488 0.016
#&gt; SRR1812721     4  0.4761     0.6167 0.000 0.372 0.000 0.628
#&gt; SRR1812718     3  0.4964     0.5694 0.000 0.380 0.616 0.004
#&gt; SRR1812717     4  0.0592     0.7295 0.000 0.016 0.000 0.984
#&gt; SRR1812716     3  0.0336     0.6190 0.000 0.000 0.992 0.008
#&gt; SRR1812715     4  0.4643     0.6521 0.000 0.344 0.000 0.656
#&gt; SRR1812714     1  0.6200     0.2819 0.504 0.444 0.000 0.052
#&gt; SRR1812719     1  0.5318     0.6243 0.732 0.072 0.196 0.000
#&gt; SRR1812713     4  0.1629     0.7161 0.000 0.024 0.024 0.952
#&gt; SRR1812712     4  0.2036     0.7139 0.000 0.032 0.032 0.936
#&gt; SRR1812711     3  0.5268     0.5072 0.000 0.452 0.540 0.008
#&gt; SRR1812710     4  0.4564     0.6576 0.000 0.328 0.000 0.672
#&gt; SRR1812709     4  0.1118     0.7321 0.000 0.036 0.000 0.964
#&gt; SRR1812708     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812707     4  0.4072     0.7048 0.000 0.252 0.000 0.748
#&gt; SRR1812705     3  0.5097     0.5335 0.000 0.428 0.568 0.004
#&gt; SRR1812706     3  0.1635     0.6287 0.000 0.044 0.948 0.008
#&gt; SRR1812704     4  0.3224     0.6760 0.000 0.016 0.120 0.864
#&gt; SRR1812703     2  0.8901     0.1339 0.244 0.440 0.068 0.248
#&gt; SRR1812702     3  0.0336     0.6190 0.000 0.000 0.992 0.008
#&gt; SRR1812741     1  0.0000     0.8449 1.000 0.000 0.000 0.000
#&gt; SRR1812740     3  0.2124     0.5714 0.000 0.068 0.924 0.008
#&gt; SRR1812739     4  0.4337     0.6006 0.000 0.140 0.052 0.808
#&gt; SRR1812738     4  0.1902     0.7319 0.000 0.064 0.004 0.932
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-3-a').click(function(){
  $('#tab-CV-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-4'>
<p><a id='tab-CV-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1   0.000     0.8922 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1   0.000     0.8922 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1   0.000     0.8922 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1   0.000     0.8922 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3   0.650     0.2826 0.000 0.024 0.564 0.148 0.264
#&gt; SRR1812749     1   0.000     0.8922 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     1   0.452     0.5614 0.724 0.012 0.236 0.000 0.028
#&gt; SRR1812745     2   0.670     0.3898 0.000 0.396 0.360 0.000 0.244
#&gt; SRR1812747     2   0.511     0.6562 0.000 0.728 0.040 0.052 0.180
#&gt; SRR1812744     3   0.477     0.4893 0.056 0.004 0.744 0.012 0.184
#&gt; SRR1812743     4   0.220     0.5651 0.000 0.008 0.016 0.916 0.060
#&gt; SRR1812742     2   0.659     0.4741 0.000 0.556 0.032 0.280 0.132
#&gt; SRR1812737     4   0.189     0.5517 0.000 0.004 0.000 0.916 0.080
#&gt; SRR1812735     4   0.614     0.1038 0.000 0.312 0.000 0.532 0.156
#&gt; SRR1812734     3   0.181     0.5183 0.060 0.000 0.928 0.000 0.012
#&gt; SRR1812733     5   0.576     0.3784 0.000 0.064 0.100 0.136 0.700
#&gt; SRR1812736     3   0.676    -0.4319 0.000 0.336 0.392 0.000 0.272
#&gt; SRR1812732     3   0.619     0.1605 0.004 0.000 0.496 0.376 0.124
#&gt; SRR1812730     2   0.635     0.5409 0.000 0.524 0.232 0.000 0.244
#&gt; SRR1812731     4   0.157     0.5620 0.000 0.000 0.004 0.936 0.060
#&gt; SRR1812729     5   0.615     0.4708 0.000 0.156 0.004 0.276 0.564
#&gt; SRR1812727     3   0.444    -0.0197 0.464 0.000 0.532 0.000 0.004
#&gt; SRR1812726     2   0.366     0.6232 0.000 0.836 0.024 0.108 0.032
#&gt; SRR1812728     2   0.358     0.6671 0.000 0.840 0.068 0.008 0.084
#&gt; SRR1812724     4   0.475    -0.5775 0.000 0.000 0.016 0.492 0.492
#&gt; SRR1812725     2   0.353     0.6672 0.000 0.832 0.072 0.000 0.096
#&gt; SRR1812723     2   0.198     0.6580 0.000 0.928 0.004 0.044 0.024
#&gt; SRR1812722     2   0.571     0.4772 0.000 0.668 0.048 0.224 0.060
#&gt; SRR1812721     4   0.357     0.5251 0.000 0.092 0.004 0.836 0.068
#&gt; SRR1812718     2   0.377     0.6644 0.000 0.812 0.008 0.036 0.144
#&gt; SRR1812717     5   0.445     0.4988 0.000 0.000 0.004 0.492 0.504
#&gt; SRR1812716     2   0.576     0.5824 0.000 0.620 0.192 0.000 0.188
#&gt; SRR1812715     4   0.293     0.5453 0.000 0.068 0.000 0.872 0.060
#&gt; SRR1812714     3   0.706     0.3774 0.244 0.040 0.588 0.060 0.068
#&gt; SRR1812719     1   0.697     0.2889 0.540 0.124 0.272 0.000 0.064
#&gt; SRR1812713     5   0.449     0.5503 0.000 0.000 0.008 0.420 0.572
#&gt; SRR1812712     5   0.466     0.5187 0.000 0.000 0.012 0.484 0.504
#&gt; SRR1812711     2   0.367     0.6461 0.000 0.828 0.004 0.100 0.068
#&gt; SRR1812710     4   0.273     0.5695 0.000 0.060 0.000 0.884 0.056
#&gt; SRR1812709     4   0.432    -0.3848 0.000 0.000 0.004 0.600 0.396
#&gt; SRR1812708     1   0.000     0.8922 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812707     4   0.177     0.5392 0.000 0.004 0.000 0.924 0.072
#&gt; SRR1812705     2   0.224     0.6510 0.000 0.912 0.004 0.064 0.020
#&gt; SRR1812706     2   0.576     0.6173 0.000 0.616 0.160 0.000 0.224
#&gt; SRR1812704     5   0.599     0.5103 0.000 0.064 0.024 0.368 0.544
#&gt; SRR1812703     3   0.676     0.3602 0.080 0.040 0.584 0.024 0.272
#&gt; SRR1812702     2   0.567     0.5894 0.000 0.632 0.188 0.000 0.180
#&gt; SRR1812741     1   0.029     0.8859 0.992 0.000 0.000 0.008 0.000
#&gt; SRR1812740     2   0.678     0.3774 0.000 0.380 0.340 0.000 0.280
#&gt; SRR1812739     5   0.614     0.4442 0.000 0.004 0.112 0.424 0.460
#&gt; SRR1812738     4   0.464    -0.2690 0.000 0.000 0.016 0.584 0.400
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-4-a').click(function(){
  $('#tab-CV-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-skmeans-get-classes-5'>
<p><a id='tab-CV-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000     0.8747 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.8747 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.8747 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.8747 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.6573    -0.0541 0.000 0.000 0.512 0.080 0.260 0.148
#&gt; SRR1812749     1  0.0000     0.8747 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.4478     0.5715 0.716 0.000 0.176 0.000 0.104 0.004
#&gt; SRR1812745     3  0.5044     0.5341 0.000 0.220 0.656 0.004 0.116 0.004
#&gt; SRR1812747     2  0.6047     0.3797 0.000 0.548 0.308 0.016 0.024 0.104
#&gt; SRR1812744     5  0.4948     0.6339 0.012 0.000 0.104 0.088 0.740 0.056
#&gt; SRR1812743     6  0.4550     0.5968 0.000 0.004 0.040 0.220 0.024 0.712
#&gt; SRR1812742     2  0.6443     0.2871 0.000 0.428 0.228 0.000 0.024 0.320
#&gt; SRR1812737     6  0.4154     0.6375 0.000 0.016 0.000 0.296 0.012 0.676
#&gt; SRR1812735     6  0.7016     0.1802 0.000 0.268 0.184 0.036 0.036 0.476
#&gt; SRR1812734     5  0.3056     0.6351 0.012 0.000 0.140 0.000 0.832 0.016
#&gt; SRR1812733     4  0.6085     0.3956 0.000 0.032 0.316 0.540 0.012 0.100
#&gt; SRR1812736     3  0.5487     0.5008 0.000 0.168 0.644 0.000 0.156 0.032
#&gt; SRR1812732     5  0.7147     0.2646 0.000 0.000 0.136 0.144 0.404 0.316
#&gt; SRR1812730     3  0.4433     0.4684 0.000 0.352 0.620 0.008 0.008 0.012
#&gt; SRR1812731     6  0.3913     0.6374 0.000 0.008 0.012 0.240 0.008 0.732
#&gt; SRR1812729     4  0.5733     0.4873 0.000 0.180 0.092 0.648 0.004 0.076
#&gt; SRR1812727     5  0.3684     0.4736 0.300 0.000 0.004 0.000 0.692 0.004
#&gt; SRR1812726     2  0.3528     0.5315 0.000 0.832 0.048 0.000 0.076 0.044
#&gt; SRR1812728     2  0.4313     0.2533 0.000 0.708 0.240 0.000 0.036 0.016
#&gt; SRR1812724     4  0.2971     0.6135 0.000 0.000 0.024 0.848 0.012 0.116
#&gt; SRR1812725     2  0.3725     0.1230 0.000 0.676 0.316 0.000 0.000 0.008
#&gt; SRR1812723     2  0.1531     0.5518 0.000 0.928 0.068 0.000 0.000 0.004
#&gt; SRR1812722     2  0.5817     0.4311 0.000 0.624 0.076 0.000 0.100 0.200
#&gt; SRR1812721     6  0.5765     0.5997 0.000 0.076 0.040 0.248 0.016 0.620
#&gt; SRR1812718     2  0.4482     0.4242 0.000 0.664 0.288 0.000 0.012 0.036
#&gt; SRR1812717     4  0.2703     0.5817 0.000 0.000 0.004 0.824 0.000 0.172
#&gt; SRR1812716     3  0.4220     0.3663 0.000 0.468 0.520 0.008 0.000 0.004
#&gt; SRR1812715     6  0.6388     0.5688 0.000 0.116 0.060 0.176 0.036 0.612
#&gt; SRR1812714     5  0.4457     0.6744 0.092 0.024 0.008 0.044 0.792 0.040
#&gt; SRR1812719     1  0.7686    -0.0880 0.380 0.180 0.128 0.000 0.292 0.020
#&gt; SRR1812713     4  0.1563     0.6310 0.000 0.000 0.012 0.932 0.000 0.056
#&gt; SRR1812712     4  0.2355     0.6164 0.000 0.000 0.004 0.876 0.008 0.112
#&gt; SRR1812711     2  0.4183     0.5454 0.000 0.764 0.148 0.000 0.020 0.068
#&gt; SRR1812710     6  0.4750     0.6555 0.000 0.064 0.000 0.260 0.012 0.664
#&gt; SRR1812709     4  0.3388     0.5041 0.000 0.004 0.004 0.764 0.004 0.224
#&gt; SRR1812708     1  0.0000     0.8747 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812707     6  0.4688     0.5855 0.000 0.020 0.012 0.352 0.008 0.608
#&gt; SRR1812705     2  0.1462     0.5640 0.000 0.936 0.056 0.000 0.000 0.008
#&gt; SRR1812706     3  0.4363     0.3273 0.000 0.400 0.580 0.004 0.008 0.008
#&gt; SRR1812704     4  0.5821     0.4859 0.000 0.036 0.172 0.628 0.008 0.156
#&gt; SRR1812703     5  0.4659     0.6189 0.040 0.008 0.028 0.176 0.740 0.008
#&gt; SRR1812702     3  0.3982     0.3803 0.000 0.460 0.536 0.000 0.000 0.004
#&gt; SRR1812741     1  0.0291     0.8698 0.992 0.000 0.000 0.000 0.004 0.004
#&gt; SRR1812740     3  0.5303     0.5417 0.000 0.204 0.668 0.012 0.096 0.020
#&gt; SRR1812739     4  0.5905     0.4950 0.000 0.000 0.116 0.632 0.104 0.148
#&gt; SRR1812738     4  0.5643     0.0952 0.000 0.004 0.040 0.488 0.048 0.420
</code></pre>

<script>
$('#tab-CV-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-skmeans-get-classes-5-a').click(function(){
  $('#tab-CV-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-CV-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-skmeans-signature_compare](figure_cola/CV-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-CV-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-skmeans-collect-classes](figure_cola/CV-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:pam**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "pam"]
# you can also extract it by
# res = res_list["CV:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-pam-collect-plots](figure_cola/CV-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-pam-select-partition-number](figure_cola/CV-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.991       0.996         0.2188 0.788   0.788
#> 3 3 0.528           0.723       0.885         1.7081 0.613   0.508
#> 4 4 0.516           0.660       0.808         0.1673 0.848   0.634
#> 5 5 0.535           0.682       0.798         0.0514 0.918   0.745
#> 6 6 0.621           0.578       0.809         0.0574 0.927   0.751
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-classes'>
<ul>
<li><a href='#tab-CV-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-pam-get-classes-1'>
<p><a id='tab-CV-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000      1.000 1.000 0.000
#&gt; SRR1812753     1   0.000      1.000 1.000 0.000
#&gt; SRR1812751     1   0.000      1.000 1.000 0.000
#&gt; SRR1812750     1   0.000      1.000 1.000 0.000
#&gt; SRR1812748     2   0.000      0.995 0.000 1.000
#&gt; SRR1812749     1   0.000      1.000 1.000 0.000
#&gt; SRR1812746     2   0.000      0.995 0.000 1.000
#&gt; SRR1812745     2   0.000      0.995 0.000 1.000
#&gt; SRR1812747     2   0.000      0.995 0.000 1.000
#&gt; SRR1812744     2   0.000      0.995 0.000 1.000
#&gt; SRR1812743     2   0.000      0.995 0.000 1.000
#&gt; SRR1812742     2   0.000      0.995 0.000 1.000
#&gt; SRR1812737     2   0.000      0.995 0.000 1.000
#&gt; SRR1812735     2   0.000      0.995 0.000 1.000
#&gt; SRR1812734     2   0.000      0.995 0.000 1.000
#&gt; SRR1812733     2   0.000      0.995 0.000 1.000
#&gt; SRR1812736     2   0.000      0.995 0.000 1.000
#&gt; SRR1812732     2   0.000      0.995 0.000 1.000
#&gt; SRR1812730     2   0.000      0.995 0.000 1.000
#&gt; SRR1812731     2   0.000      0.995 0.000 1.000
#&gt; SRR1812729     2   0.000      0.995 0.000 1.000
#&gt; SRR1812727     2   0.000      0.995 0.000 1.000
#&gt; SRR1812726     2   0.000      0.995 0.000 1.000
#&gt; SRR1812728     2   0.000      0.995 0.000 1.000
#&gt; SRR1812724     2   0.000      0.995 0.000 1.000
#&gt; SRR1812725     2   0.000      0.995 0.000 1.000
#&gt; SRR1812723     2   0.000      0.995 0.000 1.000
#&gt; SRR1812722     2   0.000      0.995 0.000 1.000
#&gt; SRR1812721     2   0.000      0.995 0.000 1.000
#&gt; SRR1812718     2   0.000      0.995 0.000 1.000
#&gt; SRR1812717     2   0.000      0.995 0.000 1.000
#&gt; SRR1812716     2   0.000      0.995 0.000 1.000
#&gt; SRR1812715     2   0.000      0.995 0.000 1.000
#&gt; SRR1812714     2   0.000      0.995 0.000 1.000
#&gt; SRR1812719     2   0.000      0.995 0.000 1.000
#&gt; SRR1812713     2   0.000      0.995 0.000 1.000
#&gt; SRR1812712     2   0.000      0.995 0.000 1.000
#&gt; SRR1812711     2   0.000      0.995 0.000 1.000
#&gt; SRR1812710     2   0.000      0.995 0.000 1.000
#&gt; SRR1812709     2   0.000      0.995 0.000 1.000
#&gt; SRR1812708     1   0.000      1.000 1.000 0.000
#&gt; SRR1812707     2   0.000      0.995 0.000 1.000
#&gt; SRR1812705     2   0.000      0.995 0.000 1.000
#&gt; SRR1812706     2   0.000      0.995 0.000 1.000
#&gt; SRR1812704     2   0.000      0.995 0.000 1.000
#&gt; SRR1812703     2   0.000      0.995 0.000 1.000
#&gt; SRR1812702     2   0.000      0.995 0.000 1.000
#&gt; SRR1812741     2   0.745      0.731 0.212 0.788
#&gt; SRR1812740     2   0.000      0.995 0.000 1.000
#&gt; SRR1812739     2   0.000      0.995 0.000 1.000
#&gt; SRR1812738     2   0.000      0.995 0.000 1.000
</code></pre>

<script>
$('#tab-CV-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-1-a').click(function(){
  $('#tab-CV-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-2'>
<p><a id='tab-CV-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000     1.0000 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     1.0000 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     1.0000 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     1.0000 1.000 0.000 0.000
#&gt; SRR1812748     3  0.4002     0.7078 0.000 0.160 0.840
#&gt; SRR1812749     1  0.0000     1.0000 1.000 0.000 0.000
#&gt; SRR1812746     3  0.0237     0.8383 0.000 0.004 0.996
#&gt; SRR1812745     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812747     2  0.6295     0.1823 0.000 0.528 0.472
#&gt; SRR1812744     2  0.6295     0.1478 0.000 0.528 0.472
#&gt; SRR1812743     2  0.1529     0.8224 0.000 0.960 0.040
#&gt; SRR1812742     2  0.6168     0.3563 0.000 0.588 0.412
#&gt; SRR1812737     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812735     2  0.4796     0.6950 0.000 0.780 0.220
#&gt; SRR1812734     3  0.2959     0.7740 0.000 0.100 0.900
#&gt; SRR1812733     3  0.4452     0.6664 0.000 0.192 0.808
#&gt; SRR1812736     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812732     2  0.1860     0.8186 0.000 0.948 0.052
#&gt; SRR1812730     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812731     2  0.2711     0.8085 0.000 0.912 0.088
#&gt; SRR1812729     2  0.4178     0.7557 0.000 0.828 0.172
#&gt; SRR1812727     3  0.5560     0.4863 0.000 0.300 0.700
#&gt; SRR1812726     3  0.6180     0.1621 0.000 0.416 0.584
#&gt; SRR1812728     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812724     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812725     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812723     3  0.4654     0.6420 0.000 0.208 0.792
#&gt; SRR1812722     2  0.6307     0.1140 0.000 0.512 0.488
#&gt; SRR1812721     2  0.4178     0.7531 0.000 0.828 0.172
#&gt; SRR1812718     2  0.6192     0.3369 0.000 0.580 0.420
#&gt; SRR1812717     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812716     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812714     2  0.3879     0.7583 0.000 0.848 0.152
#&gt; SRR1812719     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812713     2  0.0237     0.8222 0.000 0.996 0.004
#&gt; SRR1812712     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812711     3  0.6252     0.0599 0.000 0.444 0.556
#&gt; SRR1812710     2  0.0747     0.8237 0.000 0.984 0.016
#&gt; SRR1812709     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812708     1  0.0000     1.0000 1.000 0.000 0.000
#&gt; SRR1812707     2  0.1753     0.8209 0.000 0.952 0.048
#&gt; SRR1812705     3  0.5882     0.3628 0.000 0.348 0.652
#&gt; SRR1812706     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812704     2  0.4750     0.6469 0.000 0.784 0.216
#&gt; SRR1812703     2  0.4062     0.7182 0.000 0.836 0.164
#&gt; SRR1812702     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812741     2  0.0424     0.8215 0.008 0.992 0.000
#&gt; SRR1812740     3  0.0000     0.8402 0.000 0.000 1.000
#&gt; SRR1812739     2  0.0000     0.8226 0.000 1.000 0.000
#&gt; SRR1812738     2  0.2537     0.8108 0.000 0.920 0.080
</code></pre>

<script>
$('#tab-CV-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-2-a').click(function(){
  $('#tab-CV-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-3'>
<p><a id='tab-CV-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     0.9138 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.9138 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.9138 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.3726     0.9138 0.788 0.212 0.000 0.000
#&gt; SRR1812748     3  0.2805     0.7262 0.000 0.012 0.888 0.100
#&gt; SRR1812749     1  0.3726     0.9138 0.788 0.212 0.000 0.000
#&gt; SRR1812746     3  0.2081     0.7624 0.000 0.084 0.916 0.000
#&gt; SRR1812745     3  0.0469     0.7798 0.000 0.012 0.988 0.000
#&gt; SRR1812747     2  0.6315     0.2837 0.000 0.540 0.396 0.064
#&gt; SRR1812744     3  0.6660    -0.0359 0.000 0.084 0.464 0.452
#&gt; SRR1812743     4  0.1211     0.8067 0.000 0.000 0.040 0.960
#&gt; SRR1812742     2  0.5906     0.2394 0.000 0.528 0.436 0.036
#&gt; SRR1812737     4  0.0000     0.7948 0.000 0.000 0.000 1.000
#&gt; SRR1812735     2  0.6362     0.6471 0.000 0.656 0.176 0.168
#&gt; SRR1812734     3  0.4633     0.6182 0.000 0.172 0.780 0.048
#&gt; SRR1812733     3  0.2704     0.7118 0.000 0.000 0.876 0.124
#&gt; SRR1812736     3  0.0707     0.7781 0.000 0.020 0.980 0.000
#&gt; SRR1812732     2  0.5865     0.6507 0.000 0.612 0.048 0.340
#&gt; SRR1812730     3  0.0000     0.7796 0.000 0.000 1.000 0.000
#&gt; SRR1812731     4  0.2149     0.7891 0.000 0.000 0.088 0.912
#&gt; SRR1812729     4  0.3142     0.7450 0.000 0.008 0.132 0.860
#&gt; SRR1812727     3  0.5972     0.4590 0.000 0.068 0.640 0.292
#&gt; SRR1812726     3  0.7259     0.2834 0.000 0.156 0.492 0.352
#&gt; SRR1812728     3  0.1940     0.7672 0.000 0.076 0.924 0.000
#&gt; SRR1812724     2  0.4967     0.5172 0.000 0.548 0.000 0.452
#&gt; SRR1812725     3  0.1557     0.7652 0.000 0.056 0.944 0.000
#&gt; SRR1812723     3  0.5655     0.6005 0.000 0.084 0.704 0.212
#&gt; SRR1812722     2  0.5228     0.4456 0.000 0.664 0.312 0.024
#&gt; SRR1812721     4  0.4589     0.6448 0.000 0.048 0.168 0.784
#&gt; SRR1812718     4  0.6605    -0.0561 0.000 0.080 0.440 0.480
#&gt; SRR1812717     2  0.4843     0.5990 0.000 0.604 0.000 0.396
#&gt; SRR1812716     3  0.1118     0.7759 0.000 0.036 0.964 0.000
#&gt; SRR1812715     2  0.4643     0.6410 0.000 0.656 0.000 0.344
#&gt; SRR1812714     2  0.6167     0.6672 0.000 0.648 0.096 0.256
#&gt; SRR1812719     3  0.1302     0.7734 0.000 0.044 0.956 0.000
#&gt; SRR1812713     4  0.0188     0.7963 0.000 0.000 0.004 0.996
#&gt; SRR1812712     4  0.2814     0.6720 0.000 0.132 0.000 0.868
#&gt; SRR1812711     3  0.6510     0.2749 0.000 0.080 0.540 0.380
#&gt; SRR1812710     4  0.0592     0.8029 0.000 0.000 0.016 0.984
#&gt; SRR1812709     4  0.0000     0.7948 0.000 0.000 0.000 1.000
#&gt; SRR1812708     1  0.3726     0.9138 0.788 0.212 0.000 0.000
#&gt; SRR1812707     4  0.1389     0.8053 0.000 0.000 0.048 0.952
#&gt; SRR1812705     3  0.6700     0.4264 0.000 0.112 0.572 0.316
#&gt; SRR1812706     3  0.0469     0.7786 0.000 0.012 0.988 0.000
#&gt; SRR1812704     4  0.3486     0.6384 0.000 0.000 0.188 0.812
#&gt; SRR1812703     4  0.5816     0.4937 0.000 0.144 0.148 0.708
#&gt; SRR1812702     3  0.0336     0.7799 0.000 0.008 0.992 0.000
#&gt; SRR1812741     2  0.4643     0.6282 0.000 0.656 0.000 0.344
#&gt; SRR1812740     3  0.0707     0.7781 0.000 0.020 0.980 0.000
#&gt; SRR1812739     4  0.1118     0.7780 0.000 0.036 0.000 0.964
#&gt; SRR1812738     4  0.2011     0.7932 0.000 0.000 0.080 0.920
</code></pre>

<script>
$('#tab-CV-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-3-a').click(function(){
  $('#tab-CV-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-4'>
<p><a id='tab-CV-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.3707     1.0000 0.716 0.000 0.284 0.000 0.000
#&gt; SRR1812753     1  0.3707     1.0000 0.716 0.000 0.284 0.000 0.000
#&gt; SRR1812751     1  0.3707     1.0000 0.716 0.000 0.284 0.000 0.000
#&gt; SRR1812750     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812748     5  0.1885     0.7231 0.020 0.004 0.000 0.044 0.932
#&gt; SRR1812749     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812746     5  0.3610     0.7022 0.056 0.048 0.044 0.000 0.852
#&gt; SRR1812745     5  0.0771     0.7378 0.020 0.004 0.000 0.000 0.976
#&gt; SRR1812747     2  0.7205    -0.0997 0.156 0.416 0.000 0.044 0.384
#&gt; SRR1812744     4  0.6274     0.0996 0.012 0.104 0.000 0.456 0.428
#&gt; SRR1812743     4  0.0609     0.8336 0.000 0.000 0.000 0.980 0.020
#&gt; SRR1812742     5  0.6302     0.1204 0.136 0.384 0.000 0.004 0.476
#&gt; SRR1812737     4  0.0000     0.8300 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812735     2  0.4096     0.6821 0.000 0.772 0.000 0.052 0.176
#&gt; SRR1812734     5  0.4652     0.5662 0.056 0.188 0.000 0.012 0.744
#&gt; SRR1812733     5  0.1764     0.7170 0.008 0.000 0.000 0.064 0.928
#&gt; SRR1812736     5  0.1012     0.7359 0.020 0.012 0.000 0.000 0.968
#&gt; SRR1812732     2  0.4527     0.7310 0.000 0.732 0.000 0.204 0.064
#&gt; SRR1812730     5  0.0000     0.7392 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812731     4  0.1341     0.8206 0.000 0.000 0.000 0.944 0.056
#&gt; SRR1812729     4  0.2573     0.7714 0.016 0.000 0.000 0.880 0.104
#&gt; SRR1812727     5  0.6689     0.3323 0.104 0.048 0.000 0.308 0.540
#&gt; SRR1812726     5  0.7888     0.4394 0.264 0.112 0.000 0.184 0.440
#&gt; SRR1812728     5  0.4552     0.6614 0.264 0.040 0.000 0.000 0.696
#&gt; SRR1812724     2  0.4015     0.5858 0.000 0.652 0.000 0.348 0.000
#&gt; SRR1812725     5  0.3940     0.6640 0.220 0.024 0.000 0.000 0.756
#&gt; SRR1812723     5  0.6814     0.5662 0.264 0.048 0.000 0.136 0.552
#&gt; SRR1812722     2  0.4325     0.5360 0.036 0.724 0.000 0.000 0.240
#&gt; SRR1812721     4  0.4101     0.6218 0.000 0.048 0.000 0.768 0.184
#&gt; SRR1812718     5  0.7338     0.4213 0.220 0.048 0.000 0.256 0.476
#&gt; SRR1812717     2  0.3684     0.6662 0.000 0.720 0.000 0.280 0.000
#&gt; SRR1812716     5  0.1661     0.7317 0.036 0.024 0.000 0.000 0.940
#&gt; SRR1812715     2  0.3461     0.7108 0.000 0.772 0.000 0.224 0.004
#&gt; SRR1812714     2  0.4361     0.7250 0.000 0.768 0.000 0.124 0.108
#&gt; SRR1812719     5  0.2228     0.7241 0.048 0.040 0.000 0.000 0.912
#&gt; SRR1812713     4  0.0000     0.8300 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812712     4  0.2852     0.6720 0.000 0.172 0.000 0.828 0.000
#&gt; SRR1812711     5  0.7061     0.4890 0.220 0.048 0.000 0.200 0.532
#&gt; SRR1812710     4  0.0162     0.8317 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812709     4  0.0000     0.8300 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812708     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812707     4  0.0510     0.8334 0.000 0.000 0.000 0.984 0.016
#&gt; SRR1812705     5  0.7300     0.5256 0.264 0.072 0.000 0.156 0.508
#&gt; SRR1812706     5  0.0404     0.7389 0.000 0.012 0.000 0.000 0.988
#&gt; SRR1812704     4  0.2732     0.7141 0.000 0.000 0.000 0.840 0.160
#&gt; SRR1812703     4  0.5664     0.4867 0.004 0.180 0.000 0.648 0.168
#&gt; SRR1812702     5  0.0963     0.7406 0.036 0.000 0.000 0.000 0.964
#&gt; SRR1812741     2  0.1851     0.6425 0.000 0.912 0.000 0.088 0.000
#&gt; SRR1812740     5  0.1012     0.7359 0.020 0.012 0.000 0.000 0.968
#&gt; SRR1812739     4  0.1121     0.8105 0.000 0.044 0.000 0.956 0.000
#&gt; SRR1812738     4  0.1121     0.8268 0.000 0.000 0.000 0.956 0.044
</code></pre>

<script>
$('#tab-CV-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-4-a').click(function(){
  $('#tab-CV-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-pam-get-classes-5'>
<p><a id='tab-CV-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette p1    p2    p3    p4    p5   p6
#&gt; SRR1812752     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.00
#&gt; SRR1812753     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.00
#&gt; SRR1812751     1  0.0000      1.000  1 0.000 0.000 0.000 0.000 0.00
#&gt; SRR1812750     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.00
#&gt; SRR1812748     5  0.1426      0.540  0 0.008 0.028 0.016 0.948 0.00
#&gt; SRR1812749     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.00
#&gt; SRR1812746     5  0.3630      0.390  0 0.004 0.176 0.000 0.780 0.04
#&gt; SRR1812745     5  0.0790      0.544  0 0.000 0.032 0.000 0.968 0.00
#&gt; SRR1812747     5  0.6481     -0.329  0 0.360 0.252 0.020 0.368 0.00
#&gt; SRR1812744     4  0.6403      0.115  0 0.152 0.040 0.412 0.396 0.00
#&gt; SRR1812743     4  0.0260      0.836  0 0.000 0.000 0.992 0.008 0.00
#&gt; SRR1812742     5  0.5918     -0.280  0 0.276 0.224 0.004 0.496 0.00
#&gt; SRR1812737     4  0.0000      0.834  0 0.000 0.000 1.000 0.000 0.00
#&gt; SRR1812735     2  0.2340      0.697  0 0.852 0.000 0.000 0.148 0.00
#&gt; SRR1812734     5  0.5348      0.259  0 0.216 0.192 0.000 0.592 0.00
#&gt; SRR1812733     5  0.1257      0.539  0 0.000 0.020 0.028 0.952 0.00
#&gt; SRR1812736     5  0.1074      0.543  0 0.012 0.028 0.000 0.960 0.00
#&gt; SRR1812732     2  0.2350      0.766  0 0.880 0.000 0.100 0.020 0.00
#&gt; SRR1812730     5  0.0547      0.533  0 0.000 0.020 0.000 0.980 0.00
#&gt; SRR1812731     4  0.1007      0.825  0 0.000 0.000 0.956 0.044 0.00
#&gt; SRR1812729     4  0.2301      0.778  0 0.000 0.020 0.884 0.096 0.00
#&gt; SRR1812727     5  0.7038      0.143  0 0.116 0.252 0.176 0.456 0.00
#&gt; SRR1812726     3  0.4780      0.906  0 0.032 0.588 0.016 0.364 0.00
#&gt; SRR1812728     3  0.3774      0.934  0 0.000 0.592 0.000 0.408 0.00
#&gt; SRR1812724     2  0.3371      0.653  0 0.708 0.000 0.292 0.000 0.00
#&gt; SRR1812725     5  0.4449     -0.594  0 0.028 0.440 0.000 0.532 0.00
#&gt; SRR1812723     3  0.3899      0.940  0 0.000 0.592 0.004 0.404 0.00
#&gt; SRR1812722     2  0.4983      0.418  0 0.644 0.148 0.000 0.208 0.00
#&gt; SRR1812721     4  0.3807      0.629  0 0.052 0.000 0.756 0.192 0.00
#&gt; SRR1812718     5  0.5278     -0.539  0 0.048 0.432 0.024 0.496 0.00
#&gt; SRR1812717     2  0.2697      0.745  0 0.812 0.000 0.188 0.000 0.00
#&gt; SRR1812716     5  0.2135      0.449  0 0.000 0.128 0.000 0.872 0.00
#&gt; SRR1812715     2  0.2219      0.767  0 0.864 0.000 0.136 0.000 0.00
#&gt; SRR1812714     2  0.1176      0.741  0 0.956 0.000 0.024 0.020 0.00
#&gt; SRR1812719     5  0.2562      0.397  0 0.000 0.172 0.000 0.828 0.00
#&gt; SRR1812713     4  0.0000      0.834  0 0.000 0.000 1.000 0.000 0.00
#&gt; SRR1812712     4  0.2912      0.659  0 0.216 0.000 0.784 0.000 0.00
#&gt; SRR1812711     5  0.5225     -0.540  0 0.044 0.432 0.024 0.500 0.00
#&gt; SRR1812710     4  0.0146      0.835  0 0.000 0.000 0.996 0.004 0.00
#&gt; SRR1812709     4  0.0363      0.831  0 0.012 0.000 0.988 0.000 0.00
#&gt; SRR1812708     6  0.0000      1.000  0 0.000 0.000 0.000 0.000 1.00
#&gt; SRR1812707     4  0.0146      0.835  0 0.000 0.000 0.996 0.004 0.00
#&gt; SRR1812705     3  0.4343      0.939  0 0.020 0.592 0.004 0.384 0.00
#&gt; SRR1812706     5  0.0622      0.538  0 0.012 0.008 0.000 0.980 0.00
#&gt; SRR1812704     4  0.2932      0.723  0 0.016 0.000 0.820 0.164 0.00
#&gt; SRR1812703     4  0.6040      0.416  0 0.288 0.036 0.540 0.136 0.00
#&gt; SRR1812702     5  0.1444      0.481  0 0.000 0.072 0.000 0.928 0.00
#&gt; SRR1812741     2  0.3547      0.579  0 0.668 0.332 0.000 0.000 0.00
#&gt; SRR1812740     5  0.1074      0.545  0 0.012 0.028 0.000 0.960 0.00
#&gt; SRR1812739     4  0.1663      0.795  0 0.088 0.000 0.912 0.000 0.00
#&gt; SRR1812738     4  0.0547      0.833  0 0.000 0.000 0.980 0.020 0.00
</code></pre>

<script>
$('#tab-CV-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-pam-get-classes-5-a').click(function(){
  $('#tab-CV-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-pam-membership-heatmap'>
<ul>
<li><a href='#tab-CV-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-pam-signature_compare](figure_cola/CV-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-pam-dimension-reduction'>
<ul>
<li><a href='#tab-CV-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-pam-collect-classes](figure_cola/CV-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "mclust"]
# you can also extract it by
# res = res_list["CV:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-mclust-collect-plots](figure_cola/CV-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-mclust-select-partition-number](figure_cola/CV-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.844           0.873       0.940          0.358 0.594   0.594
#> 3 3 0.401           0.603       0.741          0.589 0.755   0.588
#> 4 4 0.337           0.460       0.672          0.153 0.789   0.495
#> 5 5 0.394           0.511       0.630          0.103 0.869   0.628
#> 6 6 0.549           0.421       0.629          0.076 0.868   0.557
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-classes'>
<ul>
<li><a href='#tab-CV-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-mclust-get-classes-1'>
<p><a id='tab-CV-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812753     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812751     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812750     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812748     2  0.2423     0.9357 0.040 0.960
#&gt; SRR1812749     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812746     1  0.9358     0.5968 0.648 0.352
#&gt; SRR1812745     2  0.0376     0.9789 0.004 0.996
#&gt; SRR1812747     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812744     1  0.9944     0.4384 0.544 0.456
#&gt; SRR1812743     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812742     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812737     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812735     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812734     1  0.9850     0.4993 0.572 0.428
#&gt; SRR1812733     2  0.0376     0.9788 0.004 0.996
#&gt; SRR1812736     2  0.0938     0.9707 0.012 0.988
#&gt; SRR1812732     2  0.9732    -0.0121 0.404 0.596
#&gt; SRR1812730     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812731     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812729     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812727     1  0.1414     0.7837 0.980 0.020
#&gt; SRR1812726     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812728     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812724     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812725     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812723     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812722     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812721     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812718     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812717     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812716     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812715     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812714     1  0.9850     0.4993 0.572 0.428
#&gt; SRR1812719     1  0.9608     0.5631 0.616 0.384
#&gt; SRR1812713     2  0.1184     0.9659 0.016 0.984
#&gt; SRR1812712     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812711     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812710     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812709     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812708     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812707     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812705     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812706     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812704     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812703     1  0.9993     0.3636 0.516 0.484
#&gt; SRR1812702     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812741     1  0.0000     0.7883 1.000 0.000
#&gt; SRR1812740     2  0.0000     0.9825 0.000 1.000
#&gt; SRR1812739     2  0.0672     0.9750 0.008 0.992
#&gt; SRR1812738     2  0.0000     0.9825 0.000 1.000
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-1-a').click(function(){
  $('#tab-CV-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-2'>
<p><a id='tab-CV-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000     0.7641 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.7641 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.7641 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.7641 1.000 0.000 0.000
#&gt; SRR1812748     2  0.5036     0.5874 0.048 0.832 0.120
#&gt; SRR1812749     1  0.1163     0.7658 0.972 0.000 0.028
#&gt; SRR1812746     1  0.6451     0.7086 0.560 0.004 0.436
#&gt; SRR1812745     3  0.6244     0.7481 0.000 0.440 0.560
#&gt; SRR1812747     2  0.6286    -0.6810 0.000 0.536 0.464
#&gt; SRR1812744     1  0.9722     0.5567 0.444 0.312 0.244
#&gt; SRR1812743     2  0.0424     0.7591 0.000 0.992 0.008
#&gt; SRR1812742     3  0.5968     0.7642 0.000 0.364 0.636
#&gt; SRR1812737     2  0.0424     0.7591 0.000 0.992 0.008
#&gt; SRR1812735     2  0.4002     0.4941 0.000 0.840 0.160
#&gt; SRR1812734     1  0.7262     0.7068 0.528 0.028 0.444
#&gt; SRR1812733     2  0.4605     0.4878 0.000 0.796 0.204
#&gt; SRR1812736     3  0.6460     0.7457 0.004 0.440 0.556
#&gt; SRR1812732     2  0.8604     0.0993 0.312 0.564 0.124
#&gt; SRR1812730     3  0.6291     0.7405 0.000 0.468 0.532
#&gt; SRR1812731     2  0.0424     0.7591 0.000 0.992 0.008
#&gt; SRR1812729     2  0.2448     0.6725 0.000 0.924 0.076
#&gt; SRR1812727     1  0.8046     0.7090 0.536 0.068 0.396
#&gt; SRR1812726     3  0.6209     0.7765 0.004 0.368 0.628
#&gt; SRR1812728     3  0.6308     0.7163 0.000 0.492 0.508
#&gt; SRR1812724     2  0.0000     0.7591 0.000 1.000 0.000
#&gt; SRR1812725     2  0.6291    -0.6834 0.000 0.532 0.468
#&gt; SRR1812723     3  0.5948     0.7719 0.000 0.360 0.640
#&gt; SRR1812722     3  0.6667     0.7691 0.016 0.368 0.616
#&gt; SRR1812721     2  0.0000     0.7591 0.000 1.000 0.000
#&gt; SRR1812718     2  0.6252    -0.6411 0.000 0.556 0.444
#&gt; SRR1812717     2  0.0592     0.7560 0.000 0.988 0.012
#&gt; SRR1812716     3  0.6516     0.7282 0.004 0.480 0.516
#&gt; SRR1812715     2  0.0424     0.7591 0.000 0.992 0.008
#&gt; SRR1812714     1  0.8663     0.7101 0.524 0.112 0.364
#&gt; SRR1812719     1  0.7276     0.7147 0.564 0.032 0.404
#&gt; SRR1812713     2  0.1950     0.7161 0.008 0.952 0.040
#&gt; SRR1812712     2  0.0237     0.7582 0.000 0.996 0.004
#&gt; SRR1812711     3  0.5882     0.7649 0.000 0.348 0.652
#&gt; SRR1812710     2  0.0424     0.7591 0.000 0.992 0.008
#&gt; SRR1812709     2  0.0000     0.7591 0.000 1.000 0.000
#&gt; SRR1812708     1  0.1163     0.7658 0.972 0.000 0.028
#&gt; SRR1812707     2  0.0000     0.7591 0.000 1.000 0.000
#&gt; SRR1812705     3  0.5988     0.7733 0.000 0.368 0.632
#&gt; SRR1812706     3  0.6509     0.7256 0.004 0.472 0.524
#&gt; SRR1812704     2  0.1411     0.7289 0.000 0.964 0.036
#&gt; SRR1812703     1  0.9970     0.3562 0.356 0.296 0.348
#&gt; SRR1812702     3  0.6516     0.7282 0.004 0.480 0.516
#&gt; SRR1812741     1  0.8834     0.6964 0.580 0.196 0.224
#&gt; SRR1812740     2  0.6345    -0.4052 0.004 0.596 0.400
#&gt; SRR1812739     2  0.0661     0.7556 0.004 0.988 0.008
#&gt; SRR1812738     2  0.0424     0.7587 0.000 0.992 0.008
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-2-a').click(function(){
  $('#tab-CV-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-3'>
<p><a id='tab-CV-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000    0.90697 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000    0.90697 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000    0.90697 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.2984    0.91113 0.888 0.084 0.028 0.000
#&gt; SRR1812748     4  0.6124    0.19275 0.000 0.084 0.276 0.640
#&gt; SRR1812749     1  0.2984    0.91113 0.888 0.084 0.028 0.000
#&gt; SRR1812746     3  0.9673   -0.00856 0.260 0.180 0.372 0.188
#&gt; SRR1812745     3  0.5695    0.57010 0.000 0.040 0.624 0.336
#&gt; SRR1812747     3  0.7787    0.24532 0.000 0.244 0.384 0.372
#&gt; SRR1812744     3  0.8877   -0.03373 0.044 0.312 0.324 0.320
#&gt; SRR1812743     4  0.3710    0.66425 0.000 0.004 0.192 0.804
#&gt; SRR1812742     3  0.7894   -0.18367 0.000 0.304 0.376 0.320
#&gt; SRR1812737     4  0.3668    0.66309 0.000 0.004 0.188 0.808
#&gt; SRR1812735     4  0.5062    0.21989 0.000 0.020 0.300 0.680
#&gt; SRR1812734     2  0.7758   -0.05345 0.008 0.472 0.328 0.192
#&gt; SRR1812733     4  0.5245    0.25769 0.004 0.016 0.320 0.660
#&gt; SRR1812736     3  0.5954    0.56653 0.000 0.052 0.604 0.344
#&gt; SRR1812732     4  0.7598    0.24961 0.028 0.232 0.164 0.576
#&gt; SRR1812730     3  0.4730    0.57736 0.000 0.000 0.636 0.364
#&gt; SRR1812731     4  0.3710    0.66425 0.000 0.004 0.192 0.804
#&gt; SRR1812729     4  0.3612    0.59408 0.000 0.100 0.044 0.856
#&gt; SRR1812727     2  0.8639   -0.03026 0.064 0.476 0.272 0.188
#&gt; SRR1812726     2  0.7650    0.20485 0.000 0.424 0.364 0.212
#&gt; SRR1812728     3  0.6930    0.46962 0.000 0.120 0.524 0.356
#&gt; SRR1812724     4  0.1256    0.66263 0.000 0.008 0.028 0.964
#&gt; SRR1812725     3  0.6819    0.50182 0.000 0.112 0.540 0.348
#&gt; SRR1812723     2  0.7638    0.20726 0.000 0.420 0.372 0.208
#&gt; SRR1812722     2  0.7762    0.20504 0.004 0.436 0.356 0.204
#&gt; SRR1812721     4  0.3710    0.66718 0.000 0.004 0.192 0.804
#&gt; SRR1812718     3  0.7782    0.27311 0.000 0.244 0.396 0.360
#&gt; SRR1812717     4  0.2256    0.66485 0.000 0.056 0.020 0.924
#&gt; SRR1812716     3  0.5093    0.58086 0.000 0.012 0.640 0.348
#&gt; SRR1812715     4  0.3852    0.66403 0.000 0.008 0.192 0.800
#&gt; SRR1812714     2  0.6879    0.01511 0.016 0.496 0.064 0.424
#&gt; SRR1812719     3  0.9041    0.09064 0.108 0.240 0.464 0.188
#&gt; SRR1812713     4  0.4775    0.51083 0.000 0.028 0.232 0.740
#&gt; SRR1812712     4  0.3450    0.55545 0.000 0.008 0.156 0.836
#&gt; SRR1812711     2  0.7654    0.20687 0.000 0.420 0.368 0.212
#&gt; SRR1812710     4  0.3751    0.66498 0.000 0.004 0.196 0.800
#&gt; SRR1812709     4  0.0000    0.67227 0.000 0.000 0.000 1.000
#&gt; SRR1812708     1  0.2984    0.91060 0.888 0.084 0.028 0.000
#&gt; SRR1812707     4  0.3710    0.66718 0.000 0.004 0.192 0.804
#&gt; SRR1812705     2  0.7654    0.20687 0.000 0.420 0.368 0.212
#&gt; SRR1812706     3  0.5592    0.55079 0.000 0.044 0.656 0.300
#&gt; SRR1812704     4  0.3032    0.59158 0.000 0.008 0.124 0.868
#&gt; SRR1812703     2  0.7170    0.04088 0.012 0.592 0.152 0.244
#&gt; SRR1812702     3  0.5024    0.57933 0.000 0.008 0.632 0.360
#&gt; SRR1812741     1  0.7026    0.72346 0.644 0.184 0.144 0.028
#&gt; SRR1812740     3  0.5649    0.57082 0.000 0.036 0.620 0.344
#&gt; SRR1812739     4  0.4617    0.45021 0.000 0.032 0.204 0.764
#&gt; SRR1812738     4  0.0817    0.67343 0.000 0.024 0.000 0.976
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-3-a').click(function(){
  $('#tab-CV-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-4'>
<p><a id='tab-CV-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5
#&gt; SRR1812752     1   0.000     0.8599 1.000 0.000 NA 0.000 0.000
#&gt; SRR1812753     1   0.000     0.8599 1.000 0.000 NA 0.000 0.000
#&gt; SRR1812751     1   0.000     0.8599 1.000 0.000 NA 0.000 0.000
#&gt; SRR1812750     1   0.452     0.8407 0.712 0.028 NA 0.000 0.008
#&gt; SRR1812748     5   0.695     0.3972 0.000 0.056 NA 0.316 0.512
#&gt; SRR1812749     1   0.452     0.8407 0.712 0.028 NA 0.000 0.008
#&gt; SRR1812746     5   0.696     0.3226 0.092 0.040 NA 0.012 0.484
#&gt; SRR1812745     5   0.523     0.4680 0.000 0.060 NA 0.112 0.744
#&gt; SRR1812747     5   0.665    -0.3982 0.000 0.308 NA 0.144 0.524
#&gt; SRR1812744     5   0.715     0.4318 0.000 0.164 NA 0.136 0.572
#&gt; SRR1812743     4   0.495     0.6352 0.000 0.012 NA 0.616 0.020
#&gt; SRR1812742     2   0.683     0.7634 0.000 0.508 NA 0.204 0.268
#&gt; SRR1812737     4   0.502     0.6200 0.000 0.008 NA 0.556 0.020
#&gt; SRR1812735     4   0.806     0.2347 0.000 0.136 NA 0.384 0.160
#&gt; SRR1812734     5   0.642     0.4587 0.000 0.192 NA 0.020 0.584
#&gt; SRR1812733     5   0.616     0.3198 0.000 0.028 NA 0.364 0.536
#&gt; SRR1812736     5   0.548     0.4656 0.000 0.060 NA 0.140 0.720
#&gt; SRR1812732     4   0.646     0.3330 0.000 0.144 NA 0.636 0.148
#&gt; SRR1812730     5   0.450     0.3999 0.000 0.072 NA 0.136 0.776
#&gt; SRR1812731     4   0.478     0.6349 0.000 0.004 NA 0.612 0.020
#&gt; SRR1812729     4   0.709     0.0245 0.000 0.116 NA 0.468 0.356
#&gt; SRR1812727     5   0.683     0.4542 0.024 0.228 NA 0.024 0.588
#&gt; SRR1812726     2   0.604     0.8862 0.000 0.560 NA 0.156 0.284
#&gt; SRR1812728     5   0.593     0.1246 0.000 0.200 NA 0.136 0.644
#&gt; SRR1812724     4   0.121     0.6173 0.000 0.000 NA 0.960 0.024
#&gt; SRR1812725     5   0.614     0.0411 0.000 0.212 NA 0.140 0.624
#&gt; SRR1812723     2   0.606     0.8635 0.000 0.536 NA 0.140 0.324
#&gt; SRR1812722     2   0.610     0.8773 0.000 0.552 NA 0.164 0.284
#&gt; SRR1812721     4   0.511     0.6138 0.000 0.008 NA 0.544 0.024
#&gt; SRR1812718     5   0.679    -0.4674 0.000 0.352 NA 0.148 0.476
#&gt; SRR1812717     4   0.357     0.5625 0.000 0.076 NA 0.844 0.012
#&gt; SRR1812716     5   0.413     0.4192 0.000 0.044 NA 0.144 0.796
#&gt; SRR1812715     4   0.500     0.6203 0.000 0.004 NA 0.552 0.024
#&gt; SRR1812714     5   0.742     0.3984 0.000 0.284 NA 0.128 0.492
#&gt; SRR1812719     5   0.687     0.4552 0.052 0.120 NA 0.016 0.604
#&gt; SRR1812713     4   0.321     0.5861 0.000 0.028 NA 0.872 0.060
#&gt; SRR1812712     4   0.235     0.5993 0.000 0.000 NA 0.896 0.088
#&gt; SRR1812711     2   0.591     0.8780 0.000 0.560 NA 0.128 0.312
#&gt; SRR1812710     4   0.511     0.6177 0.000 0.008 NA 0.548 0.024
#&gt; SRR1812709     4   0.121     0.6268 0.000 0.000 NA 0.960 0.016
#&gt; SRR1812708     1   0.365     0.8605 0.816 0.028 NA 0.000 0.008
#&gt; SRR1812707     4   0.518     0.6174 0.000 0.008 NA 0.544 0.028
#&gt; SRR1812705     2   0.609     0.8726 0.000 0.524 NA 0.140 0.336
#&gt; SRR1812706     5   0.458     0.4252 0.000 0.084 NA 0.120 0.776
#&gt; SRR1812704     4   0.586    -0.0460 0.000 0.048 NA 0.508 0.420
#&gt; SRR1812703     5   0.658     0.4049 0.000 0.312 NA 0.048 0.548
#&gt; SRR1812702     5   0.400     0.4288 0.000 0.040 NA 0.148 0.800
#&gt; SRR1812741     1   0.745     0.7042 0.520 0.208 NA 0.044 0.016
#&gt; SRR1812740     5   0.519     0.4692 0.000 0.024 NA 0.184 0.716
#&gt; SRR1812739     4   0.353     0.5766 0.000 0.016 NA 0.840 0.112
#&gt; SRR1812738     4   0.074     0.6239 0.000 0.004 NA 0.980 0.008
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-4-a').click(function(){
  $('#tab-CV-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-mclust-get-classes-5'>
<p><a id='tab-CV-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.5503   7.24e-01 0.552 0.000 0.000 0.276 0.172 0.000
#&gt; SRR1812753     1  0.5503   7.24e-01 0.552 0.000 0.000 0.276 0.172 0.000
#&gt; SRR1812751     1  0.5503   7.24e-01 0.552 0.000 0.000 0.276 0.172 0.000
#&gt; SRR1812750     1  0.1313   7.38e-01 0.952 0.000 0.028 0.004 0.000 0.016
#&gt; SRR1812748     3  0.5795   3.61e-01 0.000 0.268 0.608 0.064 0.020 0.040
#&gt; SRR1812749     1  0.1151   7.36e-01 0.956 0.000 0.032 0.000 0.000 0.012
#&gt; SRR1812746     3  0.6025   3.55e-01 0.264 0.000 0.584 0.008 0.056 0.088
#&gt; SRR1812745     3  0.6034  -1.76e-02 0.000 0.016 0.556 0.008 0.244 0.176
#&gt; SRR1812747     5  0.6475   1.45e-01 0.000 0.076 0.092 0.008 0.512 0.312
#&gt; SRR1812744     3  0.5302   4.37e-01 0.008 0.128 0.700 0.132 0.020 0.012
#&gt; SRR1812743     2  0.2212   4.08e-01 0.000 0.880 0.000 0.112 0.000 0.008
#&gt; SRR1812742     6  0.5466   6.78e-01 0.000 0.084 0.004 0.040 0.228 0.644
#&gt; SRR1812737     2  0.1075   4.93e-01 0.000 0.952 0.000 0.048 0.000 0.000
#&gt; SRR1812735     2  0.5825   1.45e-01 0.000 0.608 0.008 0.024 0.152 0.208
#&gt; SRR1812734     3  0.0881   4.51e-01 0.000 0.000 0.972 0.008 0.008 0.012
#&gt; SRR1812733     3  0.6837   2.65e-01 0.000 0.328 0.488 0.048 0.080 0.056
#&gt; SRR1812736     3  0.5898  -1.22e-03 0.000 0.020 0.560 0.000 0.228 0.192
#&gt; SRR1812732     4  0.5774   4.93e-01 0.000 0.216 0.248 0.532 0.000 0.004
#&gt; SRR1812730     5  0.6497   4.28e-01 0.000 0.064 0.320 0.000 0.480 0.136
#&gt; SRR1812731     2  0.2053   4.09e-01 0.000 0.888 0.000 0.108 0.000 0.004
#&gt; SRR1812729     2  0.7856   1.11e-01 0.000 0.460 0.180 0.084 0.096 0.180
#&gt; SRR1812727     3  0.4341   4.26e-01 0.040 0.000 0.796 0.032 0.076 0.056
#&gt; SRR1812726     6  0.4340   8.00e-01 0.000 0.064 0.004 0.000 0.224 0.708
#&gt; SRR1812728     5  0.6141   3.65e-01 0.000 0.052 0.148 0.000 0.564 0.236
#&gt; SRR1812724     4  0.4513   7.54e-01 0.000 0.440 0.024 0.532 0.000 0.004
#&gt; SRR1812725     5  0.6100   3.65e-01 0.000 0.068 0.136 0.000 0.588 0.208
#&gt; SRR1812723     6  0.4787   7.36e-01 0.000 0.068 0.000 0.000 0.336 0.596
#&gt; SRR1812722     6  0.4552   7.93e-01 0.000 0.064 0.004 0.004 0.236 0.692
#&gt; SRR1812721     2  0.1138   5.12e-01 0.000 0.960 0.000 0.012 0.004 0.024
#&gt; SRR1812718     5  0.6432   2.67e-02 0.000 0.072 0.088 0.004 0.448 0.388
#&gt; SRR1812717     4  0.4460   6.83e-01 0.000 0.404 0.004 0.568 0.000 0.024
#&gt; SRR1812716     5  0.5281   4.33e-01 0.000 0.060 0.324 0.000 0.588 0.028
#&gt; SRR1812715     2  0.0508   5.08e-01 0.000 0.984 0.000 0.012 0.000 0.004
#&gt; SRR1812714     3  0.6175   3.57e-01 0.012 0.032 0.668 0.084 0.100 0.104
#&gt; SRR1812719     3  0.7681   2.75e-01 0.132 0.000 0.448 0.036 0.220 0.164
#&gt; SRR1812713     4  0.5054   7.43e-01 0.000 0.400 0.036 0.544 0.004 0.016
#&gt; SRR1812712     4  0.5298   7.24e-01 0.000 0.456 0.044 0.476 0.004 0.020
#&gt; SRR1812711     6  0.4202   8.01e-01 0.000 0.064 0.000 0.000 0.224 0.712
#&gt; SRR1812710     2  0.0603   5.14e-01 0.000 0.980 0.000 0.000 0.004 0.016
#&gt; SRR1812709     2  0.4096  -7.56e-01 0.000 0.508 0.008 0.484 0.000 0.000
#&gt; SRR1812708     1  0.1523   7.37e-01 0.940 0.000 0.044 0.008 0.000 0.008
#&gt; SRR1812707     2  0.0665   5.13e-01 0.000 0.980 0.000 0.008 0.004 0.008
#&gt; SRR1812705     6  0.4781   7.56e-01 0.000 0.072 0.000 0.000 0.320 0.608
#&gt; SRR1812706     5  0.5886   3.81e-01 0.000 0.016 0.292 0.000 0.532 0.160
#&gt; SRR1812704     2  0.8153   9.85e-05 0.000 0.396 0.252 0.152 0.104 0.096
#&gt; SRR1812703     3  0.5398   2.90e-01 0.000 0.016 0.676 0.020 0.140 0.148
#&gt; SRR1812702     5  0.5464   4.30e-01 0.000 0.072 0.336 0.000 0.564 0.028
#&gt; SRR1812741     1  0.6339   5.55e-01 0.572 0.000 0.076 0.216 0.004 0.132
#&gt; SRR1812740     3  0.6130  -7.67e-02 0.000 0.088 0.568 0.012 0.280 0.052
#&gt; SRR1812739     2  0.5893  -6.65e-01 0.000 0.440 0.108 0.432 0.012 0.008
#&gt; SRR1812738     4  0.4128   7.09e-01 0.000 0.488 0.004 0.504 0.000 0.004
</code></pre>

<script>
$('#tab-CV-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-mclust-get-classes-5-a').click(function(){
  $('#tab-CV-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-CV-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-mclust-signature_compare](figure_cola/CV-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-CV-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-mclust-collect-classes](figure_cola/CV-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### CV:NMF**






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["CV", "NMF"]
# you can also extract it by
# res = res_list["CV:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'CV' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk CV-NMF-collect-plots](figure_cola/CV-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk CV-NMF-select-partition-number](figure_cola/CV-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.971       0.990         0.3096 0.704   0.704
#> 3 3 0.779           0.812       0.929         1.0570 0.655   0.509
#> 4 4 0.775           0.688       0.876         0.1455 0.869   0.656
#> 5 5 0.715           0.657       0.836         0.0737 0.934   0.768
#> 6 6 0.764           0.707       0.794         0.0522 0.908   0.631
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-CV-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-classes'>
<ul>
<li><a href='#tab-CV-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-CV-NMF-get-classes-1'>
<p><a id='tab-CV-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1       0     1.0000 1.000 0.000
#&gt; SRR1812753     1       0     1.0000 1.000 0.000
#&gt; SRR1812751     1       0     1.0000 1.000 0.000
#&gt; SRR1812750     1       0     1.0000 1.000 0.000
#&gt; SRR1812748     2       0     0.9878 0.000 1.000
#&gt; SRR1812749     1       0     1.0000 1.000 0.000
#&gt; SRR1812746     1       0     1.0000 1.000 0.000
#&gt; SRR1812745     2       0     0.9878 0.000 1.000
#&gt; SRR1812747     2       0     0.9878 0.000 1.000
#&gt; SRR1812744     2       0     0.9878 0.000 1.000
#&gt; SRR1812743     2       0     0.9878 0.000 1.000
#&gt; SRR1812742     2       0     0.9878 0.000 1.000
#&gt; SRR1812737     2       0     0.9878 0.000 1.000
#&gt; SRR1812735     2       0     0.9878 0.000 1.000
#&gt; SRR1812734     2       0     0.9878 0.000 1.000
#&gt; SRR1812733     2       0     0.9878 0.000 1.000
#&gt; SRR1812736     2       0     0.9878 0.000 1.000
#&gt; SRR1812732     2       0     0.9878 0.000 1.000
#&gt; SRR1812730     2       0     0.9878 0.000 1.000
#&gt; SRR1812731     2       0     0.9878 0.000 1.000
#&gt; SRR1812729     2       0     0.9878 0.000 1.000
#&gt; SRR1812727     1       0     1.0000 1.000 0.000
#&gt; SRR1812726     2       0     0.9878 0.000 1.000
#&gt; SRR1812728     2       0     0.9878 0.000 1.000
#&gt; SRR1812724     2       0     0.9878 0.000 1.000
#&gt; SRR1812725     2       0     0.9878 0.000 1.000
#&gt; SRR1812723     2       0     0.9878 0.000 1.000
#&gt; SRR1812722     2       0     0.9878 0.000 1.000
#&gt; SRR1812721     2       0     0.9878 0.000 1.000
#&gt; SRR1812718     2       0     0.9878 0.000 1.000
#&gt; SRR1812717     2       0     0.9878 0.000 1.000
#&gt; SRR1812716     2       0     0.9878 0.000 1.000
#&gt; SRR1812715     2       0     0.9878 0.000 1.000
#&gt; SRR1812714     2       0     0.9878 0.000 1.000
#&gt; SRR1812719     2       1     0.0159 0.496 0.504
#&gt; SRR1812713     2       0     0.9878 0.000 1.000
#&gt; SRR1812712     2       0     0.9878 0.000 1.000
#&gt; SRR1812711     2       0     0.9878 0.000 1.000
#&gt; SRR1812710     2       0     0.9878 0.000 1.000
#&gt; SRR1812709     2       0     0.9878 0.000 1.000
#&gt; SRR1812708     1       0     1.0000 1.000 0.000
#&gt; SRR1812707     2       0     0.9878 0.000 1.000
#&gt; SRR1812705     2       0     0.9878 0.000 1.000
#&gt; SRR1812706     2       0     0.9878 0.000 1.000
#&gt; SRR1812704     2       0     0.9878 0.000 1.000
#&gt; SRR1812703     2       0     0.9878 0.000 1.000
#&gt; SRR1812702     2       0     0.9878 0.000 1.000
#&gt; SRR1812741     1       0     1.0000 1.000 0.000
#&gt; SRR1812740     2       0     0.9878 0.000 1.000
#&gt; SRR1812739     2       0     0.9878 0.000 1.000
#&gt; SRR1812738     2       0     0.9878 0.000 1.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-1-a').click(function(){
  $('#tab-CV-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-2'>
<p><a id='tab-CV-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812748     2  0.6309     0.0354 0.000 0.500 0.500
#&gt; SRR1812749     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812746     1  0.6295     0.1016 0.528 0.000 0.472
#&gt; SRR1812745     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812747     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812744     2  0.5404     0.6304 0.004 0.740 0.256
#&gt; SRR1812743     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812742     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812737     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812735     2  0.6154     0.2158 0.000 0.592 0.408
#&gt; SRR1812734     3  0.2550     0.8504 0.012 0.056 0.932
#&gt; SRR1812733     3  0.6154     0.2175 0.000 0.408 0.592
#&gt; SRR1812736     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812732     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812730     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812729     2  0.0424     0.9044 0.000 0.992 0.008
#&gt; SRR1812727     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812726     3  0.5926     0.4657 0.000 0.356 0.644
#&gt; SRR1812728     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812724     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812725     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812723     3  0.0424     0.8954 0.000 0.008 0.992
#&gt; SRR1812722     3  0.5465     0.6022 0.000 0.288 0.712
#&gt; SRR1812721     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812718     3  0.0237     0.8974 0.000 0.004 0.996
#&gt; SRR1812717     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812716     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812719     3  0.1031     0.8827 0.024 0.000 0.976
#&gt; SRR1812713     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812712     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812711     3  0.4002     0.7751 0.000 0.160 0.840
#&gt; SRR1812710     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812709     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812708     1  0.0000     0.9364 1.000 0.000 0.000
#&gt; SRR1812707     2  0.0000     0.9091 0.000 1.000 0.000
#&gt; SRR1812705     3  0.4178     0.7635 0.000 0.172 0.828
#&gt; SRR1812706     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812704     2  0.1031     0.8935 0.000 0.976 0.024
#&gt; SRR1812703     2  0.5988     0.4242 0.000 0.632 0.368
#&gt; SRR1812702     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812741     1  0.0424     0.9298 0.992 0.008 0.000
#&gt; SRR1812740     3  0.0000     0.8989 0.000 0.000 1.000
#&gt; SRR1812739     2  0.2165     0.8607 0.000 0.936 0.064
#&gt; SRR1812738     2  0.0000     0.9091 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-2-a').click(function(){
  $('#tab-CV-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-3'>
<p><a id='tab-CV-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000     0.8746 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.8746 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.8746 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.8746 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.4343     0.4515 0.000 0.004 0.732 0.264
#&gt; SRR1812749     1  0.0000     0.8746 1.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.5366     0.2240 0.548 0.012 0.440 0.000
#&gt; SRR1812745     2  0.5000    -0.1269 0.000 0.504 0.496 0.000
#&gt; SRR1812747     2  0.0000     0.8611 0.000 1.000 0.000 0.000
#&gt; SRR1812744     3  0.1557     0.4576 0.000 0.000 0.944 0.056
#&gt; SRR1812743     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812742     2  0.0469     0.8603 0.000 0.988 0.012 0.000
#&gt; SRR1812737     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812735     2  0.2011     0.7849 0.000 0.920 0.000 0.080
#&gt; SRR1812734     3  0.0188     0.4569 0.000 0.004 0.996 0.000
#&gt; SRR1812733     3  0.6324     0.2525 0.000 0.064 0.536 0.400
#&gt; SRR1812736     3  0.4925     0.1694 0.000 0.428 0.572 0.000
#&gt; SRR1812732     4  0.4730     0.4378 0.000 0.000 0.364 0.636
#&gt; SRR1812730     2  0.3266     0.7342 0.000 0.832 0.168 0.000
#&gt; SRR1812731     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812729     4  0.4961     0.1396 0.000 0.448 0.000 0.552
#&gt; SRR1812727     1  0.5132     0.3626 0.548 0.004 0.448 0.000
#&gt; SRR1812726     2  0.1940     0.8088 0.000 0.924 0.076 0.000
#&gt; SRR1812728     2  0.0336     0.8612 0.000 0.992 0.008 0.000
#&gt; SRR1812724     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812725     2  0.0336     0.8612 0.000 0.992 0.008 0.000
#&gt; SRR1812723     2  0.0000     0.8611 0.000 1.000 0.000 0.000
#&gt; SRR1812722     2  0.1792     0.8185 0.000 0.932 0.068 0.000
#&gt; SRR1812721     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812718     2  0.0188     0.8612 0.000 0.996 0.004 0.000
#&gt; SRR1812717     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812716     2  0.1211     0.8481 0.000 0.960 0.040 0.000
#&gt; SRR1812715     4  0.0469     0.8815 0.000 0.012 0.000 0.988
#&gt; SRR1812714     4  0.5483     0.2357 0.000 0.016 0.448 0.536
#&gt; SRR1812719     2  0.6967     0.3240 0.176 0.580 0.244 0.000
#&gt; SRR1812713     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812712     4  0.0188     0.8862 0.000 0.000 0.004 0.996
#&gt; SRR1812711     2  0.0000     0.8611 0.000 1.000 0.000 0.000
#&gt; SRR1812710     4  0.0592     0.8785 0.000 0.016 0.000 0.984
#&gt; SRR1812709     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812708     1  0.0000     0.8746 1.000 0.000 0.000 0.000
#&gt; SRR1812707     4  0.0000     0.8886 0.000 0.000 0.000 1.000
#&gt; SRR1812705     2  0.0000     0.8611 0.000 1.000 0.000 0.000
#&gt; SRR1812706     2  0.3266     0.7345 0.000 0.832 0.168 0.000
#&gt; SRR1812704     4  0.0895     0.8710 0.000 0.020 0.004 0.976
#&gt; SRR1812703     3  0.7117     0.0406 0.000 0.424 0.448 0.128
#&gt; SRR1812702     2  0.2760     0.7809 0.000 0.872 0.128 0.000
#&gt; SRR1812741     1  0.0707     0.8579 0.980 0.000 0.000 0.020
#&gt; SRR1812740     3  0.4972     0.0995 0.000 0.456 0.544 0.000
#&gt; SRR1812739     4  0.3486     0.6777 0.000 0.000 0.188 0.812
#&gt; SRR1812738     4  0.0000     0.8886 0.000 0.000 0.000 1.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-3-a').click(function(){
  $('#tab-CV-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-4'>
<p><a id='tab-CV-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000      0.927 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.927 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.927 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0162      0.926 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1812748     3  0.4547      0.315 0.000 0.000 0.704 0.044 0.252
#&gt; SRR1812749     1  0.0000      0.927 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     1  0.4812      0.369 0.600 0.000 0.372 0.000 0.028
#&gt; SRR1812745     3  0.5937      0.431 0.000 0.300 0.564 0.000 0.136
#&gt; SRR1812747     2  0.2929      0.655 0.000 0.840 0.152 0.000 0.008
#&gt; SRR1812744     5  0.2488      0.706 0.000 0.000 0.124 0.004 0.872
#&gt; SRR1812743     4  0.2645      0.815 0.000 0.000 0.068 0.888 0.044
#&gt; SRR1812742     2  0.4522      0.569 0.000 0.744 0.176 0.000 0.080
#&gt; SRR1812737     4  0.0162      0.862 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812735     2  0.3832      0.636 0.000 0.824 0.104 0.060 0.012
#&gt; SRR1812734     5  0.0794      0.745 0.000 0.000 0.028 0.000 0.972
#&gt; SRR1812733     3  0.4737      0.163 0.000 0.016 0.600 0.380 0.004
#&gt; SRR1812736     3  0.5264      0.503 0.000 0.128 0.676 0.000 0.196
#&gt; SRR1812732     4  0.6825     -0.112 0.000 0.000 0.328 0.340 0.332
#&gt; SRR1812730     2  0.4262      0.200 0.000 0.560 0.440 0.000 0.000
#&gt; SRR1812731     4  0.0566      0.862 0.000 0.000 0.004 0.984 0.012
#&gt; SRR1812729     4  0.5901      0.336 0.000 0.300 0.132 0.568 0.000
#&gt; SRR1812727     5  0.3003      0.725 0.188 0.000 0.000 0.000 0.812
#&gt; SRR1812726     2  0.3521      0.553 0.000 0.764 0.004 0.000 0.232
#&gt; SRR1812728     2  0.0162      0.725 0.000 0.996 0.004 0.000 0.000
#&gt; SRR1812724     4  0.0510      0.861 0.000 0.000 0.016 0.984 0.000
#&gt; SRR1812725     2  0.0880      0.716 0.000 0.968 0.032 0.000 0.000
#&gt; SRR1812723     2  0.0000      0.726 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812722     2  0.2806      0.646 0.000 0.844 0.004 0.000 0.152
#&gt; SRR1812721     4  0.0671      0.861 0.000 0.000 0.016 0.980 0.004
#&gt; SRR1812718     2  0.0290      0.726 0.000 0.992 0.008 0.000 0.000
#&gt; SRR1812717     4  0.0000      0.862 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812716     2  0.4219      0.248 0.000 0.584 0.416 0.000 0.000
#&gt; SRR1812715     4  0.4113      0.743 0.000 0.120 0.048 0.808 0.024
#&gt; SRR1812714     5  0.3454      0.731 0.000 0.064 0.000 0.100 0.836
#&gt; SRR1812719     5  0.6032      0.593 0.160 0.184 0.020 0.000 0.636
#&gt; SRR1812713     4  0.1704      0.834 0.000 0.000 0.068 0.928 0.004
#&gt; SRR1812712     4  0.1251      0.850 0.000 0.000 0.036 0.956 0.008
#&gt; SRR1812711     2  0.0703      0.721 0.000 0.976 0.024 0.000 0.000
#&gt; SRR1812710     4  0.1393      0.853 0.000 0.024 0.008 0.956 0.012
#&gt; SRR1812709     4  0.0162      0.862 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812708     1  0.0609      0.920 0.980 0.000 0.020 0.000 0.000
#&gt; SRR1812707     4  0.0162      0.862 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812705     2  0.0000      0.726 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812706     2  0.4300      0.131 0.000 0.524 0.476 0.000 0.000
#&gt; SRR1812704     4  0.3797      0.653 0.000 0.008 0.232 0.756 0.004
#&gt; SRR1812703     5  0.4432      0.720 0.000 0.080 0.112 0.020 0.788
#&gt; SRR1812702     2  0.4278      0.161 0.000 0.548 0.452 0.000 0.000
#&gt; SRR1812741     1  0.1117      0.908 0.964 0.000 0.020 0.016 0.000
#&gt; SRR1812740     3  0.5550      0.299 0.000 0.376 0.548 0.000 0.076
#&gt; SRR1812739     4  0.3732      0.725 0.000 0.000 0.176 0.792 0.032
#&gt; SRR1812738     4  0.0290      0.861 0.000 0.000 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-4-a').click(function(){
  $('#tab-CV-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-CV-NMF-get-classes-5'>
<p><a id='tab-CV-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000     0.9226 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.9226 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000     0.9226 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.1232     0.9199 0.956 0.000 0.016 0.000 0.024 0.004
#&gt; SRR1812748     3  0.1932     0.6521 0.000 0.004 0.912 0.004 0.076 0.004
#&gt; SRR1812749     1  0.0862     0.9208 0.972 0.000 0.016 0.000 0.008 0.004
#&gt; SRR1812746     1  0.5207     0.5563 0.628 0.004 0.156 0.000 0.212 0.000
#&gt; SRR1812745     3  0.5989     0.2683 0.000 0.248 0.432 0.000 0.320 0.000
#&gt; SRR1812747     2  0.1958     0.7462 0.000 0.896 0.100 0.000 0.004 0.000
#&gt; SRR1812744     6  0.2697     0.7966 0.000 0.000 0.188 0.000 0.000 0.812
#&gt; SRR1812743     4  0.3917     0.6986 0.000 0.008 0.240 0.728 0.024 0.000
#&gt; SRR1812742     2  0.3337     0.5210 0.000 0.736 0.260 0.000 0.004 0.000
#&gt; SRR1812737     4  0.1082     0.8438 0.000 0.004 0.000 0.956 0.040 0.000
#&gt; SRR1812735     2  0.2984     0.6931 0.000 0.860 0.064 0.064 0.012 0.000
#&gt; SRR1812734     6  0.0547     0.9162 0.000 0.000 0.020 0.000 0.000 0.980
#&gt; SRR1812733     5  0.3841     0.4409 0.000 0.000 0.032 0.244 0.724 0.000
#&gt; SRR1812736     3  0.4650     0.6153 0.000 0.180 0.688 0.000 0.132 0.000
#&gt; SRR1812732     3  0.2405     0.5729 0.000 0.000 0.880 0.100 0.004 0.016
#&gt; SRR1812730     5  0.2902     0.5578 0.000 0.196 0.004 0.000 0.800 0.000
#&gt; SRR1812731     4  0.1462     0.8404 0.000 0.000 0.056 0.936 0.008 0.000
#&gt; SRR1812729     5  0.5202     0.0612 0.000 0.076 0.004 0.448 0.472 0.000
#&gt; SRR1812727     6  0.0146     0.9193 0.000 0.000 0.004 0.000 0.000 0.996
#&gt; SRR1812726     2  0.3566     0.7717 0.000 0.800 0.000 0.000 0.096 0.104
#&gt; SRR1812728     2  0.3126     0.7428 0.000 0.752 0.000 0.000 0.248 0.000
#&gt; SRR1812724     4  0.2488     0.8308 0.000 0.000 0.076 0.880 0.044 0.000
#&gt; SRR1812725     2  0.3101     0.7456 0.000 0.756 0.000 0.000 0.244 0.000
#&gt; SRR1812723     2  0.3023     0.7576 0.000 0.768 0.000 0.000 0.232 0.000
#&gt; SRR1812722     2  0.2224     0.7809 0.000 0.904 0.012 0.000 0.020 0.064
#&gt; SRR1812721     4  0.1838     0.8394 0.000 0.012 0.040 0.928 0.020 0.000
#&gt; SRR1812718     2  0.1957     0.8080 0.000 0.888 0.000 0.000 0.112 0.000
#&gt; SRR1812717     4  0.0632     0.8440 0.000 0.000 0.000 0.976 0.024 0.000
#&gt; SRR1812716     5  0.3314     0.5469 0.000 0.224 0.012 0.000 0.764 0.000
#&gt; SRR1812715     4  0.5764     0.2410 0.000 0.376 0.116 0.492 0.016 0.000
#&gt; SRR1812714     6  0.0146     0.9191 0.000 0.000 0.000 0.000 0.004 0.996
#&gt; SRR1812719     6  0.4081     0.7901 0.056 0.052 0.008 0.000 0.080 0.804
#&gt; SRR1812713     4  0.2994     0.7002 0.000 0.000 0.004 0.788 0.208 0.000
#&gt; SRR1812712     4  0.1663     0.8069 0.000 0.000 0.000 0.912 0.088 0.000
#&gt; SRR1812711     2  0.1444     0.8037 0.000 0.928 0.000 0.000 0.072 0.000
#&gt; SRR1812710     4  0.2122     0.8433 0.000 0.024 0.028 0.916 0.032 0.000
#&gt; SRR1812709     4  0.0363     0.8458 0.000 0.000 0.000 0.988 0.012 0.000
#&gt; SRR1812708     1  0.2206     0.9031 0.904 0.000 0.024 0.000 0.064 0.008
#&gt; SRR1812707     4  0.0146     0.8461 0.000 0.000 0.000 0.996 0.004 0.000
#&gt; SRR1812705     2  0.2527     0.7935 0.000 0.832 0.000 0.000 0.168 0.000
#&gt; SRR1812706     5  0.3584     0.5008 0.000 0.308 0.004 0.000 0.688 0.000
#&gt; SRR1812704     5  0.3706     0.3192 0.000 0.000 0.000 0.380 0.620 0.000
#&gt; SRR1812703     6  0.0260     0.9183 0.000 0.000 0.000 0.000 0.008 0.992
#&gt; SRR1812702     5  0.3231     0.5462 0.000 0.200 0.016 0.000 0.784 0.000
#&gt; SRR1812741     1  0.1952     0.9011 0.920 0.000 0.016 0.012 0.052 0.000
#&gt; SRR1812740     5  0.5199     0.0187 0.000 0.120 0.300 0.000 0.580 0.000
#&gt; SRR1812739     4  0.3852     0.5779 0.000 0.000 0.324 0.664 0.012 0.000
#&gt; SRR1812738     4  0.2506     0.8240 0.000 0.000 0.052 0.880 0.068 0.000
</code></pre>

<script>
$('#tab-CV-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-CV-NMF-get-classes-5-a').click(function(){
  $('#tab-CV-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-CV-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-CV-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-CV-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-CV-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-CV-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-CV-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-CV-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-CV-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk CV-NMF-signature_compare](figure_cola/CV-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-CV-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-CV-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-CV-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-CV-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-CV-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-CV-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-CV-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-CV-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk CV-NMF-collect-classes](figure_cola/CV-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "hclust"]
# you can also extract it by
# res = res_list["MAD:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-hclust-collect-plots](figure_cola/MAD-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-hclust-select-partition-number](figure_cola/MAD-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.327           0.618       0.837         0.4268 0.547   0.547
#> 3 3 0.398           0.573       0.789         0.3440 0.886   0.792
#> 4 4 0.496           0.651       0.802         0.1439 0.867   0.705
#> 5 5 0.539           0.648       0.720         0.1380 0.842   0.558
#> 6 6 0.579           0.591       0.707         0.0364 0.920   0.692
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-classes'>
<ul>
<li><a href='#tab-MAD-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-hclust-get-classes-1'>
<p><a id='tab-MAD-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     2  0.7674      0.651 0.224 0.776
#&gt; SRR1812753     2  0.7674      0.651 0.224 0.776
#&gt; SRR1812751     2  0.1184      0.806 0.016 0.984
#&gt; SRR1812750     2  0.1184      0.806 0.016 0.984
#&gt; SRR1812748     1  0.1184      0.720 0.984 0.016
#&gt; SRR1812749     2  0.1184      0.806 0.016 0.984
#&gt; SRR1812746     1  0.2043      0.723 0.968 0.032
#&gt; SRR1812745     1  0.1184      0.720 0.984 0.016
#&gt; SRR1812747     2  0.1633      0.803 0.024 0.976
#&gt; SRR1812744     1  0.2778      0.721 0.952 0.048
#&gt; SRR1812743     1  0.8267      0.656 0.740 0.260
#&gt; SRR1812742     1  0.7883      0.674 0.764 0.236
#&gt; SRR1812737     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812735     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812734     1  0.1414      0.720 0.980 0.020
#&gt; SRR1812733     2  0.5946      0.737 0.144 0.856
#&gt; SRR1812736     1  0.1184      0.720 0.984 0.016
#&gt; SRR1812732     2  1.0000     -0.226 0.496 0.504
#&gt; SRR1812730     2  0.8861      0.522 0.304 0.696
#&gt; SRR1812731     2  0.9983     -0.195 0.476 0.524
#&gt; SRR1812729     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812727     1  0.7219      0.686 0.800 0.200
#&gt; SRR1812726     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812728     1  0.9087      0.580 0.676 0.324
#&gt; SRR1812724     1  0.9996      0.256 0.512 0.488
#&gt; SRR1812725     2  0.8909      0.516 0.308 0.692
#&gt; SRR1812723     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812722     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812721     1  1.0000      0.242 0.504 0.496
#&gt; SRR1812718     2  0.1843      0.803 0.028 0.972
#&gt; SRR1812717     2  0.9996     -0.247 0.488 0.512
#&gt; SRR1812716     2  0.8861      0.522 0.304 0.696
#&gt; SRR1812715     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812714     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812719     1  0.7219      0.686 0.800 0.200
#&gt; SRR1812713     2  0.4022      0.779 0.080 0.920
#&gt; SRR1812712     2  0.7219      0.650 0.200 0.800
#&gt; SRR1812711     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812710     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812709     2  0.7219      0.650 0.200 0.800
#&gt; SRR1812708     2  0.0938      0.808 0.012 0.988
#&gt; SRR1812707     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812705     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812706     1  0.9922      0.327 0.552 0.448
#&gt; SRR1812704     2  0.8909      0.516 0.308 0.692
#&gt; SRR1812703     2  0.0000      0.815 0.000 1.000
#&gt; SRR1812702     2  0.8909      0.516 0.308 0.692
#&gt; SRR1812741     1  0.9977      0.300 0.528 0.472
#&gt; SRR1812740     1  0.1184      0.720 0.984 0.016
#&gt; SRR1812739     2  0.9323      0.353 0.348 0.652
#&gt; SRR1812738     1  0.9993      0.268 0.516 0.484
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-1-a').click(function(){
  $('#tab-MAD-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-2'>
<p><a id='tab-MAD-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.4178      0.744 0.828 0.000 0.172
#&gt; SRR1812753     1  0.4178      0.744 0.828 0.000 0.172
#&gt; SRR1812751     1  0.4504      0.847 0.804 0.196 0.000
#&gt; SRR1812750     1  0.4504      0.847 0.804 0.196 0.000
#&gt; SRR1812748     3  0.0237      0.646 0.004 0.000 0.996
#&gt; SRR1812749     1  0.4504      0.847 0.804 0.196 0.000
#&gt; SRR1812746     3  0.0747      0.642 0.016 0.000 0.984
#&gt; SRR1812745     3  0.0000      0.647 0.000 0.000 1.000
#&gt; SRR1812747     2  0.1919      0.763 0.020 0.956 0.024
#&gt; SRR1812744     3  0.2400      0.647 0.064 0.004 0.932
#&gt; SRR1812743     3  0.7875      0.597 0.156 0.176 0.668
#&gt; SRR1812742     3  0.7447      0.625 0.160 0.140 0.700
#&gt; SRR1812737     2  0.1289      0.767 0.032 0.968 0.000
#&gt; SRR1812735     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812734     3  0.0237      0.646 0.004 0.000 0.996
#&gt; SRR1812733     2  0.5471      0.682 0.060 0.812 0.128
#&gt; SRR1812736     3  0.0237      0.646 0.004 0.000 0.996
#&gt; SRR1812732     2  0.9027     -0.175 0.132 0.440 0.428
#&gt; SRR1812730     2  0.7396      0.494 0.060 0.644 0.296
#&gt; SRR1812731     2  0.9224     -0.184 0.152 0.440 0.408
#&gt; SRR1812729     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812727     3  0.5158      0.470 0.232 0.004 0.764
#&gt; SRR1812726     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812728     3  0.7872      0.559 0.112 0.236 0.652
#&gt; SRR1812724     3  0.9221      0.219 0.152 0.404 0.444
#&gt; SRR1812725     2  0.7424      0.489 0.060 0.640 0.300
#&gt; SRR1812723     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812722     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812721     3  0.9264      0.209 0.156 0.412 0.432
#&gt; SRR1812718     2  0.2434      0.758 0.036 0.940 0.024
#&gt; SRR1812717     2  0.9229     -0.228 0.152 0.428 0.420
#&gt; SRR1812716     2  0.7396      0.494 0.060 0.644 0.296
#&gt; SRR1812715     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812714     2  0.0592      0.763 0.012 0.988 0.000
#&gt; SRR1812719     3  0.5158      0.470 0.232 0.004 0.764
#&gt; SRR1812713     2  0.4194      0.714 0.064 0.876 0.060
#&gt; SRR1812712     2  0.7273      0.562 0.156 0.712 0.132
#&gt; SRR1812711     2  0.1163      0.766 0.028 0.972 0.000
#&gt; SRR1812710     2  0.1289      0.767 0.032 0.968 0.000
#&gt; SRR1812709     2  0.7273      0.562 0.156 0.712 0.132
#&gt; SRR1812708     2  0.4178      0.574 0.172 0.828 0.000
#&gt; SRR1812707     2  0.1289      0.767 0.032 0.968 0.000
#&gt; SRR1812705     2  0.1399      0.765 0.028 0.968 0.004
#&gt; SRR1812706     3  0.9130      0.296 0.152 0.356 0.492
#&gt; SRR1812704     2  0.7564      0.483 0.068 0.636 0.296
#&gt; SRR1812703     2  0.0424      0.763 0.008 0.992 0.000
#&gt; SRR1812702     2  0.7424      0.489 0.060 0.640 0.300
#&gt; SRR1812741     3  0.9202      0.258 0.152 0.388 0.460
#&gt; SRR1812740     3  0.0237      0.646 0.004 0.000 0.996
#&gt; SRR1812739     2  0.8520      0.314 0.132 0.588 0.280
#&gt; SRR1812738     3  0.9217      0.230 0.152 0.400 0.448
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-2-a').click(function(){
  $('#tab-MAD-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-3'>
<p><a id='tab-MAD-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.4387      0.697 0.804 0.000 0.144 0.052
#&gt; SRR1812753     1  0.4387      0.697 0.804 0.000 0.144 0.052
#&gt; SRR1812751     1  0.3837      0.832 0.776 0.224 0.000 0.000
#&gt; SRR1812750     1  0.3837      0.832 0.776 0.224 0.000 0.000
#&gt; SRR1812748     3  0.0779      0.858 0.004 0.000 0.980 0.016
#&gt; SRR1812749     1  0.3837      0.832 0.776 0.224 0.000 0.000
#&gt; SRR1812746     3  0.0895      0.854 0.020 0.000 0.976 0.004
#&gt; SRR1812745     3  0.0188      0.858 0.000 0.000 0.996 0.004
#&gt; SRR1812747     2  0.1022      0.741 0.032 0.968 0.000 0.000
#&gt; SRR1812744     3  0.2611      0.796 0.008 0.000 0.896 0.096
#&gt; SRR1812743     4  0.0712      0.471 0.004 0.004 0.008 0.984
#&gt; SRR1812742     4  0.1847      0.444 0.004 0.004 0.052 0.940
#&gt; SRR1812737     2  0.1118      0.727 0.000 0.964 0.000 0.036
#&gt; SRR1812735     2  0.0000      0.742 0.000 1.000 0.000 0.000
#&gt; SRR1812734     3  0.0336      0.857 0.000 0.000 0.992 0.008
#&gt; SRR1812733     2  0.6791      0.571 0.184 0.676 0.092 0.048
#&gt; SRR1812736     3  0.0376      0.859 0.004 0.000 0.992 0.004
#&gt; SRR1812732     4  0.5598      0.709 0.028 0.332 0.004 0.636
#&gt; SRR1812730     2  0.7928      0.410 0.184 0.528 0.260 0.028
#&gt; SRR1812731     4  0.4819      0.733 0.000 0.344 0.004 0.652
#&gt; SRR1812729     2  0.0188      0.741 0.000 0.996 0.000 0.004
#&gt; SRR1812727     3  0.5540      0.673 0.208 0.004 0.720 0.068
#&gt; SRR1812726     2  0.0000      0.742 0.000 1.000 0.000 0.000
#&gt; SRR1812728     3  0.7629      0.473 0.088 0.220 0.608 0.084
#&gt; SRR1812724     4  0.4923      0.767 0.004 0.304 0.008 0.684
#&gt; SRR1812725     2  0.7861      0.409 0.184 0.528 0.264 0.024
#&gt; SRR1812723     2  0.0000      0.742 0.000 1.000 0.000 0.000
#&gt; SRR1812722     2  0.0000      0.742 0.000 1.000 0.000 0.000
#&gt; SRR1812721     4  0.4872      0.720 0.004 0.356 0.000 0.640
#&gt; SRR1812718     2  0.2589      0.712 0.116 0.884 0.000 0.000
#&gt; SRR1812717     4  0.4776      0.709 0.000 0.376 0.000 0.624
#&gt; SRR1812716     2  0.7928      0.410 0.184 0.528 0.260 0.028
#&gt; SRR1812715     2  0.0000      0.742 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.2125      0.729 0.076 0.920 0.004 0.000
#&gt; SRR1812719     3  0.5540      0.673 0.208 0.004 0.720 0.068
#&gt; SRR1812713     2  0.5640      0.623 0.184 0.740 0.032 0.044
#&gt; SRR1812712     2  0.5649      0.300 0.052 0.664 0.000 0.284
#&gt; SRR1812711     2  0.0000      0.742 0.000 1.000 0.000 0.000
#&gt; SRR1812710     2  0.1118      0.727 0.000 0.964 0.000 0.036
#&gt; SRR1812709     2  0.5649      0.300 0.052 0.664 0.000 0.284
#&gt; SRR1812708     2  0.4122      0.595 0.236 0.760 0.004 0.000
#&gt; SRR1812707     2  0.1118      0.727 0.000 0.964 0.000 0.036
#&gt; SRR1812705     2  0.0469      0.742 0.000 0.988 0.000 0.012
#&gt; SRR1812706     4  0.7729      0.450 0.004 0.328 0.208 0.460
#&gt; SRR1812704     2  0.8469      0.384 0.168 0.528 0.224 0.080
#&gt; SRR1812703     2  0.2053      0.730 0.072 0.924 0.004 0.000
#&gt; SRR1812702     2  0.7861      0.409 0.184 0.528 0.264 0.024
#&gt; SRR1812741     4  0.4509      0.766 0.000 0.288 0.004 0.708
#&gt; SRR1812740     3  0.0779      0.858 0.004 0.000 0.980 0.016
#&gt; SRR1812739     2  0.6178     -0.410 0.040 0.480 0.004 0.476
#&gt; SRR1812738     4  0.4655      0.766 0.000 0.312 0.004 0.684
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-3-a').click(function(){
  $('#tab-MAD-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-4'>
<p><a id='tab-MAD-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0609     0.6943 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1812753     1  0.0609     0.6943 0.980 0.000 0.000 0.000 0.020
#&gt; SRR1812751     1  0.4355     0.8167 0.732 0.224 0.000 0.000 0.044
#&gt; SRR1812750     1  0.4355     0.8167 0.732 0.224 0.000 0.000 0.044
#&gt; SRR1812748     3  0.2777     0.7261 0.000 0.000 0.864 0.016 0.120
#&gt; SRR1812749     1  0.4355     0.8167 0.732 0.224 0.000 0.000 0.044
#&gt; SRR1812746     3  0.0912     0.7619 0.012 0.000 0.972 0.000 0.016
#&gt; SRR1812745     3  0.0290     0.7595 0.000 0.000 0.992 0.000 0.008
#&gt; SRR1812747     2  0.1430     0.7623 0.000 0.944 0.000 0.004 0.052
#&gt; SRR1812744     3  0.3178     0.7067 0.004 0.000 0.860 0.088 0.048
#&gt; SRR1812743     4  0.3074     0.5006 0.000 0.000 0.000 0.804 0.196
#&gt; SRR1812742     4  0.4150     0.4714 0.000 0.000 0.036 0.748 0.216
#&gt; SRR1812737     2  0.3209     0.6415 0.000 0.812 0.000 0.180 0.008
#&gt; SRR1812735     2  0.0510     0.7864 0.000 0.984 0.000 0.016 0.000
#&gt; SRR1812734     3  0.1205     0.7519 0.004 0.000 0.956 0.000 0.040
#&gt; SRR1812733     5  0.6362     0.5301 0.000 0.420 0.044 0.060 0.476
#&gt; SRR1812736     3  0.2329     0.7291 0.000 0.000 0.876 0.000 0.124
#&gt; SRR1812732     4  0.3574     0.7132 0.000 0.168 0.000 0.804 0.028
#&gt; SRR1812730     5  0.6771     0.9112 0.000 0.264 0.184 0.024 0.528
#&gt; SRR1812731     4  0.2929     0.7235 0.000 0.180 0.000 0.820 0.000
#&gt; SRR1812729     2  0.1117     0.7847 0.000 0.964 0.000 0.020 0.016
#&gt; SRR1812727     3  0.5671     0.4789 0.400 0.000 0.536 0.016 0.048
#&gt; SRR1812726     2  0.0693     0.7863 0.000 0.980 0.000 0.012 0.008
#&gt; SRR1812728     3  0.8259     0.2490 0.276 0.172 0.432 0.020 0.100
#&gt; SRR1812724     4  0.2969     0.7312 0.000 0.128 0.000 0.852 0.020
#&gt; SRR1812725     5  0.6692     0.9107 0.000 0.264 0.184 0.020 0.532
#&gt; SRR1812723     2  0.0404     0.7783 0.000 0.988 0.000 0.000 0.012
#&gt; SRR1812722     2  0.0510     0.7864 0.000 0.984 0.000 0.016 0.000
#&gt; SRR1812721     4  0.5113     0.6904 0.008 0.208 0.000 0.700 0.084
#&gt; SRR1812718     2  0.2732     0.6357 0.000 0.840 0.000 0.000 0.160
#&gt; SRR1812717     4  0.3663     0.7148 0.000 0.208 0.000 0.776 0.016
#&gt; SRR1812716     5  0.6771     0.9112 0.000 0.264 0.184 0.024 0.528
#&gt; SRR1812715     2  0.0510     0.7864 0.000 0.984 0.000 0.016 0.000
#&gt; SRR1812714     2  0.3635     0.4653 0.000 0.748 0.004 0.000 0.248
#&gt; SRR1812719     3  0.5671     0.4789 0.400 0.000 0.536 0.016 0.048
#&gt; SRR1812713     2  0.6526     0.0948 0.000 0.452 0.000 0.204 0.344
#&gt; SRR1812712     4  0.6660     0.2747 0.000 0.384 0.000 0.388 0.228
#&gt; SRR1812711     2  0.0880     0.7689 0.000 0.968 0.000 0.000 0.032
#&gt; SRR1812710     2  0.3209     0.6415 0.000 0.812 0.000 0.180 0.008
#&gt; SRR1812709     4  0.6660     0.2747 0.000 0.384 0.000 0.388 0.228
#&gt; SRR1812708     2  0.6024     0.2661 0.152 0.588 0.004 0.000 0.256
#&gt; SRR1812707     2  0.3209     0.6415 0.000 0.812 0.000 0.180 0.008
#&gt; SRR1812705     2  0.0880     0.7695 0.000 0.968 0.000 0.000 0.032
#&gt; SRR1812706     4  0.8502    -0.1939 0.008 0.176 0.168 0.368 0.280
#&gt; SRR1812704     5  0.7484     0.8611 0.000 0.264 0.176 0.076 0.484
#&gt; SRR1812703     2  0.3550     0.4821 0.000 0.760 0.004 0.000 0.236
#&gt; SRR1812702     5  0.6692     0.9107 0.000 0.264 0.184 0.020 0.532
#&gt; SRR1812741     4  0.2329     0.7270 0.000 0.124 0.000 0.876 0.000
#&gt; SRR1812740     3  0.2777     0.7261 0.000 0.000 0.864 0.016 0.120
#&gt; SRR1812739     4  0.5008     0.5615 0.000 0.300 0.000 0.644 0.056
#&gt; SRR1812738     4  0.2909     0.7318 0.000 0.140 0.000 0.848 0.012
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-4-a').click(function(){
  $('#tab-MAD-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-hclust-get-classes-5'>
<p><a id='tab-MAD-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.2730     0.4367 0.808 0.000 0.000 0.000 0.000 0.192
#&gt; SRR1812753     1  0.2730     0.4367 0.808 0.000 0.000 0.000 0.000 0.192
#&gt; SRR1812751     1  0.2969     0.7366 0.776 0.224 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.2969     0.7366 0.776 0.224 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0363     0.7043 0.000 0.000 0.988 0.012 0.000 0.000
#&gt; SRR1812749     1  0.2969     0.7366 0.776 0.224 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.4121     0.6910 0.004 0.000 0.736 0.000 0.060 0.200
#&gt; SRR1812745     3  0.3992     0.6938 0.000 0.000 0.748 0.000 0.072 0.180
#&gt; SRR1812747     2  0.1531     0.7018 0.000 0.928 0.000 0.004 0.068 0.000
#&gt; SRR1812744     3  0.6553     0.3047 0.000 0.000 0.428 0.084 0.104 0.384
#&gt; SRR1812743     4  0.3654     0.5168 0.000 0.000 0.004 0.792 0.144 0.060
#&gt; SRR1812742     4  0.4511     0.4734 0.000 0.000 0.028 0.736 0.168 0.068
#&gt; SRR1812737     2  0.2989     0.6143 0.000 0.812 0.000 0.176 0.008 0.004
#&gt; SRR1812735     2  0.0146     0.7275 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1812734     3  0.5166     0.4466 0.000 0.000 0.524 0.000 0.092 0.384
#&gt; SRR1812733     5  0.4753     0.4558 0.000 0.348 0.004 0.052 0.596 0.000
#&gt; SRR1812736     3  0.0291     0.7052 0.000 0.000 0.992 0.000 0.004 0.004
#&gt; SRR1812732     4  0.3481     0.7596 0.000 0.160 0.000 0.792 0.048 0.000
#&gt; SRR1812730     5  0.3104     0.7547 0.000 0.204 0.004 0.004 0.788 0.000
#&gt; SRR1812731     4  0.3088     0.7663 0.000 0.172 0.000 0.808 0.020 0.000
#&gt; SRR1812729     2  0.1257     0.7260 0.000 0.952 0.000 0.020 0.028 0.000
#&gt; SRR1812727     6  0.7641     1.0000 0.240 0.000 0.140 0.016 0.196 0.408
#&gt; SRR1812726     2  0.0508     0.7267 0.000 0.984 0.000 0.000 0.012 0.004
#&gt; SRR1812728     5  0.9191    -0.5469 0.220 0.144 0.128 0.024 0.276 0.208
#&gt; SRR1812724     4  0.3054     0.7789 0.000 0.116 0.000 0.840 0.040 0.004
#&gt; SRR1812725     5  0.2964     0.7543 0.000 0.204 0.004 0.000 0.792 0.000
#&gt; SRR1812723     2  0.0790     0.7168 0.000 0.968 0.000 0.000 0.032 0.000
#&gt; SRR1812722     2  0.0146     0.7275 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1812721     4  0.6762     0.4112 0.000 0.204 0.000 0.400 0.052 0.344
#&gt; SRR1812718     2  0.2902     0.5783 0.000 0.800 0.000 0.004 0.196 0.000
#&gt; SRR1812717     4  0.3849     0.7397 0.000 0.208 0.000 0.752 0.032 0.008
#&gt; SRR1812716     5  0.3104     0.7547 0.000 0.204 0.004 0.004 0.788 0.000
#&gt; SRR1812715     2  0.0146     0.7275 0.000 0.996 0.000 0.004 0.000 0.000
#&gt; SRR1812714     2  0.4164     0.4359 0.004 0.708 0.004 0.000 0.252 0.032
#&gt; SRR1812719     6  0.7641     1.0000 0.240 0.000 0.140 0.016 0.196 0.408
#&gt; SRR1812713     2  0.6156     0.1033 0.000 0.408 0.000 0.192 0.388 0.012
#&gt; SRR1812712     2  0.7495     0.0653 0.000 0.380 0.000 0.184 0.228 0.208
#&gt; SRR1812711     2  0.1082     0.7119 0.000 0.956 0.000 0.000 0.040 0.004
#&gt; SRR1812710     2  0.2989     0.6143 0.000 0.812 0.000 0.176 0.008 0.004
#&gt; SRR1812709     2  0.7495     0.0653 0.000 0.380 0.000 0.184 0.228 0.208
#&gt; SRR1812708     2  0.6171     0.2659 0.164 0.548 0.004 0.000 0.252 0.032
#&gt; SRR1812707     2  0.2989     0.6143 0.000 0.812 0.000 0.176 0.008 0.004
#&gt; SRR1812705     2  0.1285     0.7089 0.000 0.944 0.000 0.004 0.052 0.000
#&gt; SRR1812706     5  0.6868     0.3022 0.000 0.144 0.004 0.080 0.440 0.332
#&gt; SRR1812704     5  0.4232     0.7242 0.000 0.204 0.004 0.056 0.732 0.004
#&gt; SRR1812703     2  0.3932     0.4506 0.000 0.720 0.004 0.000 0.248 0.028
#&gt; SRR1812702     5  0.2964     0.7543 0.000 0.204 0.004 0.000 0.792 0.000
#&gt; SRR1812741     4  0.2536     0.7763 0.000 0.116 0.000 0.864 0.020 0.000
#&gt; SRR1812740     3  0.0363     0.7043 0.000 0.000 0.988 0.012 0.000 0.000
#&gt; SRR1812739     4  0.4718     0.5451 0.000 0.292 0.000 0.632 0.076 0.000
#&gt; SRR1812738     4  0.3010     0.7805 0.000 0.132 0.000 0.836 0.028 0.004
</code></pre>

<script>
$('#tab-MAD-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-hclust-get-classes-5-a').click(function(){
  $('#tab-MAD-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-hclust-signature_compare](figure_cola/MAD-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-hclust-collect-classes](figure_cola/MAD-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "kmeans"]
# you can also extract it by
# res = res_list["MAD:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-kmeans-collect-plots](figure_cola/MAD-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-kmeans-select-partition-number](figure_cola/MAD-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.369           0.781       0.862         0.4705 0.492   0.492
#> 3 3 0.490           0.542       0.758         0.3222 0.775   0.582
#> 4 4 0.504           0.638       0.727         0.1544 0.780   0.477
#> 5 5 0.705           0.787       0.859         0.0844 0.896   0.641
#> 6 6 0.798           0.744       0.801         0.0466 0.967   0.857
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-kmeans-get-classes-1'>
<p><a id='tab-MAD-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000      0.755 1.000 0.000
#&gt; SRR1812753     1   0.000      0.755 1.000 0.000
#&gt; SRR1812751     2   0.913      0.574 0.328 0.672
#&gt; SRR1812750     2   0.900      0.589 0.316 0.684
#&gt; SRR1812748     1   0.605      0.836 0.852 0.148
#&gt; SRR1812749     2   0.913      0.574 0.328 0.672
#&gt; SRR1812746     1   0.204      0.777 0.968 0.032
#&gt; SRR1812745     1   0.605      0.836 0.852 0.148
#&gt; SRR1812747     2   0.000      0.884 0.000 1.000
#&gt; SRR1812744     1   0.730      0.824 0.796 0.204
#&gt; SRR1812743     1   0.936      0.711 0.648 0.352
#&gt; SRR1812742     1   0.653      0.828 0.832 0.168
#&gt; SRR1812737     2   0.000      0.884 0.000 1.000
#&gt; SRR1812735     2   0.000      0.884 0.000 1.000
#&gt; SRR1812734     1   0.204      0.777 0.968 0.032
#&gt; SRR1812733     1   0.921      0.738 0.664 0.336
#&gt; SRR1812736     1   0.605      0.836 0.852 0.148
#&gt; SRR1812732     1   0.963      0.675 0.612 0.388
#&gt; SRR1812730     1   0.662      0.837 0.828 0.172
#&gt; SRR1812731     2   0.662      0.685 0.172 0.828
#&gt; SRR1812729     2   0.000      0.884 0.000 1.000
#&gt; SRR1812727     1   0.000      0.755 1.000 0.000
#&gt; SRR1812726     2   0.000      0.884 0.000 1.000
#&gt; SRR1812728     1   0.795      0.817 0.760 0.240
#&gt; SRR1812724     1   0.975      0.640 0.592 0.408
#&gt; SRR1812725     2   0.929      0.194 0.344 0.656
#&gt; SRR1812723     2   0.000      0.884 0.000 1.000
#&gt; SRR1812722     2   0.000      0.884 0.000 1.000
#&gt; SRR1812721     2   0.833      0.515 0.264 0.736
#&gt; SRR1812718     2   0.000      0.884 0.000 1.000
#&gt; SRR1812717     2   0.000      0.884 0.000 1.000
#&gt; SRR1812716     1   0.795      0.817 0.760 0.240
#&gt; SRR1812715     2   0.000      0.884 0.000 1.000
#&gt; SRR1812714     2   0.000      0.884 0.000 1.000
#&gt; SRR1812719     1   0.184      0.775 0.972 0.028
#&gt; SRR1812713     2   0.343      0.838 0.064 0.936
#&gt; SRR1812712     2   0.343      0.838 0.064 0.936
#&gt; SRR1812711     2   0.000      0.884 0.000 1.000
#&gt; SRR1812710     2   0.000      0.884 0.000 1.000
#&gt; SRR1812709     2   0.311      0.844 0.056 0.944
#&gt; SRR1812708     2   0.895      0.587 0.312 0.688
#&gt; SRR1812707     2   0.000      0.884 0.000 1.000
#&gt; SRR1812705     2   0.000      0.884 0.000 1.000
#&gt; SRR1812706     1   0.788      0.819 0.764 0.236
#&gt; SRR1812704     1   0.943      0.713 0.640 0.360
#&gt; SRR1812703     2   0.000      0.884 0.000 1.000
#&gt; SRR1812702     1   0.689      0.835 0.816 0.184
#&gt; SRR1812741     1   0.943      0.700 0.640 0.360
#&gt; SRR1812740     1   0.605      0.836 0.852 0.148
#&gt; SRR1812739     2   0.343      0.838 0.064 0.936
#&gt; SRR1812738     1   0.943      0.713 0.640 0.360
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-2'>
<p><a id='tab-MAD-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.5465      0.203 0.712 0.000 0.288
#&gt; SRR1812753     1  0.5465      0.203 0.712 0.000 0.288
#&gt; SRR1812751     1  0.6783      0.412 0.588 0.396 0.016
#&gt; SRR1812750     1  0.6783      0.412 0.588 0.396 0.016
#&gt; SRR1812748     3  0.0424      0.684 0.008 0.000 0.992
#&gt; SRR1812749     1  0.6783      0.412 0.588 0.396 0.016
#&gt; SRR1812746     3  0.1411      0.669 0.036 0.000 0.964
#&gt; SRR1812745     3  0.0000      0.684 0.000 0.000 1.000
#&gt; SRR1812747     2  0.0000      0.774 0.000 1.000 0.000
#&gt; SRR1812744     3  0.7970      0.522 0.300 0.088 0.612
#&gt; SRR1812743     3  0.9808      0.102 0.368 0.240 0.392
#&gt; SRR1812742     3  0.8261      0.472 0.260 0.124 0.616
#&gt; SRR1812737     2  0.2165      0.767 0.064 0.936 0.000
#&gt; SRR1812735     2  0.1411      0.775 0.036 0.964 0.000
#&gt; SRR1812734     3  0.0237      0.683 0.004 0.000 0.996
#&gt; SRR1812733     3  0.6668      0.625 0.264 0.040 0.696
#&gt; SRR1812736     3  0.0237      0.683 0.004 0.000 0.996
#&gt; SRR1812732     3  0.9894      0.106 0.324 0.276 0.400
#&gt; SRR1812730     3  0.4249      0.689 0.108 0.028 0.864
#&gt; SRR1812731     2  0.7533      0.457 0.392 0.564 0.044
#&gt; SRR1812729     2  0.0592      0.773 0.012 0.988 0.000
#&gt; SRR1812727     3  0.4702      0.613 0.212 0.000 0.788
#&gt; SRR1812726     2  0.0000      0.774 0.000 1.000 0.000
#&gt; SRR1812728     3  0.7523      0.638 0.260 0.080 0.660
#&gt; SRR1812724     2  0.9544      0.154 0.388 0.420 0.192
#&gt; SRR1812725     3  0.9766      0.218 0.236 0.348 0.416
#&gt; SRR1812723     2  0.0237      0.773 0.004 0.996 0.000
#&gt; SRR1812722     2  0.0000      0.774 0.000 1.000 0.000
#&gt; SRR1812721     2  0.7004      0.438 0.428 0.552 0.020
#&gt; SRR1812718     2  0.0747      0.771 0.016 0.984 0.000
#&gt; SRR1812717     2  0.5560      0.625 0.300 0.700 0.000
#&gt; SRR1812716     3  0.6728      0.661 0.184 0.080 0.736
#&gt; SRR1812715     2  0.1529      0.774 0.040 0.960 0.000
#&gt; SRR1812714     2  0.0237      0.773 0.004 0.996 0.000
#&gt; SRR1812719     3  0.4291      0.659 0.180 0.000 0.820
#&gt; SRR1812713     2  0.6501      0.594 0.316 0.664 0.020
#&gt; SRR1812712     2  0.6445      0.595 0.308 0.672 0.020
#&gt; SRR1812711     2  0.0237      0.773 0.004 0.996 0.000
#&gt; SRR1812710     2  0.1411      0.775 0.036 0.964 0.000
#&gt; SRR1812709     2  0.5956      0.602 0.324 0.672 0.004
#&gt; SRR1812708     1  0.6659      0.312 0.532 0.460 0.008
#&gt; SRR1812707     2  0.2066      0.769 0.060 0.940 0.000
#&gt; SRR1812705     2  0.0237      0.773 0.004 0.996 0.000
#&gt; SRR1812706     3  0.7186      0.650 0.224 0.080 0.696
#&gt; SRR1812704     1  0.9942     -0.228 0.380 0.288 0.332
#&gt; SRR1812703     2  0.1031      0.763 0.024 0.976 0.000
#&gt; SRR1812702     3  0.6054      0.675 0.180 0.052 0.768
#&gt; SRR1812741     1  0.9887     -0.180 0.396 0.268 0.336
#&gt; SRR1812740     3  0.0424      0.684 0.008 0.000 0.992
#&gt; SRR1812739     2  0.7442      0.557 0.316 0.628 0.056
#&gt; SRR1812738     1  0.9857     -0.237 0.380 0.252 0.368
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-3'>
<p><a id='tab-MAD-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.2722   0.693286 0.904 0.000 0.064 0.032
#&gt; SRR1812753     1  0.2722   0.693286 0.904 0.000 0.064 0.032
#&gt; SRR1812751     1  0.3873   0.821533 0.772 0.228 0.000 0.000
#&gt; SRR1812750     1  0.3873   0.821533 0.772 0.228 0.000 0.000
#&gt; SRR1812748     3  0.1356   0.638793 0.008 0.000 0.960 0.032
#&gt; SRR1812749     1  0.3873   0.821533 0.772 0.228 0.000 0.000
#&gt; SRR1812746     3  0.0592   0.653523 0.016 0.000 0.984 0.000
#&gt; SRR1812745     3  0.0336   0.657874 0.000 0.000 0.992 0.008
#&gt; SRR1812747     2  0.0592   0.805068 0.000 0.984 0.000 0.016
#&gt; SRR1812744     4  0.5284   0.442830 0.004 0.040 0.240 0.716
#&gt; SRR1812743     4  0.5862   0.675832 0.152 0.044 0.060 0.744
#&gt; SRR1812742     4  0.7959   0.341385 0.160 0.028 0.312 0.500
#&gt; SRR1812737     2  0.3688   0.696738 0.000 0.792 0.000 0.208
#&gt; SRR1812735     2  0.2814   0.757420 0.000 0.868 0.000 0.132
#&gt; SRR1812734     3  0.0672   0.650393 0.008 0.000 0.984 0.008
#&gt; SRR1812733     3  0.8228   0.499210 0.048 0.132 0.448 0.372
#&gt; SRR1812736     3  0.0672   0.654073 0.008 0.000 0.984 0.008
#&gt; SRR1812732     4  0.4971   0.699455 0.024 0.064 0.112 0.800
#&gt; SRR1812730     3  0.8109   0.632758 0.068 0.120 0.540 0.272
#&gt; SRR1812731     4  0.3903   0.737137 0.076 0.080 0.000 0.844
#&gt; SRR1812729     2  0.0707   0.807537 0.000 0.980 0.000 0.020
#&gt; SRR1812727     3  0.7695   0.578558 0.232 0.016 0.540 0.212
#&gt; SRR1812726     2  0.0592   0.806901 0.000 0.984 0.000 0.016
#&gt; SRR1812728     3  0.8580   0.587904 0.068 0.152 0.464 0.316
#&gt; SRR1812724     4  0.2596   0.732576 0.024 0.068 0.000 0.908
#&gt; SRR1812725     2  0.9178  -0.502710 0.068 0.316 0.308 0.308
#&gt; SRR1812723     2  0.0469   0.800673 0.000 0.988 0.000 0.012
#&gt; SRR1812722     2  0.0592   0.806901 0.000 0.984 0.000 0.016
#&gt; SRR1812721     4  0.5332   0.701443 0.128 0.124 0.000 0.748
#&gt; SRR1812718     2  0.0592   0.797689 0.000 0.984 0.000 0.016
#&gt; SRR1812717     2  0.5168  -0.000888 0.004 0.504 0.000 0.492
#&gt; SRR1812716     3  0.8538   0.606444 0.068 0.160 0.488 0.284
#&gt; SRR1812715     2  0.3311   0.734578 0.000 0.828 0.000 0.172
#&gt; SRR1812714     2  0.0779   0.805149 0.004 0.980 0.000 0.016
#&gt; SRR1812719     3  0.7891   0.613488 0.132 0.048 0.552 0.268
#&gt; SRR1812713     4  0.5577   0.447645 0.036 0.328 0.000 0.636
#&gt; SRR1812712     4  0.5666   0.416329 0.036 0.348 0.000 0.616
#&gt; SRR1812711     2  0.0336   0.801635 0.000 0.992 0.000 0.008
#&gt; SRR1812710     2  0.3311   0.734578 0.000 0.828 0.000 0.172
#&gt; SRR1812709     4  0.4220   0.618129 0.004 0.248 0.000 0.748
#&gt; SRR1812708     1  0.5742   0.608192 0.596 0.368 0.000 0.036
#&gt; SRR1812707     2  0.3688   0.696738 0.000 0.792 0.000 0.208
#&gt; SRR1812705     2  0.0336   0.801635 0.000 0.992 0.000 0.008
#&gt; SRR1812706     3  0.8544   0.598847 0.068 0.152 0.476 0.304
#&gt; SRR1812704     4  0.6836   0.340056 0.060 0.088 0.172 0.680
#&gt; SRR1812703     2  0.1022   0.785373 0.000 0.968 0.000 0.032
#&gt; SRR1812702     3  0.8522   0.609344 0.068 0.160 0.492 0.280
#&gt; SRR1812741     4  0.4625   0.704842 0.140 0.044 0.012 0.804
#&gt; SRR1812740     3  0.0672   0.654073 0.008 0.000 0.984 0.008
#&gt; SRR1812739     4  0.2589   0.730273 0.000 0.116 0.000 0.884
#&gt; SRR1812738     4  0.1697   0.709161 0.004 0.028 0.016 0.952
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-4'>
<p><a id='tab-MAD-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.2158     0.8184 0.920 0.000 0.020 0.008 0.052
#&gt; SRR1812753     1  0.2158     0.8184 0.920 0.000 0.020 0.008 0.052
#&gt; SRR1812751     1  0.1908     0.8708 0.908 0.092 0.000 0.000 0.000
#&gt; SRR1812750     1  0.1908     0.8708 0.908 0.092 0.000 0.000 0.000
#&gt; SRR1812748     3  0.1117     0.9640 0.000 0.000 0.964 0.020 0.016
#&gt; SRR1812749     1  0.1908     0.8708 0.908 0.092 0.000 0.000 0.000
#&gt; SRR1812746     3  0.1364     0.9737 0.000 0.000 0.952 0.012 0.036
#&gt; SRR1812745     3  0.1331     0.9768 0.000 0.000 0.952 0.008 0.040
#&gt; SRR1812747     2  0.1121     0.8837 0.000 0.956 0.000 0.000 0.044
#&gt; SRR1812744     4  0.4913     0.6967 0.004 0.004 0.076 0.724 0.192
#&gt; SRR1812743     4  0.3386     0.7961 0.092 0.012 0.024 0.860 0.012
#&gt; SRR1812742     4  0.5960     0.6318 0.092 0.012 0.196 0.672 0.028
#&gt; SRR1812737     2  0.3556     0.8010 0.000 0.828 0.008 0.132 0.032
#&gt; SRR1812735     2  0.1243     0.8788 0.000 0.960 0.004 0.028 0.008
#&gt; SRR1812734     3  0.1168     0.9726 0.000 0.000 0.960 0.008 0.032
#&gt; SRR1812733     5  0.3947     0.7777 0.000 0.020 0.072 0.084 0.824
#&gt; SRR1812736     3  0.0880     0.9799 0.000 0.000 0.968 0.000 0.032
#&gt; SRR1812732     4  0.2569     0.8274 0.004 0.016 0.044 0.908 0.028
#&gt; SRR1812730     5  0.3246     0.7920 0.000 0.024 0.120 0.008 0.848
#&gt; SRR1812731     4  0.1278     0.8326 0.004 0.020 0.000 0.960 0.016
#&gt; SRR1812729     2  0.0609     0.8893 0.000 0.980 0.000 0.000 0.020
#&gt; SRR1812727     5  0.4264     0.7227 0.056 0.004 0.084 0.040 0.816
#&gt; SRR1812726     2  0.0798     0.8882 0.008 0.976 0.000 0.000 0.016
#&gt; SRR1812728     5  0.3696     0.7983 0.000 0.032 0.076 0.048 0.844
#&gt; SRR1812724     4  0.1484     0.8304 0.000 0.008 0.000 0.944 0.048
#&gt; SRR1812725     5  0.3307     0.7583 0.000 0.116 0.024 0.012 0.848
#&gt; SRR1812723     2  0.1357     0.8780 0.004 0.948 0.000 0.000 0.048
#&gt; SRR1812722     2  0.0727     0.8890 0.000 0.980 0.004 0.004 0.012
#&gt; SRR1812721     4  0.4120     0.7893 0.052 0.084 0.004 0.824 0.036
#&gt; SRR1812718     2  0.1357     0.8780 0.004 0.948 0.000 0.000 0.048
#&gt; SRR1812717     2  0.5256     0.4571 0.000 0.620 0.008 0.324 0.048
#&gt; SRR1812716     5  0.3283     0.7942 0.000 0.028 0.116 0.008 0.848
#&gt; SRR1812715     2  0.2349     0.8535 0.000 0.900 0.004 0.084 0.012
#&gt; SRR1812714     2  0.1314     0.8838 0.008 0.960 0.004 0.004 0.024
#&gt; SRR1812719     5  0.3291     0.7584 0.008 0.004 0.092 0.036 0.860
#&gt; SRR1812713     5  0.6598     0.0289 0.000 0.184 0.004 0.368 0.444
#&gt; SRR1812712     5  0.6615     0.0318 0.000 0.188 0.004 0.364 0.444
#&gt; SRR1812711     2  0.0898     0.8880 0.008 0.972 0.000 0.000 0.020
#&gt; SRR1812710     2  0.2390     0.8588 0.000 0.908 0.008 0.060 0.024
#&gt; SRR1812709     4  0.5684     0.5753 0.000 0.196 0.004 0.644 0.156
#&gt; SRR1812708     1  0.6205     0.6161 0.636 0.132 0.012 0.016 0.204
#&gt; SRR1812707     2  0.3556     0.8010 0.000 0.828 0.008 0.132 0.032
#&gt; SRR1812705     2  0.1357     0.8775 0.000 0.948 0.000 0.004 0.048
#&gt; SRR1812706     5  0.3344     0.7976 0.000 0.028 0.104 0.016 0.852
#&gt; SRR1812704     5  0.2825     0.7458 0.000 0.016 0.000 0.124 0.860
#&gt; SRR1812703     2  0.3741     0.6739 0.004 0.768 0.004 0.004 0.220
#&gt; SRR1812702     5  0.3283     0.7942 0.000 0.028 0.116 0.008 0.848
#&gt; SRR1812741     4  0.2354     0.8224 0.052 0.012 0.008 0.916 0.012
#&gt; SRR1812740     3  0.0880     0.9799 0.000 0.000 0.968 0.000 0.032
#&gt; SRR1812739     4  0.3283     0.7823 0.000 0.028 0.000 0.832 0.140
#&gt; SRR1812738     4  0.3121     0.7775 0.004 0.004 0.004 0.836 0.152
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-kmeans-get-classes-5'>
<p><a id='tab-MAD-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5 p6
#&gt; SRR1812752     1  0.3460      0.770 0.760 0.000 0.000 0.000 0.020 NA
#&gt; SRR1812753     1  0.3460      0.770 0.760 0.000 0.000 0.000 0.020 NA
#&gt; SRR1812751     1  0.0713      0.837 0.972 0.028 0.000 0.000 0.000 NA
#&gt; SRR1812750     1  0.0713      0.837 0.972 0.028 0.000 0.000 0.000 NA
#&gt; SRR1812748     3  0.0508      0.948 0.000 0.000 0.984 0.004 0.012 NA
#&gt; SRR1812749     1  0.0713      0.837 0.972 0.028 0.000 0.000 0.000 NA
#&gt; SRR1812746     3  0.2461      0.938 0.000 0.000 0.888 0.004 0.044 NA
#&gt; SRR1812745     3  0.2401      0.939 0.000 0.000 0.892 0.004 0.044 NA
#&gt; SRR1812747     2  0.1196      0.849 0.000 0.952 0.000 0.000 0.040 NA
#&gt; SRR1812744     4  0.4230      0.628 0.000 0.008 0.008 0.764 0.076 NA
#&gt; SRR1812743     4  0.4482      0.534 0.016 0.000 0.024 0.644 0.000 NA
#&gt; SRR1812742     4  0.6003      0.429 0.016 0.000 0.164 0.496 0.000 NA
#&gt; SRR1812737     2  0.4209      0.726 0.004 0.708 0.000 0.036 0.004 NA
#&gt; SRR1812735     2  0.2737      0.815 0.004 0.832 0.000 0.004 0.000 NA
#&gt; SRR1812734     3  0.2504      0.923 0.000 0.000 0.880 0.004 0.028 NA
#&gt; SRR1812733     5  0.4218      0.657 0.000 0.004 0.032 0.192 0.748 NA
#&gt; SRR1812736     3  0.0363      0.950 0.000 0.000 0.988 0.000 0.012 NA
#&gt; SRR1812732     4  0.1340      0.677 0.000 0.004 0.008 0.948 0.000 NA
#&gt; SRR1812730     5  0.1594      0.850 0.000 0.016 0.052 0.000 0.932 NA
#&gt; SRR1812731     4  0.1757      0.684 0.000 0.000 0.000 0.916 0.008 NA
#&gt; SRR1812729     2  0.0458      0.857 0.000 0.984 0.000 0.000 0.016 NA
#&gt; SRR1812727     5  0.5514      0.579 0.052 0.000 0.028 0.020 0.616 NA
#&gt; SRR1812726     2  0.0405      0.855 0.000 0.988 0.000 0.000 0.004 NA
#&gt; SRR1812728     5  0.2934      0.839 0.000 0.016 0.024 0.040 0.880 NA
#&gt; SRR1812724     4  0.3316      0.683 0.000 0.000 0.000 0.812 0.052 NA
#&gt; SRR1812725     5  0.2114      0.824 0.000 0.076 0.012 0.000 0.904 NA
#&gt; SRR1812723     2  0.1461      0.845 0.000 0.940 0.000 0.000 0.044 NA
#&gt; SRR1812722     2  0.1493      0.850 0.004 0.936 0.000 0.000 0.004 NA
#&gt; SRR1812721     4  0.5153      0.574 0.000 0.052 0.000 0.528 0.016 NA
#&gt; SRR1812718     2  0.1528      0.844 0.000 0.936 0.000 0.000 0.048 NA
#&gt; SRR1812717     2  0.6134      0.248 0.000 0.460 0.000 0.284 0.008 NA
#&gt; SRR1812716     5  0.1528      0.851 0.000 0.016 0.048 0.000 0.936 NA
#&gt; SRR1812715     2  0.2884      0.812 0.004 0.824 0.000 0.008 0.000 NA
#&gt; SRR1812714     2  0.0363      0.855 0.000 0.988 0.000 0.000 0.000 NA
#&gt; SRR1812719     5  0.4688      0.663 0.012 0.000 0.028 0.020 0.684 NA
#&gt; SRR1812713     4  0.7266      0.304 0.000 0.092 0.000 0.336 0.284 NA
#&gt; SRR1812712     4  0.7327      0.288 0.000 0.100 0.000 0.320 0.288 NA
#&gt; SRR1812711     2  0.0820      0.853 0.000 0.972 0.000 0.000 0.012 NA
#&gt; SRR1812710     2  0.3329      0.777 0.004 0.768 0.000 0.008 0.000 NA
#&gt; SRR1812709     4  0.6501      0.534 0.000 0.112 0.000 0.504 0.088 NA
#&gt; SRR1812708     1  0.6227      0.578 0.604 0.200 0.000 0.016 0.060 NA
#&gt; SRR1812707     2  0.4209      0.726 0.004 0.708 0.000 0.036 0.004 NA
#&gt; SRR1812705     2  0.1745      0.842 0.000 0.924 0.000 0.000 0.056 NA
#&gt; SRR1812706     5  0.3059      0.829 0.000 0.016 0.032 0.016 0.868 NA
#&gt; SRR1812704     5  0.2240      0.820 0.000 0.008 0.000 0.056 0.904 NA
#&gt; SRR1812703     2  0.1921      0.827 0.000 0.916 0.000 0.000 0.052 NA
#&gt; SRR1812702     5  0.1929      0.849 0.000 0.016 0.048 0.004 0.924 NA
#&gt; SRR1812741     4  0.2378      0.649 0.000 0.000 0.000 0.848 0.000 NA
#&gt; SRR1812740     3  0.0363      0.950 0.000 0.000 0.988 0.000 0.012 NA
#&gt; SRR1812739     4  0.3074      0.680 0.000 0.016 0.000 0.856 0.060 NA
#&gt; SRR1812738     4  0.1951      0.684 0.000 0.000 0.000 0.908 0.076 NA
</code></pre>

<script>
$('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-kmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-kmeans-signature_compare](figure_cola/MAD-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-kmeans-collect-classes](figure_cola/MAD-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:skmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "skmeans"]
# you can also extract it by
# res = res_list["MAD:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-skmeans-collect-plots](figure_cola/MAD-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-skmeans-select-partition-number](figure_cola/MAD-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.645           0.897       0.948          0.509 0.490   0.490
#> 3 3 0.630           0.808       0.878          0.322 0.773   0.567
#> 4 4 0.678           0.760       0.875          0.113 0.833   0.552
#> 5 5 0.758           0.659       0.799          0.067 0.912   0.674
#> 6 6 0.717           0.645       0.791          0.042 0.945   0.745
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-classes'>
<ul>
<li><a href='#tab-MAD-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-skmeans-get-classes-1'>
<p><a id='tab-MAD-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000      0.929 1.000 0.000
#&gt; SRR1812753     1   0.000      0.929 1.000 0.000
#&gt; SRR1812751     2   0.722      0.769 0.200 0.800
#&gt; SRR1812750     2   0.722      0.769 0.200 0.800
#&gt; SRR1812748     1   0.000      0.929 1.000 0.000
#&gt; SRR1812749     2   0.722      0.769 0.200 0.800
#&gt; SRR1812746     1   0.000      0.929 1.000 0.000
#&gt; SRR1812745     1   0.000      0.929 1.000 0.000
#&gt; SRR1812747     2   0.000      0.948 0.000 1.000
#&gt; SRR1812744     1   0.000      0.929 1.000 0.000
#&gt; SRR1812743     1   0.706      0.810 0.808 0.192
#&gt; SRR1812742     1   0.000      0.929 1.000 0.000
#&gt; SRR1812737     2   0.000      0.948 0.000 1.000
#&gt; SRR1812735     2   0.000      0.948 0.000 1.000
#&gt; SRR1812734     1   0.000      0.929 1.000 0.000
#&gt; SRR1812733     1   0.644      0.832 0.836 0.164
#&gt; SRR1812736     1   0.000      0.929 1.000 0.000
#&gt; SRR1812732     1   0.706      0.810 0.808 0.192
#&gt; SRR1812730     1   0.000      0.929 1.000 0.000
#&gt; SRR1812731     2   0.430      0.868 0.088 0.912
#&gt; SRR1812729     2   0.000      0.948 0.000 1.000
#&gt; SRR1812727     1   0.000      0.929 1.000 0.000
#&gt; SRR1812726     2   0.000      0.948 0.000 1.000
#&gt; SRR1812728     1   0.000      0.929 1.000 0.000
#&gt; SRR1812724     1   0.722      0.802 0.800 0.200
#&gt; SRR1812725     1   0.697      0.742 0.812 0.188
#&gt; SRR1812723     2   0.000      0.948 0.000 1.000
#&gt; SRR1812722     2   0.000      0.948 0.000 1.000
#&gt; SRR1812721     2   0.821      0.617 0.256 0.744
#&gt; SRR1812718     2   0.000      0.948 0.000 1.000
#&gt; SRR1812717     2   0.000      0.948 0.000 1.000
#&gt; SRR1812716     1   0.000      0.929 1.000 0.000
#&gt; SRR1812715     2   0.000      0.948 0.000 1.000
#&gt; SRR1812714     2   0.000      0.948 0.000 1.000
#&gt; SRR1812719     1   0.000      0.929 1.000 0.000
#&gt; SRR1812713     2   0.000      0.948 0.000 1.000
#&gt; SRR1812712     2   0.000      0.948 0.000 1.000
#&gt; SRR1812711     2   0.000      0.948 0.000 1.000
#&gt; SRR1812710     2   0.000      0.948 0.000 1.000
#&gt; SRR1812709     2   0.000      0.948 0.000 1.000
#&gt; SRR1812708     2   0.722      0.769 0.200 0.800
#&gt; SRR1812707     2   0.000      0.948 0.000 1.000
#&gt; SRR1812705     2   0.000      0.948 0.000 1.000
#&gt; SRR1812706     1   0.000      0.929 1.000 0.000
#&gt; SRR1812704     1   0.714      0.806 0.804 0.196
#&gt; SRR1812703     2   0.000      0.948 0.000 1.000
#&gt; SRR1812702     1   0.000      0.929 1.000 0.000
#&gt; SRR1812741     1   0.706      0.810 0.808 0.192
#&gt; SRR1812740     1   0.000      0.929 1.000 0.000
#&gt; SRR1812739     2   0.000      0.948 0.000 1.000
#&gt; SRR1812738     1   0.722      0.802 0.800 0.200
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-1-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-2'>
<p><a id='tab-MAD-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     3  0.3879      0.824 0.152 0.000 0.848
#&gt; SRR1812753     3  0.3879      0.824 0.152 0.000 0.848
#&gt; SRR1812751     2  0.3921      0.834 0.080 0.884 0.036
#&gt; SRR1812750     2  0.3921      0.834 0.080 0.884 0.036
#&gt; SRR1812748     3  0.0424      0.886 0.008 0.000 0.992
#&gt; SRR1812749     2  0.3921      0.834 0.080 0.884 0.036
#&gt; SRR1812746     3  0.1289      0.881 0.032 0.000 0.968
#&gt; SRR1812745     3  0.0592      0.886 0.012 0.000 0.988
#&gt; SRR1812747     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812744     3  0.5882      0.511 0.348 0.000 0.652
#&gt; SRR1812743     1  0.3116      0.841 0.892 0.000 0.108
#&gt; SRR1812742     3  0.5733      0.561 0.324 0.000 0.676
#&gt; SRR1812737     2  0.6008      0.404 0.372 0.628 0.000
#&gt; SRR1812735     2  0.3619      0.795 0.136 0.864 0.000
#&gt; SRR1812734     3  0.1031      0.884 0.024 0.000 0.976
#&gt; SRR1812733     3  0.3755      0.849 0.120 0.008 0.872
#&gt; SRR1812736     3  0.0424      0.886 0.008 0.000 0.992
#&gt; SRR1812732     1  0.3551      0.840 0.868 0.000 0.132
#&gt; SRR1812730     3  0.3310      0.870 0.064 0.028 0.908
#&gt; SRR1812731     1  0.3896      0.854 0.888 0.052 0.060
#&gt; SRR1812729     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812727     3  0.2448      0.869 0.076 0.000 0.924
#&gt; SRR1812726     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812728     3  0.3590      0.870 0.076 0.028 0.896
#&gt; SRR1812724     1  0.2903      0.859 0.924 0.028 0.048
#&gt; SRR1812725     3  0.6004      0.761 0.064 0.156 0.780
#&gt; SRR1812723     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812722     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812721     1  0.2301      0.859 0.936 0.060 0.004
#&gt; SRR1812718     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812717     1  0.6026      0.404 0.624 0.376 0.000
#&gt; SRR1812716     3  0.3310      0.870 0.064 0.028 0.908
#&gt; SRR1812715     2  0.4702      0.711 0.212 0.788 0.000
#&gt; SRR1812714     2  0.0424      0.884 0.008 0.992 0.000
#&gt; SRR1812719     3  0.2261      0.872 0.068 0.000 0.932
#&gt; SRR1812713     1  0.5075      0.837 0.836 0.096 0.068
#&gt; SRR1812712     1  0.5085      0.837 0.836 0.092 0.072
#&gt; SRR1812711     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812710     2  0.4346      0.747 0.184 0.816 0.000
#&gt; SRR1812709     1  0.2878      0.849 0.904 0.096 0.000
#&gt; SRR1812708     2  0.3832      0.836 0.076 0.888 0.036
#&gt; SRR1812707     2  0.6008      0.404 0.372 0.628 0.000
#&gt; SRR1812705     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812706     3  0.3310      0.870 0.064 0.028 0.908
#&gt; SRR1812704     1  0.6307      0.506 0.660 0.012 0.328
#&gt; SRR1812703     2  0.0000      0.887 0.000 1.000 0.000
#&gt; SRR1812702     3  0.3310      0.870 0.064 0.028 0.908
#&gt; SRR1812741     1  0.2448      0.835 0.924 0.000 0.076
#&gt; SRR1812740     3  0.0424      0.886 0.008 0.000 0.992
#&gt; SRR1812739     1  0.3769      0.847 0.880 0.104 0.016
#&gt; SRR1812738     1  0.3551      0.840 0.868 0.000 0.132
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-2-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-3'>
<p><a id='tab-MAD-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0937     0.8079 0.976 0.000 0.012 0.012
#&gt; SRR1812753     1  0.0937     0.8079 0.976 0.000 0.012 0.012
#&gt; SRR1812751     1  0.2589     0.8360 0.884 0.116 0.000 0.000
#&gt; SRR1812750     1  0.2589     0.8360 0.884 0.116 0.000 0.000
#&gt; SRR1812748     3  0.3392     0.8876 0.072 0.000 0.872 0.056
#&gt; SRR1812749     1  0.2589     0.8360 0.884 0.116 0.000 0.000
#&gt; SRR1812746     3  0.3710     0.8040 0.192 0.000 0.804 0.004
#&gt; SRR1812745     3  0.2466     0.9039 0.056 0.000 0.916 0.028
#&gt; SRR1812747     2  0.0804     0.8966 0.012 0.980 0.008 0.000
#&gt; SRR1812744     4  0.7781     0.0748 0.248 0.000 0.344 0.408
#&gt; SRR1812743     4  0.2101     0.7646 0.060 0.000 0.012 0.928
#&gt; SRR1812742     4  0.7918     0.0243 0.332 0.000 0.316 0.352
#&gt; SRR1812737     2  0.3942     0.7021 0.000 0.764 0.000 0.236
#&gt; SRR1812735     2  0.0707     0.8934 0.000 0.980 0.000 0.020
#&gt; SRR1812734     3  0.3616     0.8726 0.112 0.000 0.852 0.036
#&gt; SRR1812733     3  0.1510     0.8998 0.016 0.000 0.956 0.028
#&gt; SRR1812736     3  0.2965     0.8961 0.072 0.000 0.892 0.036
#&gt; SRR1812732     4  0.2944     0.7524 0.044 0.004 0.052 0.900
#&gt; SRR1812730     3  0.0927     0.9089 0.008 0.016 0.976 0.000
#&gt; SRR1812731     4  0.0895     0.7770 0.020 0.004 0.000 0.976
#&gt; SRR1812729     2  0.0672     0.8984 0.008 0.984 0.000 0.008
#&gt; SRR1812727     1  0.2530     0.7731 0.888 0.000 0.112 0.000
#&gt; SRR1812726     2  0.0336     0.8983 0.008 0.992 0.000 0.000
#&gt; SRR1812728     3  0.1884     0.9051 0.020 0.016 0.948 0.016
#&gt; SRR1812724     4  0.1271     0.7767 0.012 0.008 0.012 0.968
#&gt; SRR1812725     3  0.3166     0.8127 0.016 0.116 0.868 0.000
#&gt; SRR1812723     2  0.0927     0.8928 0.008 0.976 0.016 0.000
#&gt; SRR1812722     2  0.0000     0.8987 0.000 1.000 0.000 0.000
#&gt; SRR1812721     4  0.2529     0.7681 0.024 0.048 0.008 0.920
#&gt; SRR1812718     2  0.0804     0.8966 0.012 0.980 0.008 0.000
#&gt; SRR1812717     2  0.5334     0.1118 0.004 0.508 0.004 0.484
#&gt; SRR1812716     3  0.0779     0.9073 0.004 0.016 0.980 0.000
#&gt; SRR1812715     2  0.2469     0.8428 0.000 0.892 0.000 0.108
#&gt; SRR1812714     2  0.0707     0.8950 0.020 0.980 0.000 0.000
#&gt; SRR1812719     1  0.4989    -0.0249 0.528 0.000 0.472 0.000
#&gt; SRR1812713     4  0.5787     0.6451 0.016 0.168 0.084 0.732
#&gt; SRR1812712     4  0.5940     0.6754 0.016 0.124 0.132 0.728
#&gt; SRR1812711     2  0.0524     0.8977 0.008 0.988 0.004 0.000
#&gt; SRR1812710     2  0.2216     0.8549 0.000 0.908 0.000 0.092
#&gt; SRR1812709     4  0.3345     0.7246 0.012 0.124 0.004 0.860
#&gt; SRR1812708     1  0.2589     0.8334 0.884 0.116 0.000 0.000
#&gt; SRR1812707     2  0.3873     0.7129 0.000 0.772 0.000 0.228
#&gt; SRR1812705     2  0.1042     0.8899 0.008 0.972 0.020 0.000
#&gt; SRR1812706     3  0.1762     0.9001 0.012 0.016 0.952 0.020
#&gt; SRR1812704     4  0.5408     0.1582 0.012 0.000 0.488 0.500
#&gt; SRR1812703     2  0.0592     0.8984 0.016 0.984 0.000 0.000
#&gt; SRR1812702     3  0.0779     0.9075 0.004 0.016 0.980 0.000
#&gt; SRR1812741     4  0.1489     0.7724 0.044 0.000 0.004 0.952
#&gt; SRR1812740     3  0.2816     0.8993 0.064 0.000 0.900 0.036
#&gt; SRR1812739     4  0.1262     0.7783 0.008 0.016 0.008 0.968
#&gt; SRR1812738     4  0.0937     0.7775 0.012 0.000 0.012 0.976
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-3-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-4'>
<p><a id='tab-MAD-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.1278     0.8793 0.960 0.000 0.020 0.004 0.016
#&gt; SRR1812753     1  0.1372     0.8781 0.956 0.000 0.024 0.004 0.016
#&gt; SRR1812751     1  0.0703     0.8889 0.976 0.024 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0703     0.8889 0.976 0.024 0.000 0.000 0.000
#&gt; SRR1812748     3  0.1205     0.8324 0.004 0.000 0.956 0.040 0.000
#&gt; SRR1812749     1  0.0609     0.8894 0.980 0.020 0.000 0.000 0.000
#&gt; SRR1812746     3  0.2522     0.7473 0.108 0.000 0.880 0.000 0.012
#&gt; SRR1812745     3  0.0510     0.8327 0.000 0.000 0.984 0.000 0.016
#&gt; SRR1812747     2  0.0771     0.8870 0.004 0.976 0.000 0.000 0.020
#&gt; SRR1812744     3  0.5646     0.2883 0.016 0.000 0.616 0.300 0.068
#&gt; SRR1812743     4  0.3497     0.6439 0.012 0.000 0.140 0.828 0.020
#&gt; SRR1812742     4  0.5751     0.0755 0.044 0.000 0.448 0.488 0.020
#&gt; SRR1812737     2  0.5136     0.6527 0.000 0.688 0.000 0.196 0.116
#&gt; SRR1812735     2  0.1211     0.8790 0.000 0.960 0.000 0.016 0.024
#&gt; SRR1812734     3  0.1153     0.8417 0.024 0.000 0.964 0.004 0.008
#&gt; SRR1812733     5  0.5032     0.4361 0.000 0.000 0.448 0.032 0.520
#&gt; SRR1812736     3  0.0162     0.8440 0.004 0.000 0.996 0.000 0.000
#&gt; SRR1812732     4  0.4961     0.1078 0.000 0.000 0.448 0.524 0.028
#&gt; SRR1812730     5  0.4235     0.5525 0.000 0.000 0.424 0.000 0.576
#&gt; SRR1812731     4  0.1267     0.6983 0.012 0.000 0.004 0.960 0.024
#&gt; SRR1812729     2  0.0451     0.8870 0.004 0.988 0.000 0.000 0.008
#&gt; SRR1812727     1  0.3693     0.8046 0.828 0.000 0.080 0.004 0.088
#&gt; SRR1812726     2  0.0566     0.8866 0.012 0.984 0.000 0.000 0.004
#&gt; SRR1812728     5  0.4852     0.5401 0.016 0.000 0.380 0.008 0.596
#&gt; SRR1812724     4  0.3196     0.6270 0.004 0.000 0.000 0.804 0.192
#&gt; SRR1812725     5  0.5309     0.5476 0.000 0.060 0.364 0.000 0.576
#&gt; SRR1812723     2  0.0898     0.8855 0.008 0.972 0.000 0.000 0.020
#&gt; SRR1812722     2  0.0162     0.8864 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1812721     4  0.4645     0.5451 0.012 0.016 0.000 0.672 0.300
#&gt; SRR1812718     2  0.0992     0.8846 0.008 0.968 0.000 0.000 0.024
#&gt; SRR1812717     2  0.6170     0.3443 0.000 0.524 0.000 0.320 0.156
#&gt; SRR1812716     5  0.4210     0.5624 0.000 0.000 0.412 0.000 0.588
#&gt; SRR1812715     2  0.2813     0.8370 0.000 0.876 0.000 0.084 0.040
#&gt; SRR1812714     2  0.1484     0.8713 0.048 0.944 0.000 0.000 0.008
#&gt; SRR1812719     1  0.6304     0.3191 0.532 0.000 0.248 0.000 0.220
#&gt; SRR1812713     5  0.5078    -0.2727 0.004 0.028 0.000 0.424 0.544
#&gt; SRR1812712     5  0.5137    -0.2640 0.004 0.032 0.000 0.416 0.548
#&gt; SRR1812711     2  0.0807     0.8861 0.012 0.976 0.000 0.000 0.012
#&gt; SRR1812710     2  0.2863     0.8388 0.000 0.876 0.000 0.064 0.060
#&gt; SRR1812709     4  0.4999     0.4802 0.004 0.032 0.000 0.604 0.360
#&gt; SRR1812708     1  0.0992     0.8867 0.968 0.024 0.000 0.000 0.008
#&gt; SRR1812707     2  0.5039     0.6683 0.000 0.700 0.000 0.184 0.116
#&gt; SRR1812705     2  0.0963     0.8797 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812706     5  0.3920     0.5657 0.004 0.000 0.268 0.004 0.724
#&gt; SRR1812704     5  0.3526     0.4540 0.000 0.000 0.072 0.096 0.832
#&gt; SRR1812703     2  0.2438     0.8487 0.060 0.900 0.000 0.000 0.040
#&gt; SRR1812702     5  0.4227     0.5571 0.000 0.000 0.420 0.000 0.580
#&gt; SRR1812741     4  0.2069     0.7003 0.012 0.000 0.052 0.924 0.012
#&gt; SRR1812740     3  0.0566     0.8396 0.004 0.000 0.984 0.000 0.012
#&gt; SRR1812739     4  0.2784     0.6798 0.000 0.004 0.016 0.872 0.108
#&gt; SRR1812738     4  0.3086     0.6940 0.004 0.000 0.092 0.864 0.040
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-4-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-skmeans-get-classes-5'>
<p><a id='tab-MAD-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.2832     0.8615 0.884 0.000 0.016 0.016 0.044 0.040
#&gt; SRR1812753     1  0.2987     0.8582 0.876 0.000 0.020 0.016 0.044 0.044
#&gt; SRR1812751     1  0.0000     0.8917 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.8917 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.1204     0.8572 0.000 0.000 0.944 0.000 0.000 0.056
#&gt; SRR1812749     1  0.0000     0.8917 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.2009     0.8053 0.068 0.000 0.908 0.000 0.024 0.000
#&gt; SRR1812745     3  0.1152     0.8468 0.000 0.000 0.952 0.004 0.044 0.000
#&gt; SRR1812747     2  0.2221     0.8009 0.004 0.908 0.000 0.040 0.044 0.004
#&gt; SRR1812744     3  0.5124     0.3436 0.008 0.000 0.628 0.044 0.024 0.296
#&gt; SRR1812743     6  0.1857     0.6517 0.000 0.000 0.044 0.028 0.004 0.924
#&gt; SRR1812742     6  0.4438     0.3903 0.004 0.000 0.332 0.012 0.016 0.636
#&gt; SRR1812737     2  0.4713     0.4379 0.000 0.560 0.000 0.400 0.028 0.012
#&gt; SRR1812735     2  0.3485     0.7393 0.000 0.784 0.000 0.184 0.028 0.004
#&gt; SRR1812734     3  0.0820     0.8634 0.000 0.000 0.972 0.000 0.016 0.012
#&gt; SRR1812733     5  0.6203     0.4000 0.000 0.000 0.336 0.140 0.488 0.036
#&gt; SRR1812736     3  0.0622     0.8684 0.000 0.000 0.980 0.000 0.008 0.012
#&gt; SRR1812732     6  0.4536     0.4566 0.000 0.000 0.300 0.036 0.012 0.652
#&gt; SRR1812730     5  0.3151     0.7107 0.000 0.000 0.252 0.000 0.748 0.000
#&gt; SRR1812731     6  0.2793     0.5661 0.000 0.000 0.000 0.200 0.000 0.800
#&gt; SRR1812729     2  0.1719     0.8019 0.000 0.932 0.000 0.032 0.032 0.004
#&gt; SRR1812727     1  0.6429     0.5225 0.584 0.000 0.128 0.048 0.212 0.028
#&gt; SRR1812726     2  0.1957     0.7856 0.048 0.920 0.000 0.008 0.024 0.000
#&gt; SRR1812728     5  0.4387     0.6793 0.004 0.008 0.132 0.060 0.772 0.024
#&gt; SRR1812724     6  0.4395     0.1576 0.000 0.000 0.000 0.404 0.028 0.568
#&gt; SRR1812725     5  0.4489     0.6941 0.000 0.072 0.140 0.024 0.756 0.008
#&gt; SRR1812723     2  0.1464     0.7944 0.016 0.944 0.000 0.004 0.036 0.000
#&gt; SRR1812722     2  0.2176     0.7905 0.000 0.896 0.000 0.080 0.024 0.000
#&gt; SRR1812721     4  0.4223     0.3123 0.000 0.004 0.000 0.612 0.016 0.368
#&gt; SRR1812718     2  0.2577     0.7806 0.024 0.888 0.000 0.008 0.072 0.008
#&gt; SRR1812717     4  0.5715    -0.1436 0.000 0.396 0.000 0.492 0.028 0.084
#&gt; SRR1812716     5  0.3259     0.7251 0.000 0.000 0.216 0.012 0.772 0.000
#&gt; SRR1812715     2  0.4430     0.6813 0.000 0.708 0.000 0.232 0.032 0.028
#&gt; SRR1812714     2  0.3997     0.7255 0.128 0.796 0.000 0.040 0.024 0.012
#&gt; SRR1812719     5  0.7163     0.0477 0.328 0.000 0.192 0.048 0.408 0.024
#&gt; SRR1812713     4  0.3690     0.5994 0.000 0.012 0.000 0.804 0.116 0.068
#&gt; SRR1812712     4  0.3368     0.5996 0.000 0.004 0.000 0.820 0.116 0.060
#&gt; SRR1812711     2  0.1116     0.7937 0.028 0.960 0.000 0.004 0.008 0.000
#&gt; SRR1812710     2  0.3907     0.6703 0.000 0.704 0.000 0.268 0.028 0.000
#&gt; SRR1812709     4  0.3071     0.5486 0.000 0.000 0.000 0.804 0.016 0.180
#&gt; SRR1812708     1  0.1007     0.8793 0.968 0.004 0.000 0.004 0.016 0.008
#&gt; SRR1812707     2  0.4554     0.4541 0.000 0.568 0.000 0.400 0.024 0.008
#&gt; SRR1812705     2  0.1982     0.7975 0.000 0.912 0.000 0.016 0.068 0.004
#&gt; SRR1812706     5  0.4348     0.6908 0.000 0.000 0.124 0.152 0.724 0.000
#&gt; SRR1812704     5  0.4828     0.5616 0.000 0.000 0.036 0.184 0.708 0.072
#&gt; SRR1812703     2  0.5460     0.6248 0.112 0.712 0.008 0.096 0.056 0.016
#&gt; SRR1812702     5  0.3314     0.7193 0.000 0.000 0.224 0.012 0.764 0.000
#&gt; SRR1812741     6  0.1442     0.6448 0.000 0.000 0.004 0.040 0.012 0.944
#&gt; SRR1812740     3  0.1092     0.8646 0.000 0.000 0.960 0.000 0.020 0.020
#&gt; SRR1812739     6  0.4814     0.3405 0.000 0.004 0.020 0.416 0.016 0.544
#&gt; SRR1812738     6  0.4029     0.6137 0.000 0.000 0.028 0.160 0.040 0.772
</code></pre>

<script>
$('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-skmeans-get-classes-5-a').click(function(){
  $('#tab-MAD-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-skmeans-signature_compare](figure_cola/MAD-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-skmeans-collect-classes](figure_cola/MAD-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:pam






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "pam"]
# you can also extract it by
# res = res_list["MAD:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-pam-collect-plots](figure_cola/MAD-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-pam-select-partition-number](figure_cola/MAD-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.764           0.879       0.949         0.5027 0.500   0.500
#> 3 3 0.648           0.801       0.903         0.2011 0.884   0.771
#> 4 4 0.807           0.853       0.942         0.1405 0.911   0.774
#> 5 5 0.831           0.864       0.941         0.1220 0.893   0.665
#> 6 6 0.806           0.740       0.881         0.0473 0.976   0.895
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-classes'>
<ul>
<li><a href='#tab-MAD-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-pam-get-classes-1'>
<p><a id='tab-MAD-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812753     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812751     2  0.5294      0.841 0.120 0.880
#&gt; SRR1812750     2  0.2423      0.923 0.040 0.960
#&gt; SRR1812748     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812749     1  0.2236      0.911 0.964 0.036
#&gt; SRR1812746     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812745     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812747     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812744     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812743     2  0.3114      0.907 0.056 0.944
#&gt; SRR1812742     2  0.9323      0.460 0.348 0.652
#&gt; SRR1812737     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812735     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812734     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812733     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812736     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812732     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812730     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812731     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812729     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812727     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812726     1  0.5178      0.835 0.884 0.116
#&gt; SRR1812728     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812724     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812725     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812723     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812722     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812721     1  0.0376      0.934 0.996 0.004
#&gt; SRR1812718     2  0.1184      0.942 0.016 0.984
#&gt; SRR1812717     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812716     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812715     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812714     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812719     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812713     2  0.9209      0.452 0.336 0.664
#&gt; SRR1812712     1  0.9393      0.473 0.644 0.356
#&gt; SRR1812711     2  0.1414      0.939 0.020 0.980
#&gt; SRR1812710     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812709     1  0.9710      0.370 0.600 0.400
#&gt; SRR1812708     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812707     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812705     1  0.5737      0.812 0.864 0.136
#&gt; SRR1812706     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812704     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812703     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812702     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812741     1  0.9686      0.380 0.604 0.396
#&gt; SRR1812740     1  0.0000      0.936 1.000 0.000
#&gt; SRR1812739     2  0.0000      0.951 0.000 1.000
#&gt; SRR1812738     1  0.7139      0.744 0.804 0.196
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-1-a').click(function(){
  $('#tab-MAD-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-2'>
<p><a id='tab-MAD-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.4555      0.822 0.800 0.000 0.200
#&gt; SRR1812753     1  0.4555      0.822 0.800 0.000 0.200
#&gt; SRR1812751     1  0.4555      0.723 0.800 0.200 0.000
#&gt; SRR1812750     1  0.4555      0.723 0.800 0.200 0.000
#&gt; SRR1812748     3  0.4555      0.749 0.200 0.000 0.800
#&gt; SRR1812749     1  0.5413      0.829 0.800 0.036 0.164
#&gt; SRR1812746     3  0.4555      0.749 0.200 0.000 0.800
#&gt; SRR1812745     3  0.4555      0.749 0.200 0.000 0.800
#&gt; SRR1812747     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812744     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812743     2  0.1964      0.880 0.000 0.944 0.056
#&gt; SRR1812742     2  0.5882      0.385 0.000 0.652 0.348
#&gt; SRR1812737     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812735     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812734     3  0.4555      0.749 0.200 0.000 0.800
#&gt; SRR1812733     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812736     3  0.4555      0.749 0.200 0.000 0.800
#&gt; SRR1812732     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812730     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812729     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812727     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812726     3  0.3267      0.754 0.000 0.116 0.884
#&gt; SRR1812728     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812724     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812725     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812723     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812722     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812721     3  0.0237      0.836 0.000 0.004 0.996
#&gt; SRR1812718     2  0.0747      0.928 0.000 0.984 0.016
#&gt; SRR1812717     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812716     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812719     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812713     2  0.5810      0.425 0.000 0.664 0.336
#&gt; SRR1812712     3  0.5926      0.404 0.000 0.356 0.644
#&gt; SRR1812711     2  0.0892      0.923 0.000 0.980 0.020
#&gt; SRR1812710     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812709     3  0.6126      0.332 0.000 0.400 0.600
#&gt; SRR1812708     1  0.5859      0.633 0.656 0.000 0.344
#&gt; SRR1812707     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812705     3  0.3619      0.746 0.000 0.136 0.864
#&gt; SRR1812706     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812704     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812703     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812702     3  0.0000      0.838 0.000 0.000 1.000
#&gt; SRR1812741     3  0.6111      0.339 0.000 0.396 0.604
#&gt; SRR1812740     3  0.4555      0.749 0.200 0.000 0.800
#&gt; SRR1812739     2  0.0000      0.942 0.000 1.000 0.000
#&gt; SRR1812738     3  0.4504      0.643 0.000 0.196 0.804
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-2-a').click(function(){
  $('#tab-MAD-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-3'>
<p><a id='tab-MAD-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1812749     1  0.0000      0.900 1.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1812745     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1812747     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812744     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812743     2  0.1557      0.897 0.000 0.944 0.000 0.056
#&gt; SRR1812742     2  0.5764      0.459 0.000 0.644 0.052 0.304
#&gt; SRR1812737     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1812733     4  0.3649      0.700 0.000 0.000 0.204 0.796
#&gt; SRR1812736     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1812732     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812730     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812731     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812729     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812727     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812726     4  0.2589      0.777 0.000 0.116 0.000 0.884
#&gt; SRR1812728     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812724     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812725     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812723     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812722     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812721     4  0.0188      0.863 0.000 0.004 0.000 0.996
#&gt; SRR1812718     2  0.0592      0.938 0.000 0.984 0.000 0.016
#&gt; SRR1812717     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812716     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812719     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812713     2  0.4605      0.406 0.000 0.664 0.000 0.336
#&gt; SRR1812712     4  0.4697      0.509 0.000 0.356 0.000 0.644
#&gt; SRR1812711     2  0.0707      0.935 0.000 0.980 0.000 0.020
#&gt; SRR1812710     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812709     4  0.4855      0.416 0.000 0.400 0.000 0.600
#&gt; SRR1812708     1  0.4643      0.425 0.656 0.000 0.000 0.344
#&gt; SRR1812707     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812705     4  0.2868      0.754 0.000 0.136 0.000 0.864
#&gt; SRR1812706     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812704     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812703     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812702     4  0.0000      0.864 0.000 0.000 0.000 1.000
#&gt; SRR1812741     4  0.4843      0.425 0.000 0.396 0.000 0.604
#&gt; SRR1812740     3  0.0000      1.000 0.000 0.000 1.000 0.000
#&gt; SRR1812739     2  0.0000      0.951 0.000 1.000 0.000 0.000
#&gt; SRR1812738     4  0.3569      0.714 0.000 0.196 0.000 0.804
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-3-a').click(function(){
  $('#tab-MAD-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-4'>
<p><a id='tab-MAD-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000      0.912 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.912 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.912 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.912 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812749     1  0.0000      0.912 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812745     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812747     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812744     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812743     2  0.1341      0.893 0.000 0.944 0.000 0.000 0.056
#&gt; SRR1812742     2  0.5068      0.489 0.000 0.640 0.060 0.000 0.300
#&gt; SRR1812737     2  0.2230      0.860 0.000 0.884 0.000 0.116 0.000
#&gt; SRR1812735     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812733     4  0.3090      0.796 0.000 0.000 0.088 0.860 0.052
#&gt; SRR1812736     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812732     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812730     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812731     2  0.2471      0.843 0.000 0.864 0.000 0.136 0.000
#&gt; SRR1812729     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812727     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812726     5  0.3317      0.781 0.000 0.116 0.000 0.044 0.840
#&gt; SRR1812728     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812724     4  0.3274      0.706 0.000 0.220 0.000 0.780 0.000
#&gt; SRR1812725     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812723     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812722     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812721     4  0.0510      0.847 0.000 0.000 0.000 0.984 0.016
#&gt; SRR1812718     2  0.0510      0.930 0.000 0.984 0.000 0.000 0.016
#&gt; SRR1812717     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812716     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812719     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812713     4  0.0000      0.852 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812712     4  0.0000      0.852 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812711     2  0.0798      0.927 0.000 0.976 0.000 0.008 0.016
#&gt; SRR1812710     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812709     4  0.0000      0.852 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812708     1  0.3999      0.459 0.656 0.000 0.000 0.000 0.344
#&gt; SRR1812707     2  0.3210      0.760 0.000 0.788 0.000 0.212 0.000
#&gt; SRR1812705     5  0.2471      0.787 0.000 0.136 0.000 0.000 0.864
#&gt; SRR1812706     4  0.3983      0.519 0.000 0.000 0.000 0.660 0.340
#&gt; SRR1812704     5  0.0510      0.898 0.000 0.000 0.000 0.016 0.984
#&gt; SRR1812703     4  0.2329      0.796 0.000 0.124 0.000 0.876 0.000
#&gt; SRR1812702     5  0.0000      0.908 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812741     5  0.4171      0.385 0.000 0.396 0.000 0.000 0.604
#&gt; SRR1812740     3  0.0000      1.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812739     2  0.0290      0.934 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1812738     5  0.3074      0.719 0.000 0.196 0.000 0.000 0.804
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-4-a').click(function(){
  $('#tab-MAD-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-pam-get-classes-5'>
<p><a id='tab-MAD-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000      0.894 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.894 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.894 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.894 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.3747      0.997 0.000 0.000 0.604 0.000 0.000 0.396
#&gt; SRR1812749     1  0.0000      0.894 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.3737      0.999 0.000 0.000 0.608 0.000 0.000 0.392
#&gt; SRR1812745     3  0.3737      0.999 0.000 0.000 0.608 0.000 0.000 0.392
#&gt; SRR1812747     2  0.3634      0.570 0.000 0.644 0.356 0.000 0.000 0.000
#&gt; SRR1812744     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812743     6  0.3747      0.491 0.000 0.396 0.000 0.000 0.000 0.604
#&gt; SRR1812742     6  0.4764      0.529 0.000 0.108 0.000 0.000 0.232 0.660
#&gt; SRR1812737     2  0.2260      0.731 0.000 0.860 0.000 0.140 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.3737      0.999 0.000 0.000 0.608 0.000 0.000 0.392
#&gt; SRR1812733     4  0.3396      0.733 0.000 0.000 0.000 0.812 0.072 0.116
#&gt; SRR1812736     3  0.3737      0.999 0.000 0.000 0.608 0.000 0.000 0.392
#&gt; SRR1812732     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812730     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812731     2  0.2664      0.701 0.000 0.816 0.000 0.184 0.000 0.000
#&gt; SRR1812729     2  0.2762      0.693 0.000 0.804 0.196 0.000 0.000 0.000
#&gt; SRR1812727     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812726     5  0.5391      0.339 0.000 0.116 0.392 0.000 0.492 0.000
#&gt; SRR1812728     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812724     4  0.3076      0.587 0.000 0.240 0.000 0.760 0.000 0.000
#&gt; SRR1812725     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812723     5  0.3737      0.495 0.000 0.000 0.392 0.000 0.608 0.000
#&gt; SRR1812722     2  0.0146      0.802 0.000 0.996 0.004 0.000 0.000 0.000
#&gt; SRR1812721     4  0.0458      0.813 0.000 0.000 0.000 0.984 0.016 0.000
#&gt; SRR1812718     2  0.3737      0.543 0.000 0.608 0.392 0.000 0.000 0.000
#&gt; SRR1812717     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812716     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812715     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0713      0.796 0.000 0.972 0.028 0.000 0.000 0.000
#&gt; SRR1812719     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812713     4  0.0000      0.819 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812712     4  0.0000      0.819 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812711     2  0.3872      0.538 0.000 0.604 0.392 0.000 0.004 0.000
#&gt; SRR1812710     2  0.0000      0.802 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812709     4  0.0000      0.819 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812708     1  0.3833      0.397 0.648 0.000 0.008 0.000 0.344 0.000
#&gt; SRR1812707     2  0.3076      0.642 0.000 0.760 0.000 0.240 0.000 0.000
#&gt; SRR1812705     5  0.5502      0.333 0.000 0.136 0.364 0.000 0.500 0.000
#&gt; SRR1812706     4  0.3592      0.477 0.000 0.000 0.000 0.656 0.344 0.000
#&gt; SRR1812704     5  0.0363      0.795 0.000 0.000 0.000 0.012 0.988 0.000
#&gt; SRR1812703     4  0.3062      0.727 0.000 0.024 0.160 0.816 0.000 0.000
#&gt; SRR1812702     5  0.0000      0.802 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812741     5  0.3747      0.275 0.000 0.396 0.000 0.000 0.604 0.000
#&gt; SRR1812740     3  0.3747      0.997 0.000 0.000 0.604 0.000 0.000 0.396
#&gt; SRR1812739     2  0.0260      0.801 0.000 0.992 0.000 0.008 0.000 0.000
#&gt; SRR1812738     5  0.2762      0.613 0.000 0.196 0.000 0.000 0.804 0.000
</code></pre>

<script>
$('#tab-MAD-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-pam-get-classes-5-a').click(function(){
  $('#tab-MAD-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-pam-signature_compare](figure_cola/MAD-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-pam-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-pam-collect-classes](figure_cola/MAD-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "mclust"]
# you can also extract it by
# res = res_list["MAD:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-mclust-collect-plots](figure_cola/MAD-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-mclust-select-partition-number](figure_cola/MAD-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.813           0.928       0.943          0.331 0.678   0.678
#> 3 3 0.254           0.352       0.729          0.609 0.768   0.683
#> 4 4 0.336           0.428       0.669          0.191 0.703   0.507
#> 5 5 0.382           0.581       0.724          0.136 0.701   0.371
#> 6 6 0.545           0.665       0.721          0.102 0.864   0.564
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-classes'>
<ul>
<li><a href='#tab-MAD-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-mclust-get-classes-1'>
<p><a id='tab-MAD-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.3733      0.944 0.928 0.072
#&gt; SRR1812753     1  0.3733      0.944 0.928 0.072
#&gt; SRR1812751     1  0.3733      0.944 0.928 0.072
#&gt; SRR1812750     1  0.3733      0.944 0.928 0.072
#&gt; SRR1812748     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812749     1  0.3733      0.944 0.928 0.072
#&gt; SRR1812746     1  0.5294      0.934 0.880 0.120
#&gt; SRR1812745     2  0.5946      0.813 0.144 0.856
#&gt; SRR1812747     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812744     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812743     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812742     2  0.0938      0.946 0.012 0.988
#&gt; SRR1812737     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812735     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812734     1  0.8909      0.685 0.692 0.308
#&gt; SRR1812733     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812736     2  0.6148      0.801 0.152 0.848
#&gt; SRR1812732     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812730     2  0.3431      0.909 0.064 0.936
#&gt; SRR1812731     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812729     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812727     1  0.4939      0.941 0.892 0.108
#&gt; SRR1812726     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812728     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812724     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812725     2  0.2948      0.918 0.052 0.948
#&gt; SRR1812723     2  0.4022      0.934 0.080 0.920
#&gt; SRR1812722     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812721     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812718     2  0.2423      0.944 0.040 0.960
#&gt; SRR1812717     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812716     2  0.3733      0.901 0.072 0.928
#&gt; SRR1812715     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812714     2  0.4690      0.922 0.100 0.900
#&gt; SRR1812719     1  0.6623      0.887 0.828 0.172
#&gt; SRR1812713     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812712     2  0.0000      0.949 0.000 1.000
#&gt; SRR1812711     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812710     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812709     2  0.3584      0.937 0.068 0.932
#&gt; SRR1812708     1  0.4939      0.941 0.892 0.108
#&gt; SRR1812707     2  0.3733      0.935 0.072 0.928
#&gt; SRR1812705     2  0.3879      0.935 0.076 0.924
#&gt; SRR1812706     2  0.3431      0.908 0.064 0.936
#&gt; SRR1812704     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812703     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812702     2  0.1414      0.941 0.020 0.980
#&gt; SRR1812741     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812740     2  0.0376      0.949 0.004 0.996
#&gt; SRR1812739     2  0.2043      0.945 0.032 0.968
#&gt; SRR1812738     2  0.0376      0.949 0.004 0.996
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-1-a').click(function(){
  $('#tab-MAD-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-2'>
<p><a id='tab-MAD-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0892     0.8065 0.980 0.020 0.000
#&gt; SRR1812753     1  0.0892     0.8065 0.980 0.020 0.000
#&gt; SRR1812751     1  0.0892     0.8065 0.980 0.020 0.000
#&gt; SRR1812750     1  0.0892     0.8065 0.980 0.020 0.000
#&gt; SRR1812748     3  0.7536     0.3894 0.064 0.304 0.632
#&gt; SRR1812749     1  0.0892     0.8065 0.980 0.020 0.000
#&gt; SRR1812746     1  0.9550     0.3945 0.408 0.192 0.400
#&gt; SRR1812745     2  0.8286     0.1842 0.104 0.588 0.308
#&gt; SRR1812747     2  0.0892     0.4967 0.020 0.980 0.000
#&gt; SRR1812744     2  0.4963     0.3518 0.008 0.792 0.200
#&gt; SRR1812743     3  0.6819     0.6149 0.028 0.328 0.644
#&gt; SRR1812742     3  0.7389     0.5519 0.036 0.408 0.556
#&gt; SRR1812737     2  0.5621     0.1503 0.000 0.692 0.308
#&gt; SRR1812735     2  0.5431     0.1906 0.000 0.716 0.284
#&gt; SRR1812734     2  0.9492    -0.0919 0.184 0.416 0.400
#&gt; SRR1812733     2  0.4808     0.4593 0.008 0.804 0.188
#&gt; SRR1812736     2  0.8964     0.0375 0.160 0.544 0.296
#&gt; SRR1812732     2  0.6286    -0.2274 0.000 0.536 0.464
#&gt; SRR1812730     2  0.6379     0.2542 0.008 0.624 0.368
#&gt; SRR1812731     2  0.7278    -0.2976 0.028 0.516 0.456
#&gt; SRR1812729     2  0.0892     0.4967 0.020 0.980 0.000
#&gt; SRR1812727     1  0.8703     0.5816 0.588 0.168 0.244
#&gt; SRR1812726     2  0.1781     0.4967 0.020 0.960 0.020
#&gt; SRR1812728     2  0.4121     0.4665 0.000 0.832 0.168
#&gt; SRR1812724     2  0.5948     0.0928 0.000 0.640 0.360
#&gt; SRR1812725     2  0.2772     0.4810 0.004 0.916 0.080
#&gt; SRR1812723     2  0.3502     0.4788 0.020 0.896 0.084
#&gt; SRR1812722     2  0.1129     0.4954 0.020 0.976 0.004
#&gt; SRR1812721     3  0.7268     0.4502 0.028 0.448 0.524
#&gt; SRR1812718     2  0.1453     0.4990 0.024 0.968 0.008
#&gt; SRR1812717     2  0.5733     0.1372 0.000 0.676 0.324
#&gt; SRR1812716     2  0.4521     0.4331 0.004 0.816 0.180
#&gt; SRR1812715     2  0.5650     0.1454 0.000 0.688 0.312
#&gt; SRR1812714     2  0.5708     0.3690 0.028 0.768 0.204
#&gt; SRR1812719     2  0.9642    -0.1043 0.208 0.416 0.376
#&gt; SRR1812713     2  0.6260     0.0728 0.000 0.552 0.448
#&gt; SRR1812712     2  0.5016     0.3244 0.000 0.760 0.240
#&gt; SRR1812711     2  0.3752     0.4712 0.020 0.884 0.096
#&gt; SRR1812710     2  0.5650     0.1454 0.000 0.688 0.312
#&gt; SRR1812709     2  0.5706     0.1407 0.000 0.680 0.320
#&gt; SRR1812708     1  0.8072     0.5947 0.652 0.184 0.164
#&gt; SRR1812707     2  0.5650     0.1454 0.000 0.688 0.312
#&gt; SRR1812705     2  0.1129     0.4954 0.020 0.976 0.004
#&gt; SRR1812706     2  0.4293     0.4471 0.004 0.832 0.164
#&gt; SRR1812704     2  0.4654     0.3411 0.000 0.792 0.208
#&gt; SRR1812703     2  0.2772     0.4860 0.004 0.916 0.080
#&gt; SRR1812702     2  0.5201     0.3418 0.004 0.760 0.236
#&gt; SRR1812741     3  0.6819     0.6149 0.028 0.328 0.644
#&gt; SRR1812740     3  0.8206     0.1838 0.072 0.448 0.480
#&gt; SRR1812739     2  0.5948     0.0928 0.000 0.640 0.360
#&gt; SRR1812738     2  0.6008     0.0610 0.000 0.628 0.372
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-2-a').click(function(){
  $('#tab-MAD-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-3'>
<p><a id='tab-MAD-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.1211    0.96291 0.960 0.000 0.000 0.040
#&gt; SRR1812753     1  0.1211    0.96291 0.960 0.000 0.000 0.040
#&gt; SRR1812751     1  0.0000    0.97552 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000    0.97552 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.4755    0.38706 0.000 0.200 0.760 0.040
#&gt; SRR1812749     1  0.0000    0.97552 1.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0779    0.53779 0.016 0.004 0.980 0.000
#&gt; SRR1812745     3  0.3877    0.57213 0.000 0.048 0.840 0.112
#&gt; SRR1812747     2  0.8217    0.41905 0.040 0.448 0.148 0.364
#&gt; SRR1812744     2  0.7900    0.17499 0.000 0.368 0.300 0.332
#&gt; SRR1812743     4  0.4817    0.62637 0.000 0.388 0.000 0.612
#&gt; SRR1812742     4  0.5511    0.53118 0.000 0.196 0.084 0.720
#&gt; SRR1812737     2  0.0188    0.37190 0.000 0.996 0.004 0.000
#&gt; SRR1812735     2  0.2647    0.39347 0.000 0.880 0.000 0.120
#&gt; SRR1812734     3  0.1585    0.55945 0.004 0.004 0.952 0.040
#&gt; SRR1812733     4  0.7910   -0.43247 0.000 0.320 0.316 0.364
#&gt; SRR1812736     3  0.3217    0.56629 0.000 0.012 0.860 0.128
#&gt; SRR1812732     2  0.5962    0.35413 0.000 0.692 0.128 0.180
#&gt; SRR1812730     3  0.5448    0.51367 0.000 0.056 0.700 0.244
#&gt; SRR1812731     2  0.6248    0.30112 0.000 0.660 0.128 0.212
#&gt; SRR1812729     2  0.8223    0.41602 0.040 0.444 0.148 0.368
#&gt; SRR1812727     3  0.2530    0.47971 0.112 0.000 0.888 0.000
#&gt; SRR1812726     2  0.8254    0.41025 0.040 0.440 0.152 0.368
#&gt; SRR1812728     3  0.7825    0.10790 0.000 0.304 0.412 0.284
#&gt; SRR1812724     2  0.6011    0.35491 0.000 0.688 0.132 0.180
#&gt; SRR1812725     3  0.7818    0.05485 0.000 0.332 0.404 0.264
#&gt; SRR1812723     2  0.8613    0.35669 0.040 0.432 0.252 0.276
#&gt; SRR1812722     2  0.7184    0.42430 0.000 0.492 0.144 0.364
#&gt; SRR1812721     4  0.4866    0.62081 0.000 0.404 0.000 0.596
#&gt; SRR1812718     2  0.8580    0.36404 0.040 0.440 0.244 0.276
#&gt; SRR1812717     2  0.3196    0.48440 0.000 0.856 0.136 0.008
#&gt; SRR1812716     3  0.7802    0.12160 0.000 0.304 0.420 0.276
#&gt; SRR1812715     2  0.0000    0.36654 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.8593    0.38298 0.040 0.436 0.244 0.280
#&gt; SRR1812719     3  0.0336    0.54121 0.008 0.000 0.992 0.000
#&gt; SRR1812713     2  0.7082    0.35454 0.036 0.648 0.140 0.176
#&gt; SRR1812712     2  0.6906    0.43118 0.000 0.580 0.156 0.264
#&gt; SRR1812711     2  0.8602    0.35813 0.040 0.436 0.260 0.264
#&gt; SRR1812710     2  0.0000    0.36654 0.000 1.000 0.000 0.000
#&gt; SRR1812709     2  0.3088    0.48322 0.000 0.864 0.128 0.008
#&gt; SRR1812708     3  0.8182    0.00188 0.208 0.340 0.432 0.020
#&gt; SRR1812707     2  0.0000    0.36654 0.000 1.000 0.000 0.000
#&gt; SRR1812705     2  0.7571    0.36685 0.000 0.484 0.244 0.272
#&gt; SRR1812706     3  0.7790    0.12602 0.000 0.304 0.424 0.272
#&gt; SRR1812704     2  0.7244    0.39371 0.000 0.488 0.152 0.360
#&gt; SRR1812703     2  0.8595    0.36995 0.040 0.432 0.236 0.292
#&gt; SRR1812702     3  0.7746    0.15989 0.000 0.288 0.440 0.272
#&gt; SRR1812741     4  0.5807    0.63045 0.000 0.344 0.044 0.612
#&gt; SRR1812740     3  0.4996    0.50585 0.000 0.056 0.752 0.192
#&gt; SRR1812739     2  0.6025    0.36285 0.000 0.688 0.140 0.172
#&gt; SRR1812738     2  0.5962    0.35413 0.000 0.692 0.128 0.180
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-3-a').click(function(){
  $('#tab-MAD-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-4'>
<p><a id='tab-MAD-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.1478      0.945 0.936 0.000 0.000 0.000 0.064
#&gt; SRR1812753     1  0.1638      0.943 0.932 0.000 0.004 0.000 0.064
#&gt; SRR1812751     1  0.0000      0.964 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.964 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     2  0.8338      0.359 0.000 0.348 0.184 0.172 0.296
#&gt; SRR1812749     1  0.0000      0.964 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     5  0.3392      0.832 0.004 0.084 0.000 0.064 0.848
#&gt; SRR1812745     2  0.6063      0.421 0.000 0.540 0.144 0.000 0.316
#&gt; SRR1812747     2  0.2648      0.545 0.000 0.848 0.000 0.152 0.000
#&gt; SRR1812744     2  0.7024      0.522 0.000 0.536 0.048 0.184 0.232
#&gt; SRR1812743     3  0.2690      0.721 0.000 0.000 0.844 0.156 0.000
#&gt; SRR1812742     3  0.5556      0.389 0.000 0.152 0.660 0.004 0.184
#&gt; SRR1812737     4  0.0000      0.679 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812735     4  0.0963      0.668 0.000 0.036 0.000 0.964 0.000
#&gt; SRR1812734     5  0.3234      0.831 0.000 0.084 0.000 0.064 0.852
#&gt; SRR1812733     2  0.6462      0.463 0.000 0.548 0.176 0.012 0.264
#&gt; SRR1812736     2  0.7894      0.420 0.000 0.400 0.144 0.124 0.332
#&gt; SRR1812732     4  0.5931      0.626 0.000 0.088 0.220 0.652 0.040
#&gt; SRR1812730     2  0.7104      0.474 0.000 0.496 0.144 0.052 0.308
#&gt; SRR1812731     4  0.6116      0.599 0.000 0.084 0.272 0.608 0.036
#&gt; SRR1812729     2  0.3398      0.422 0.000 0.780 0.000 0.216 0.004
#&gt; SRR1812727     5  0.2349      0.830 0.012 0.084 0.004 0.000 0.900
#&gt; SRR1812726     2  0.4787      0.494 0.000 0.712 0.000 0.208 0.080
#&gt; SRR1812728     2  0.7131      0.541 0.000 0.536 0.060 0.184 0.220
#&gt; SRR1812724     4  0.6079      0.616 0.000 0.084 0.252 0.624 0.040
#&gt; SRR1812725     2  0.6265      0.487 0.000 0.596 0.172 0.016 0.216
#&gt; SRR1812723     2  0.2818      0.552 0.000 0.856 0.000 0.132 0.012
#&gt; SRR1812722     2  0.4449     -0.109 0.000 0.512 0.000 0.484 0.004
#&gt; SRR1812721     4  0.4306     -0.227 0.000 0.000 0.492 0.508 0.000
#&gt; SRR1812718     2  0.2214      0.538 0.000 0.916 0.004 0.028 0.052
#&gt; SRR1812717     4  0.3928      0.684 0.000 0.152 0.008 0.800 0.040
#&gt; SRR1812716     2  0.6296      0.485 0.000 0.592 0.176 0.016 0.216
#&gt; SRR1812715     4  0.0000      0.679 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812714     2  0.5091      0.473 0.000 0.692 0.000 0.196 0.112
#&gt; SRR1812719     5  0.1792      0.830 0.000 0.084 0.000 0.000 0.916
#&gt; SRR1812713     4  0.5437      0.576 0.000 0.296 0.024 0.636 0.044
#&gt; SRR1812712     4  0.5379      0.375 0.000 0.460 0.004 0.492 0.044
#&gt; SRR1812711     2  0.4901      0.495 0.000 0.708 0.000 0.196 0.096
#&gt; SRR1812710     4  0.0000      0.679 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812709     4  0.3154      0.691 0.000 0.088 0.008 0.864 0.040
#&gt; SRR1812708     5  0.6823      0.535 0.228 0.132 0.000 0.064 0.576
#&gt; SRR1812707     4  0.0000      0.679 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812705     2  0.3596      0.541 0.000 0.776 0.000 0.212 0.012
#&gt; SRR1812706     2  0.7442      0.525 0.000 0.516 0.176 0.092 0.216
#&gt; SRR1812704     2  0.6866      0.539 0.000 0.564 0.048 0.196 0.192
#&gt; SRR1812703     2  0.1981      0.547 0.000 0.924 0.000 0.028 0.048
#&gt; SRR1812702     2  0.6571      0.464 0.000 0.540 0.176 0.016 0.268
#&gt; SRR1812741     3  0.3081      0.726 0.000 0.000 0.832 0.156 0.012
#&gt; SRR1812740     2  0.8193      0.412 0.000 0.384 0.156 0.172 0.288
#&gt; SRR1812739     4  0.6941      0.557 0.000 0.244 0.172 0.540 0.044
#&gt; SRR1812738     4  0.5695      0.647 0.000 0.088 0.188 0.684 0.040
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-4-a').click(function(){
  $('#tab-MAD-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-mclust-get-classes-5'>
<p><a id='tab-MAD-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0458      0.987 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0458      0.987 0.984 0.000 0.016 0.000 0.000 0.000
#&gt; SRR1812751     1  0.0000      0.990 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000      0.990 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     5  0.7455      0.301 0.000 0.220 0.104 0.032 0.472 0.172
#&gt; SRR1812749     1  0.0146      0.988 0.996 0.004 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.2631      0.829 0.000 0.004 0.856 0.012 0.128 0.000
#&gt; SRR1812745     5  0.4533      0.703 0.000 0.048 0.092 0.104 0.756 0.000
#&gt; SRR1812747     2  0.3835      0.706 0.000 0.684 0.000 0.300 0.016 0.000
#&gt; SRR1812744     2  0.7728      0.560 0.000 0.428 0.160 0.268 0.056 0.088
#&gt; SRR1812743     6  0.0260      0.720 0.000 0.000 0.000 0.008 0.000 0.992
#&gt; SRR1812742     6  0.3277      0.613 0.000 0.084 0.092 0.000 0.000 0.824
#&gt; SRR1812737     4  0.1495      0.758 0.000 0.020 0.020 0.948 0.004 0.008
#&gt; SRR1812735     4  0.3048      0.614 0.000 0.152 0.020 0.824 0.004 0.000
#&gt; SRR1812734     3  0.2631      0.829 0.000 0.004 0.856 0.012 0.128 0.000
#&gt; SRR1812733     5  0.5111      0.604 0.000 0.220 0.064 0.020 0.680 0.016
#&gt; SRR1812736     5  0.5144      0.453 0.000 0.220 0.120 0.012 0.648 0.000
#&gt; SRR1812732     4  0.5571      0.590 0.000 0.004 0.088 0.656 0.060 0.192
#&gt; SRR1812730     5  0.3821      0.690 0.000 0.068 0.092 0.032 0.808 0.000
#&gt; SRR1812731     4  0.3840      0.582 0.000 0.000 0.000 0.696 0.020 0.284
#&gt; SRR1812729     2  0.4065      0.702 0.000 0.672 0.000 0.300 0.028 0.000
#&gt; SRR1812727     3  0.1448      0.828 0.012 0.000 0.948 0.016 0.024 0.000
#&gt; SRR1812726     2  0.3050      0.708 0.000 0.764 0.000 0.236 0.000 0.000
#&gt; SRR1812728     2  0.7534      0.599 0.000 0.404 0.108 0.268 0.204 0.016
#&gt; SRR1812724     4  0.4276      0.700 0.000 0.008 0.092 0.788 0.068 0.044
#&gt; SRR1812725     5  0.4535      0.569 0.000 0.152 0.000 0.144 0.704 0.000
#&gt; SRR1812723     2  0.4771      0.710 0.000 0.664 0.000 0.220 0.116 0.000
#&gt; SRR1812722     2  0.4620      0.509 0.000 0.532 0.000 0.428 0.000 0.040
#&gt; SRR1812721     6  0.4322      0.118 0.000 0.000 0.000 0.452 0.020 0.528
#&gt; SRR1812718     2  0.4511      0.356 0.000 0.620 0.000 0.048 0.332 0.000
#&gt; SRR1812717     4  0.2203      0.760 0.000 0.016 0.004 0.896 0.084 0.000
#&gt; SRR1812716     5  0.3856      0.668 0.000 0.068 0.000 0.132 0.788 0.012
#&gt; SRR1812715     4  0.1321      0.760 0.000 0.024 0.020 0.952 0.004 0.000
#&gt; SRR1812714     2  0.4489      0.702 0.000 0.704 0.040 0.232 0.000 0.024
#&gt; SRR1812719     3  0.1168      0.827 0.000 0.000 0.956 0.016 0.028 0.000
#&gt; SRR1812713     4  0.5186      0.463 0.000 0.004 0.088 0.596 0.308 0.004
#&gt; SRR1812712     2  0.7300      0.146 0.000 0.360 0.088 0.236 0.312 0.004
#&gt; SRR1812711     2  0.4740      0.710 0.000 0.664 0.000 0.228 0.108 0.000
#&gt; SRR1812710     4  0.1592      0.756 0.000 0.032 0.020 0.940 0.000 0.008
#&gt; SRR1812709     4  0.2058      0.762 0.000 0.008 0.000 0.908 0.072 0.012
#&gt; SRR1812708     3  0.4386      0.685 0.092 0.164 0.736 0.004 0.000 0.004
#&gt; SRR1812707     4  0.1237      0.760 0.000 0.020 0.020 0.956 0.004 0.000
#&gt; SRR1812705     2  0.6302      0.678 0.000 0.516 0.000 0.304 0.116 0.064
#&gt; SRR1812706     5  0.4255      0.677 0.000 0.068 0.020 0.120 0.780 0.012
#&gt; SRR1812704     2  0.6383      0.562 0.000 0.480 0.108 0.352 0.056 0.004
#&gt; SRR1812703     2  0.4030      0.690 0.000 0.756 0.000 0.140 0.104 0.000
#&gt; SRR1812702     5  0.4776      0.699 0.000 0.072 0.052 0.112 0.752 0.012
#&gt; SRR1812741     6  0.0260      0.720 0.000 0.000 0.000 0.008 0.000 0.992
#&gt; SRR1812740     5  0.5770      0.393 0.000 0.328 0.104 0.028 0.540 0.000
#&gt; SRR1812739     4  0.5683      0.465 0.000 0.004 0.088 0.572 0.308 0.028
#&gt; SRR1812738     4  0.2974      0.744 0.000 0.004 0.044 0.872 0.052 0.028
</code></pre>

<script>
$('#tab-MAD-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-mclust-get-classes-5-a').click(function(){
  $('#tab-MAD-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-mclust-signature_compare](figure_cola/MAD-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-mclust-collect-classes](figure_cola/MAD-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### MAD:NMF






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["MAD", "NMF"]
# you can also extract it by
# res = res_list["MAD:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'MAD' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 3.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk MAD-NMF-collect-plots](figure_cola/MAD-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk MAD-NMF-select-partition-number](figure_cola/MAD-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.590           0.831       0.921         0.4790 0.514   0.514
#> 3 3 0.793           0.858       0.923         0.2825 0.800   0.636
#> 4 4 0.608           0.625       0.826         0.2007 0.751   0.438
#> 5 5 0.738           0.753       0.857         0.0788 0.801   0.391
#> 6 6 0.768           0.669       0.837         0.0365 0.920   0.648
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 3
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-classes'>
<ul>
<li><a href='#tab-MAD-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-MAD-NMF-get-classes-1'>
<p><a id='tab-MAD-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.163      0.875 0.976 0.024
#&gt; SRR1812753     1   0.000      0.878 1.000 0.000
#&gt; SRR1812751     2   0.722      0.741 0.200 0.800
#&gt; SRR1812750     2   0.722      0.741 0.200 0.800
#&gt; SRR1812748     1   0.295      0.881 0.948 0.052
#&gt; SRR1812749     2   0.722      0.741 0.200 0.800
#&gt; SRR1812746     1   0.000      0.878 1.000 0.000
#&gt; SRR1812745     1   0.000      0.878 1.000 0.000
#&gt; SRR1812747     2   0.000      0.924 0.000 1.000
#&gt; SRR1812744     1   0.494      0.864 0.892 0.108
#&gt; SRR1812743     1   0.999      0.222 0.516 0.484
#&gt; SRR1812742     1   0.295      0.865 0.948 0.052
#&gt; SRR1812737     2   0.000      0.924 0.000 1.000
#&gt; SRR1812735     2   0.000      0.924 0.000 1.000
#&gt; SRR1812734     1   0.000      0.878 1.000 0.000
#&gt; SRR1812733     1   0.722      0.801 0.800 0.200
#&gt; SRR1812736     1   0.000      0.878 1.000 0.000
#&gt; SRR1812732     2   0.997     -0.103 0.468 0.532
#&gt; SRR1812730     1   0.260      0.882 0.956 0.044
#&gt; SRR1812731     2   0.000      0.924 0.000 1.000
#&gt; SRR1812729     2   0.000      0.924 0.000 1.000
#&gt; SRR1812727     1   0.000      0.878 1.000 0.000
#&gt; SRR1812726     2   0.000      0.924 0.000 1.000
#&gt; SRR1812728     1   0.881      0.678 0.700 0.300
#&gt; SRR1812724     2   0.118      0.912 0.016 0.984
#&gt; SRR1812725     2   0.767      0.651 0.224 0.776
#&gt; SRR1812723     2   0.000      0.924 0.000 1.000
#&gt; SRR1812722     2   0.000      0.924 0.000 1.000
#&gt; SRR1812721     2   0.000      0.924 0.000 1.000
#&gt; SRR1812718     2   0.000      0.924 0.000 1.000
#&gt; SRR1812717     2   0.000      0.924 0.000 1.000
#&gt; SRR1812716     1   0.714      0.805 0.804 0.196
#&gt; SRR1812715     2   0.000      0.924 0.000 1.000
#&gt; SRR1812714     2   0.000      0.924 0.000 1.000
#&gt; SRR1812719     1   0.000      0.878 1.000 0.000
#&gt; SRR1812713     2   0.000      0.924 0.000 1.000
#&gt; SRR1812712     2   0.000      0.924 0.000 1.000
#&gt; SRR1812711     2   0.000      0.924 0.000 1.000
#&gt; SRR1812710     2   0.000      0.924 0.000 1.000
#&gt; SRR1812709     2   0.000      0.924 0.000 1.000
#&gt; SRR1812708     2   0.722      0.741 0.200 0.800
#&gt; SRR1812707     2   0.000      0.924 0.000 1.000
#&gt; SRR1812705     2   0.000      0.924 0.000 1.000
#&gt; SRR1812706     1   0.738      0.795 0.792 0.208
#&gt; SRR1812704     2   0.456      0.836 0.096 0.904
#&gt; SRR1812703     2   0.000      0.924 0.000 1.000
#&gt; SRR1812702     1   0.327      0.879 0.940 0.060
#&gt; SRR1812741     2   0.881      0.491 0.300 0.700
#&gt; SRR1812740     1   0.584      0.845 0.860 0.140
#&gt; SRR1812739     2   0.000      0.924 0.000 1.000
#&gt; SRR1812738     1   0.781      0.771 0.768 0.232
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-1-a').click(function(){
  $('#tab-MAD-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-2'>
<p><a id='tab-MAD-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0237      0.916 0.996 0.000 0.004
#&gt; SRR1812753     1  0.0237      0.916 0.996 0.000 0.004
#&gt; SRR1812751     1  0.2448      0.938 0.924 0.076 0.000
#&gt; SRR1812750     1  0.2625      0.933 0.916 0.084 0.000
#&gt; SRR1812748     3  0.2356      0.837 0.072 0.000 0.928
#&gt; SRR1812749     1  0.2448      0.938 0.924 0.076 0.000
#&gt; SRR1812746     3  0.2959      0.826 0.100 0.000 0.900
#&gt; SRR1812745     3  0.0592      0.843 0.012 0.000 0.988
#&gt; SRR1812747     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812744     3  0.1989      0.844 0.048 0.004 0.948
#&gt; SRR1812743     3  0.8334      0.205 0.080 0.440 0.480
#&gt; SRR1812742     3  0.4346      0.769 0.184 0.000 0.816
#&gt; SRR1812737     2  0.0237      0.954 0.004 0.996 0.000
#&gt; SRR1812735     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812734     3  0.2448      0.836 0.076 0.000 0.924
#&gt; SRR1812733     3  0.2096      0.827 0.004 0.052 0.944
#&gt; SRR1812736     3  0.2356      0.837 0.072 0.000 0.928
#&gt; SRR1812732     3  0.6082      0.598 0.012 0.296 0.692
#&gt; SRR1812730     3  0.0237      0.842 0.004 0.000 0.996
#&gt; SRR1812731     2  0.0475      0.954 0.004 0.992 0.004
#&gt; SRR1812729     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812727     1  0.1643      0.889 0.956 0.000 0.044
#&gt; SRR1812726     2  0.0237      0.954 0.004 0.996 0.000
#&gt; SRR1812728     3  0.1647      0.834 0.004 0.036 0.960
#&gt; SRR1812724     2  0.2772      0.906 0.004 0.916 0.080
#&gt; SRR1812725     2  0.6330      0.313 0.004 0.600 0.396
#&gt; SRR1812723     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812722     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812721     2  0.0475      0.954 0.004 0.992 0.004
#&gt; SRR1812718     2  0.0237      0.955 0.000 0.996 0.004
#&gt; SRR1812717     2  0.0829      0.951 0.004 0.984 0.012
#&gt; SRR1812716     3  0.0475      0.841 0.004 0.004 0.992
#&gt; SRR1812715     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812714     2  0.2796      0.877 0.092 0.908 0.000
#&gt; SRR1812719     3  0.4796      0.734 0.220 0.000 0.780
#&gt; SRR1812713     2  0.2682      0.909 0.004 0.920 0.076
#&gt; SRR1812712     2  0.2590      0.912 0.004 0.924 0.072
#&gt; SRR1812711     2  0.0592      0.949 0.012 0.988 0.000
#&gt; SRR1812710     2  0.0237      0.954 0.004 0.996 0.000
#&gt; SRR1812709     2  0.0475      0.954 0.004 0.992 0.004
#&gt; SRR1812708     1  0.2959      0.918 0.900 0.100 0.000
#&gt; SRR1812707     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812705     2  0.0000      0.956 0.000 1.000 0.000
#&gt; SRR1812706     3  0.3983      0.756 0.004 0.144 0.852
#&gt; SRR1812704     2  0.3272      0.883 0.004 0.892 0.104
#&gt; SRR1812703     2  0.1620      0.941 0.024 0.964 0.012
#&gt; SRR1812702     3  0.0237      0.842 0.000 0.004 0.996
#&gt; SRR1812741     3  0.8603      0.590 0.232 0.168 0.600
#&gt; SRR1812740     3  0.1643      0.843 0.044 0.000 0.956
#&gt; SRR1812739     2  0.1267      0.945 0.004 0.972 0.024
#&gt; SRR1812738     3  0.5158      0.668 0.004 0.232 0.764
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-2-a').click(function(){
  $('#tab-MAD-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-3'>
<p><a id='tab-MAD-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.1022     0.8559 0.968 0.000 0.032 0.000
#&gt; SRR1812753     1  0.2011     0.8192 0.920 0.000 0.080 0.000
#&gt; SRR1812751     1  0.0000     0.8652 1.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0000     0.8652 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0921     0.7283 0.000 0.000 0.972 0.028
#&gt; SRR1812749     1  0.0000     0.8652 1.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.6497     0.3943 0.100 0.000 0.596 0.304
#&gt; SRR1812745     3  0.5000     0.0342 0.000 0.000 0.500 0.500
#&gt; SRR1812747     2  0.4891     0.5782 0.012 0.680 0.000 0.308
#&gt; SRR1812744     3  0.1637     0.7215 0.000 0.000 0.940 0.060
#&gt; SRR1812743     3  0.4977     0.2818 0.000 0.460 0.540 0.000
#&gt; SRR1812742     3  0.1661     0.7002 0.004 0.052 0.944 0.000
#&gt; SRR1812737     2  0.0188     0.7988 0.000 0.996 0.000 0.004
#&gt; SRR1812735     2  0.3024     0.7435 0.000 0.852 0.000 0.148
#&gt; SRR1812734     3  0.1302     0.7302 0.000 0.000 0.956 0.044
#&gt; SRR1812733     4  0.4040     0.5029 0.000 0.000 0.248 0.752
#&gt; SRR1812736     3  0.1211     0.7303 0.000 0.000 0.960 0.040
#&gt; SRR1812732     3  0.4790     0.4348 0.000 0.380 0.620 0.000
#&gt; SRR1812730     4  0.2469     0.6908 0.000 0.000 0.108 0.892
#&gt; SRR1812731     2  0.0921     0.7887 0.000 0.972 0.028 0.000
#&gt; SRR1812729     2  0.5007     0.4987 0.008 0.636 0.000 0.356
#&gt; SRR1812727     1  0.0817     0.8596 0.976 0.000 0.024 0.000
#&gt; SRR1812726     2  0.5592     0.5521 0.044 0.656 0.000 0.300
#&gt; SRR1812728     4  0.1389     0.7300 0.000 0.000 0.048 0.952
#&gt; SRR1812724     2  0.0921     0.7887 0.000 0.972 0.028 0.000
#&gt; SRR1812725     4  0.0657     0.7347 0.000 0.012 0.004 0.984
#&gt; SRR1812723     4  0.6238     0.3589 0.084 0.296 0.000 0.620
#&gt; SRR1812722     2  0.4606     0.6319 0.012 0.724 0.000 0.264
#&gt; SRR1812721     2  0.0817     0.7914 0.000 0.976 0.024 0.000
#&gt; SRR1812718     4  0.3707     0.6701 0.028 0.132 0.000 0.840
#&gt; SRR1812717     2  0.0336     0.7991 0.000 0.992 0.000 0.008
#&gt; SRR1812716     4  0.1867     0.7189 0.000 0.000 0.072 0.928
#&gt; SRR1812715     2  0.0592     0.7949 0.000 0.984 0.016 0.000
#&gt; SRR1812714     1  0.6229     0.4883 0.664 0.204 0.000 0.132
#&gt; SRR1812719     1  0.7205     0.2125 0.504 0.000 0.152 0.344
#&gt; SRR1812713     2  0.3266     0.7081 0.000 0.832 0.000 0.168
#&gt; SRR1812712     4  0.4989     0.1066 0.000 0.472 0.000 0.528
#&gt; SRR1812711     4  0.7685     0.1730 0.256 0.288 0.000 0.456
#&gt; SRR1812710     2  0.1398     0.7969 0.004 0.956 0.000 0.040
#&gt; SRR1812709     2  0.1302     0.7962 0.000 0.956 0.000 0.044
#&gt; SRR1812708     1  0.0336     0.8634 0.992 0.000 0.000 0.008
#&gt; SRR1812707     2  0.1389     0.7953 0.000 0.952 0.000 0.048
#&gt; SRR1812705     2  0.5921     0.2284 0.036 0.516 0.000 0.448
#&gt; SRR1812706     4  0.0921     0.7345 0.000 0.000 0.028 0.972
#&gt; SRR1812704     4  0.5113     0.5904 0.000 0.252 0.036 0.712
#&gt; SRR1812703     4  0.2943     0.7119 0.076 0.032 0.000 0.892
#&gt; SRR1812702     4  0.2011     0.7139 0.000 0.000 0.080 0.920
#&gt; SRR1812741     3  0.4761     0.4542 0.000 0.372 0.628 0.000
#&gt; SRR1812740     3  0.1389     0.7302 0.000 0.000 0.952 0.048
#&gt; SRR1812739     2  0.0817     0.7914 0.000 0.976 0.024 0.000
#&gt; SRR1812738     2  0.5132    -0.0907 0.000 0.548 0.448 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-3-a').click(function(){
  $('#tab-MAD-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-4'>
<p><a id='tab-MAD-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0162      0.888 0.996 0.004 0.000 0.000 0.000
#&gt; SRR1812753     1  0.1074      0.883 0.968 0.016 0.012 0.000 0.004
#&gt; SRR1812751     1  0.0880      0.889 0.968 0.032 0.000 0.000 0.000
#&gt; SRR1812750     1  0.1965      0.851 0.904 0.096 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0898      0.798 0.000 0.000 0.972 0.008 0.020
#&gt; SRR1812749     1  0.0880      0.889 0.968 0.032 0.000 0.000 0.000
#&gt; SRR1812746     1  0.6944      0.302 0.532 0.040 0.252 0.000 0.176
#&gt; SRR1812745     5  0.4808      0.248 0.000 0.032 0.348 0.000 0.620
#&gt; SRR1812747     2  0.1830      0.886 0.000 0.924 0.000 0.068 0.008
#&gt; SRR1812744     3  0.8583      0.432 0.200 0.044 0.436 0.096 0.224
#&gt; SRR1812743     4  0.4980      0.238 0.000 0.020 0.396 0.576 0.008
#&gt; SRR1812742     3  0.1653      0.785 0.000 0.028 0.944 0.024 0.004
#&gt; SRR1812737     4  0.0794      0.818 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1812735     2  0.2377      0.852 0.000 0.872 0.000 0.128 0.000
#&gt; SRR1812734     3  0.5281      0.624 0.024 0.040 0.680 0.004 0.252
#&gt; SRR1812733     5  0.1851      0.791 0.004 0.024 0.024 0.008 0.940
#&gt; SRR1812736     3  0.0671      0.796 0.000 0.004 0.980 0.000 0.016
#&gt; SRR1812732     3  0.4134      0.563 0.000 0.004 0.704 0.284 0.008
#&gt; SRR1812730     5  0.2326      0.874 0.004 0.072 0.012 0.004 0.908
#&gt; SRR1812731     4  0.0162      0.817 0.000 0.004 0.000 0.996 0.000
#&gt; SRR1812729     2  0.4313      0.772 0.000 0.760 0.000 0.068 0.172
#&gt; SRR1812727     1  0.1173      0.883 0.964 0.020 0.004 0.000 0.012
#&gt; SRR1812726     2  0.2177      0.885 0.004 0.908 0.000 0.080 0.008
#&gt; SRR1812728     5  0.2284      0.878 0.004 0.096 0.000 0.004 0.896
#&gt; SRR1812724     4  0.0671      0.818 0.000 0.004 0.000 0.980 0.016
#&gt; SRR1812725     5  0.3300      0.783 0.000 0.204 0.000 0.004 0.792
#&gt; SRR1812723     2  0.3556      0.806 0.004 0.808 0.000 0.020 0.168
#&gt; SRR1812722     2  0.2046      0.890 0.000 0.916 0.000 0.068 0.016
#&gt; SRR1812721     4  0.0794      0.817 0.000 0.028 0.000 0.972 0.000
#&gt; SRR1812718     2  0.2069      0.878 0.000 0.912 0.000 0.012 0.076
#&gt; SRR1812717     4  0.1818      0.811 0.000 0.024 0.000 0.932 0.044
#&gt; SRR1812716     5  0.2286      0.872 0.000 0.108 0.000 0.004 0.888
#&gt; SRR1812715     4  0.3816      0.521 0.000 0.304 0.000 0.696 0.000
#&gt; SRR1812714     2  0.3734      0.748 0.168 0.796 0.000 0.036 0.000
#&gt; SRR1812719     1  0.2959      0.813 0.864 0.016 0.008 0.000 0.112
#&gt; SRR1812713     4  0.3013      0.740 0.000 0.008 0.000 0.832 0.160
#&gt; SRR1812712     4  0.4532      0.634 0.004 0.036 0.000 0.712 0.248
#&gt; SRR1812711     2  0.2086      0.883 0.028 0.928 0.000 0.016 0.028
#&gt; SRR1812710     4  0.4182      0.313 0.000 0.400 0.000 0.600 0.000
#&gt; SRR1812709     4  0.0693      0.820 0.000 0.012 0.000 0.980 0.008
#&gt; SRR1812708     1  0.1704      0.880 0.928 0.068 0.000 0.000 0.004
#&gt; SRR1812707     4  0.1205      0.815 0.000 0.040 0.000 0.956 0.004
#&gt; SRR1812705     2  0.2462      0.860 0.000 0.880 0.000 0.008 0.112
#&gt; SRR1812706     5  0.2068      0.879 0.000 0.092 0.000 0.004 0.904
#&gt; SRR1812704     4  0.5624      0.241 0.004 0.064 0.000 0.512 0.420
#&gt; SRR1812703     5  0.2678      0.865 0.016 0.100 0.000 0.004 0.880
#&gt; SRR1812702     5  0.2293      0.877 0.000 0.084 0.016 0.000 0.900
#&gt; SRR1812741     4  0.2312      0.784 0.008 0.012 0.056 0.916 0.008
#&gt; SRR1812740     3  0.1310      0.791 0.000 0.020 0.956 0.000 0.024
#&gt; SRR1812739     4  0.0798      0.816 0.000 0.008 0.016 0.976 0.000
#&gt; SRR1812738     4  0.1597      0.807 0.000 0.008 0.024 0.948 0.020
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-4-a').click(function(){
  $('#tab-MAD-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-MAD-NMF-get-classes-5'>
<p><a id='tab-MAD-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000      0.873 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0964      0.869 0.968 0.000 0.012 0.000 0.004 0.016
#&gt; SRR1812751     1  0.1148      0.874 0.960 0.020 0.016 0.000 0.000 0.004
#&gt; SRR1812750     1  0.2680      0.809 0.856 0.124 0.016 0.000 0.000 0.004
#&gt; SRR1812748     6  0.3564      0.638 0.000 0.000 0.264 0.000 0.012 0.724
#&gt; SRR1812749     1  0.1485      0.871 0.944 0.028 0.024 0.000 0.000 0.004
#&gt; SRR1812746     3  0.4712      0.490 0.292 0.000 0.648 0.000 0.016 0.044
#&gt; SRR1812745     3  0.4204      0.612 0.000 0.000 0.740 0.000 0.128 0.132
#&gt; SRR1812747     2  0.0508      0.782 0.000 0.984 0.000 0.000 0.012 0.004
#&gt; SRR1812744     3  0.1991      0.651 0.012 0.000 0.920 0.024 0.000 0.044
#&gt; SRR1812743     4  0.3995      0.110 0.000 0.000 0.004 0.516 0.000 0.480
#&gt; SRR1812742     6  0.0912      0.621 0.000 0.012 0.004 0.008 0.004 0.972
#&gt; SRR1812737     4  0.0777      0.857 0.000 0.024 0.004 0.972 0.000 0.000
#&gt; SRR1812735     2  0.1261      0.777 0.000 0.956 0.004 0.028 0.004 0.008
#&gt; SRR1812734     3  0.1663      0.653 0.000 0.000 0.912 0.000 0.000 0.088
#&gt; SRR1812733     3  0.3595      0.548 0.000 0.000 0.704 0.008 0.288 0.000
#&gt; SRR1812736     6  0.3314      0.638 0.000 0.000 0.256 0.000 0.004 0.740
#&gt; SRR1812732     6  0.6054      0.276 0.000 0.000 0.328 0.268 0.000 0.404
#&gt; SRR1812730     5  0.0692      0.780 0.000 0.000 0.020 0.000 0.976 0.004
#&gt; SRR1812731     4  0.0551      0.858 0.000 0.004 0.004 0.984 0.000 0.008
#&gt; SRR1812729     5  0.4455      0.277 0.000 0.388 0.008 0.020 0.584 0.000
#&gt; SRR1812727     1  0.1870      0.863 0.928 0.004 0.044 0.000 0.012 0.012
#&gt; SRR1812726     2  0.1605      0.777 0.000 0.936 0.044 0.004 0.016 0.000
#&gt; SRR1812728     5  0.1931      0.782 0.016 0.020 0.032 0.004 0.928 0.000
#&gt; SRR1812724     4  0.0964      0.860 0.000 0.000 0.004 0.968 0.016 0.012
#&gt; SRR1812725     5  0.2118      0.764 0.000 0.104 0.008 0.000 0.888 0.000
#&gt; SRR1812723     2  0.3950      0.172 0.000 0.564 0.004 0.000 0.432 0.000
#&gt; SRR1812722     2  0.1788      0.775 0.000 0.928 0.004 0.012 0.052 0.004
#&gt; SRR1812721     4  0.0405      0.861 0.000 0.004 0.000 0.988 0.008 0.000
#&gt; SRR1812718     2  0.1663      0.756 0.000 0.912 0.000 0.000 0.088 0.000
#&gt; SRR1812717     4  0.3053      0.747 0.000 0.004 0.012 0.812 0.172 0.000
#&gt; SRR1812716     5  0.0951      0.783 0.000 0.020 0.004 0.000 0.968 0.008
#&gt; SRR1812715     2  0.4227     -0.127 0.000 0.496 0.004 0.492 0.000 0.008
#&gt; SRR1812714     2  0.2564      0.737 0.044 0.892 0.052 0.004 0.004 0.004
#&gt; SRR1812719     1  0.3836      0.697 0.764 0.000 0.040 0.000 0.188 0.008
#&gt; SRR1812713     4  0.1970      0.824 0.000 0.000 0.008 0.900 0.092 0.000
#&gt; SRR1812712     4  0.3065      0.763 0.000 0.000 0.028 0.820 0.152 0.000
#&gt; SRR1812711     2  0.0405      0.781 0.000 0.988 0.004 0.000 0.008 0.000
#&gt; SRR1812710     4  0.3866      0.031 0.000 0.484 0.000 0.516 0.000 0.000
#&gt; SRR1812709     4  0.0508      0.860 0.000 0.000 0.004 0.984 0.012 0.000
#&gt; SRR1812708     1  0.3971      0.663 0.704 0.024 0.268 0.000 0.000 0.004
#&gt; SRR1812707     4  0.0603      0.860 0.000 0.016 0.000 0.980 0.004 0.000
#&gt; SRR1812705     2  0.3699      0.423 0.000 0.660 0.004 0.000 0.336 0.000
#&gt; SRR1812706     5  0.2573      0.755 0.000 0.024 0.112 0.000 0.864 0.000
#&gt; SRR1812704     5  0.2151      0.744 0.008 0.000 0.016 0.072 0.904 0.000
#&gt; SRR1812703     5  0.5638      0.314 0.028 0.076 0.400 0.000 0.496 0.000
#&gt; SRR1812702     5  0.3629      0.596 0.000 0.016 0.260 0.000 0.724 0.000
#&gt; SRR1812741     4  0.0820      0.856 0.016 0.000 0.000 0.972 0.000 0.012
#&gt; SRR1812740     6  0.3214      0.627 0.004 0.000 0.080 0.000 0.080 0.836
#&gt; SRR1812739     4  0.1524      0.831 0.000 0.000 0.060 0.932 0.000 0.008
#&gt; SRR1812738     4  0.0665      0.860 0.000 0.000 0.008 0.980 0.008 0.004
</code></pre>

<script>
$('#tab-MAD-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-MAD-NMF-get-classes-5-a').click(function(){
  $('#tab-MAD-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-MAD-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-MAD-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-MAD-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-MAD-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-MAD-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-MAD-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-MAD-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-MAD-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk MAD-NMF-signature_compare](figure_cola/MAD-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-MAD-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-MAD-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-MAD-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-MAD-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-MAD-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-MAD-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-MAD-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-MAD-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk MAD-NMF-collect-classes](figure_cola/MAD-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:hclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "hclust"]
# you can also extract it by
# res = res_list["ATC:hclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'hclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-hclust-collect-plots](figure_cola/ATC-hclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-hclust-select-partition-number](figure_cola/ATC-hclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.286           0.285       0.682         0.3708 0.576   0.576
#> 3 3 0.466           0.641       0.806         0.5473 0.784   0.626
#> 4 4 0.583           0.603       0.769         0.1377 0.960   0.890
#> 5 5 0.702           0.660       0.771         0.1119 0.934   0.806
#> 6 6 0.736           0.550       0.746         0.0688 0.846   0.515
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-classes'>
<ul>
<li><a href='#tab-ATC-hclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-hclust-get-classes-1'>
<p><a id='tab-ATC-hclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     2   0.978   -0.00421 0.412 0.588
#&gt; SRR1812753     2   0.978   -0.00421 0.412 0.588
#&gt; SRR1812751     2   0.000    0.27977 0.000 1.000
#&gt; SRR1812750     2   0.000    0.27977 0.000 1.000
#&gt; SRR1812748     1   0.000    0.49735 1.000 0.000
#&gt; SRR1812749     2   0.000    0.27977 0.000 1.000
#&gt; SRR1812746     1   0.000    0.49735 1.000 0.000
#&gt; SRR1812745     1   0.000    0.49735 1.000 0.000
#&gt; SRR1812747     2   0.961    0.37721 0.384 0.616
#&gt; SRR1812744     2   0.994    0.23941 0.456 0.544
#&gt; SRR1812743     2   0.978   -0.00421 0.412 0.588
#&gt; SRR1812742     2   0.978   -0.00421 0.412 0.588
#&gt; SRR1812737     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812735     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812734     1   0.000    0.49735 1.000 0.000
#&gt; SRR1812733     2   0.991    0.27632 0.444 0.556
#&gt; SRR1812736     1   0.000    0.49735 1.000 0.000
#&gt; SRR1812732     2   0.994    0.23941 0.456 0.544
#&gt; SRR1812730     1   0.904    0.39032 0.680 0.320
#&gt; SRR1812731     2   0.574    0.23471 0.136 0.864
#&gt; SRR1812729     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812727     1   0.904    0.39032 0.680 0.320
#&gt; SRR1812726     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812728     1   1.000   -0.08994 0.508 0.492
#&gt; SRR1812724     1   1.000   -0.08994 0.508 0.492
#&gt; SRR1812725     2   0.991    0.27632 0.444 0.556
#&gt; SRR1812723     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812722     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812721     2   0.978   -0.00421 0.412 0.588
#&gt; SRR1812718     2   0.961    0.37721 0.384 0.616
#&gt; SRR1812717     2   0.943    0.41541 0.360 0.640
#&gt; SRR1812716     1   0.983    0.14301 0.576 0.424
#&gt; SRR1812715     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812714     2   0.932    0.41645 0.348 0.652
#&gt; SRR1812719     1   0.904    0.39032 0.680 0.320
#&gt; SRR1812713     2   0.991    0.27632 0.444 0.556
#&gt; SRR1812712     2   0.991    0.27632 0.444 0.556
#&gt; SRR1812711     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812710     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812709     2   0.574    0.23471 0.136 0.864
#&gt; SRR1812708     2   0.000    0.27977 0.000 1.000
#&gt; SRR1812707     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812705     2   0.939    0.42095 0.356 0.644
#&gt; SRR1812706     1   0.904    0.39032 0.680 0.320
#&gt; SRR1812704     1   1.000   -0.08994 0.508 0.492
#&gt; SRR1812703     2   0.991    0.27632 0.444 0.556
#&gt; SRR1812702     2   0.991    0.27632 0.444 0.556
#&gt; SRR1812741     2   0.978   -0.00421 0.412 0.588
#&gt; SRR1812740     1   0.000    0.49735 1.000 0.000
#&gt; SRR1812739     2   0.985    0.30728 0.428 0.572
#&gt; SRR1812738     1   1.000   -0.08994 0.508 0.492
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-1-a').click(function(){
  $('#tab-ATC-hclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-2'>
<p><a id='tab-ATC-hclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.3192    0.65604 0.888 0.000 0.112
#&gt; SRR1812753     1  0.3192    0.65604 0.888 0.000 0.112
#&gt; SRR1812751     1  0.6244    0.45560 0.560 0.440 0.000
#&gt; SRR1812750     1  0.6244    0.45560 0.560 0.440 0.000
#&gt; SRR1812748     3  0.1529    0.54147 0.040 0.000 0.960
#&gt; SRR1812749     1  0.6244    0.45560 0.560 0.440 0.000
#&gt; SRR1812746     3  0.1529    0.54147 0.040 0.000 0.960
#&gt; SRR1812745     3  0.1529    0.54147 0.040 0.000 0.960
#&gt; SRR1812747     2  0.1163    0.83894 0.000 0.972 0.028
#&gt; SRR1812744     2  0.6728    0.70955 0.124 0.748 0.128
#&gt; SRR1812743     1  0.3192    0.65604 0.888 0.000 0.112
#&gt; SRR1812742     1  0.3192    0.65604 0.888 0.000 0.112
#&gt; SRR1812737     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812735     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812734     3  0.1529    0.54147 0.040 0.000 0.960
#&gt; SRR1812733     2  0.6526    0.72500 0.112 0.760 0.128
#&gt; SRR1812736     3  0.1529    0.54147 0.040 0.000 0.960
#&gt; SRR1812732     2  0.6728    0.70955 0.124 0.748 0.128
#&gt; SRR1812730     3  0.6200    0.63096 0.012 0.312 0.676
#&gt; SRR1812731     1  0.8665   -0.00632 0.508 0.384 0.108
#&gt; SRR1812729     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812727     3  0.6200    0.63096 0.012 0.312 0.676
#&gt; SRR1812726     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812728     3  0.8848    0.46099 0.124 0.372 0.504
#&gt; SRR1812724     3  0.8848    0.46099 0.124 0.372 0.504
#&gt; SRR1812725     2  0.6526    0.72500 0.112 0.760 0.128
#&gt; SRR1812723     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812722     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812721     1  0.3192    0.65604 0.888 0.000 0.112
#&gt; SRR1812718     2  0.1163    0.83894 0.000 0.972 0.028
#&gt; SRR1812717     2  0.0237    0.84727 0.000 0.996 0.004
#&gt; SRR1812716     3  0.7867    0.54859 0.068 0.348 0.584
#&gt; SRR1812715     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0424    0.84251 0.008 0.992 0.000
#&gt; SRR1812719     3  0.6200    0.63096 0.012 0.312 0.676
#&gt; SRR1812713     2  0.6526    0.72500 0.112 0.760 0.128
#&gt; SRR1812712     2  0.6526    0.72500 0.112 0.760 0.128
#&gt; SRR1812711     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812710     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812709     1  0.8665   -0.00632 0.508 0.384 0.108
#&gt; SRR1812708     2  0.6941    0.02567 0.464 0.520 0.016
#&gt; SRR1812707     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812705     2  0.0000    0.84809 0.000 1.000 0.000
#&gt; SRR1812706     3  0.6200    0.63096 0.012 0.312 0.676
#&gt; SRR1812704     3  0.8848    0.46099 0.124 0.372 0.504
#&gt; SRR1812703     2  0.5874    0.74946 0.088 0.796 0.116
#&gt; SRR1812702     2  0.6526    0.72500 0.112 0.760 0.128
#&gt; SRR1812741     1  0.3192    0.65604 0.888 0.000 0.112
#&gt; SRR1812740     3  0.1529    0.54147 0.040 0.000 0.960
#&gt; SRR1812739     2  0.6255    0.73965 0.112 0.776 0.112
#&gt; SRR1812738     3  0.8848    0.46099 0.124 0.372 0.504
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-2-a').click(function(){
  $('#tab-ATC-hclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-3'>
<p><a id='tab-ATC-hclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     4  0.0000     0.7518 0.000 0.000 0.000 1.000
#&gt; SRR1812753     4  0.0000     0.7518 0.000 0.000 0.000 1.000
#&gt; SRR1812751     1  0.4925     0.8149 0.572 0.428 0.000 0.000
#&gt; SRR1812750     1  0.4925     0.8149 0.572 0.428 0.000 0.000
#&gt; SRR1812748     3  0.0000     0.5441 0.000 0.000 1.000 0.000
#&gt; SRR1812749     1  0.4925     0.8149 0.572 0.428 0.000 0.000
#&gt; SRR1812746     3  0.0000     0.5441 0.000 0.000 1.000 0.000
#&gt; SRR1812745     3  0.0000     0.5441 0.000 0.000 1.000 0.000
#&gt; SRR1812747     2  0.1867     0.6515 0.072 0.928 0.000 0.000
#&gt; SRR1812744     2  0.5320     0.5405 0.416 0.572 0.000 0.012
#&gt; SRR1812743     4  0.0000     0.7518 0.000 0.000 0.000 1.000
#&gt; SRR1812742     4  0.0000     0.7518 0.000 0.000 0.000 1.000
#&gt; SRR1812737     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812735     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812734     3  0.0000     0.5441 0.000 0.000 1.000 0.000
#&gt; SRR1812733     2  0.4925     0.5479 0.428 0.572 0.000 0.000
#&gt; SRR1812736     3  0.0000     0.5441 0.000 0.000 1.000 0.000
#&gt; SRR1812732     2  0.5320     0.5405 0.416 0.572 0.000 0.012
#&gt; SRR1812730     3  0.7405     0.6495 0.236 0.184 0.568 0.012
#&gt; SRR1812731     4  0.7666     0.0367 0.392 0.212 0.000 0.396
#&gt; SRR1812729     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812727     3  0.7405     0.6495 0.236 0.184 0.568 0.012
#&gt; SRR1812726     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812728     3  0.7999     0.5430 0.392 0.200 0.396 0.012
#&gt; SRR1812724     3  0.7999     0.5430 0.392 0.200 0.396 0.012
#&gt; SRR1812725     2  0.4925     0.5479 0.428 0.572 0.000 0.000
#&gt; SRR1812723     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812722     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812721     4  0.0000     0.7518 0.000 0.000 0.000 1.000
#&gt; SRR1812718     2  0.1867     0.6515 0.072 0.928 0.000 0.000
#&gt; SRR1812717     2  0.0592     0.6633 0.016 0.984 0.000 0.000
#&gt; SRR1812716     3  0.7442     0.6055 0.340 0.184 0.476 0.000
#&gt; SRR1812715     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.0707     0.6334 0.020 0.980 0.000 0.000
#&gt; SRR1812719     3  0.7405     0.6495 0.236 0.184 0.568 0.012
#&gt; SRR1812713     2  0.4925     0.5479 0.428 0.572 0.000 0.000
#&gt; SRR1812712     2  0.4925     0.5479 0.428 0.572 0.000 0.000
#&gt; SRR1812711     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812710     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812709     4  0.7666     0.0367 0.392 0.212 0.000 0.396
#&gt; SRR1812708     1  0.4855     0.2183 0.600 0.400 0.000 0.000
#&gt; SRR1812707     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812705     2  0.0000     0.6639 0.000 1.000 0.000 0.000
#&gt; SRR1812706     3  0.7405     0.6495 0.236 0.184 0.568 0.012
#&gt; SRR1812704     3  0.7999     0.5430 0.392 0.200 0.396 0.012
#&gt; SRR1812703     2  0.4830     0.5633 0.392 0.608 0.000 0.000
#&gt; SRR1812702     2  0.4925     0.5479 0.428 0.572 0.000 0.000
#&gt; SRR1812741     4  0.0000     0.7518 0.000 0.000 0.000 1.000
#&gt; SRR1812740     3  0.0000     0.5441 0.000 0.000 1.000 0.000
#&gt; SRR1812739     2  0.4855     0.5603 0.400 0.600 0.000 0.000
#&gt; SRR1812738     3  0.7999     0.5430 0.392 0.200 0.396 0.012
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-3-a').click(function(){
  $('#tab-ATC-hclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-4'>
<p><a id='tab-ATC-hclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     4  0.0000     0.7345 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812753     4  0.0000     0.7345 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812751     1  0.0404     0.8378 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1812750     1  0.0404     0.8378 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1812748     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812749     1  0.0404     0.8378 0.988 0.000 0.000 0.000 0.012
#&gt; SRR1812746     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812745     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812747     2  0.3966     0.6395 0.336 0.664 0.000 0.000 0.000
#&gt; SRR1812744     2  0.4219     0.0282 0.000 0.584 0.000 0.000 0.416
#&gt; SRR1812743     4  0.4015     0.8664 0.000 0.000 0.000 0.652 0.348
#&gt; SRR1812742     4  0.4015     0.8664 0.000 0.000 0.000 0.652 0.348
#&gt; SRR1812737     2  0.4235     0.6528 0.424 0.576 0.000 0.000 0.000
#&gt; SRR1812735     2  0.4235     0.6528 0.424 0.576 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812733     2  0.0963     0.4909 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812736     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812732     2  0.4219     0.0282 0.000 0.584 0.000 0.000 0.416
#&gt; SRR1812730     5  0.4249     0.6321 0.000 0.000 0.432 0.000 0.568
#&gt; SRR1812731     5  0.3109     0.2829 0.000 0.200 0.000 0.000 0.800
#&gt; SRR1812729     2  0.4201     0.6555 0.408 0.592 0.000 0.000 0.000
#&gt; SRR1812727     5  0.4249     0.6321 0.000 0.000 0.432 0.000 0.568
#&gt; SRR1812726     2  0.4235     0.6528 0.424 0.576 0.000 0.000 0.000
#&gt; SRR1812728     5  0.6189     0.7027 0.000 0.140 0.384 0.000 0.476
#&gt; SRR1812724     5  0.6189     0.7027 0.000 0.140 0.384 0.000 0.476
#&gt; SRR1812725     2  0.0963     0.4909 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812723     2  0.4201     0.6555 0.408 0.592 0.000 0.000 0.000
#&gt; SRR1812722     2  0.4201     0.6555 0.408 0.592 0.000 0.000 0.000
#&gt; SRR1812721     4  0.4138     0.8594 0.000 0.000 0.000 0.616 0.384
#&gt; SRR1812718     2  0.4066     0.6359 0.324 0.672 0.000 0.000 0.004
#&gt; SRR1812717     2  0.4150     0.6553 0.388 0.612 0.000 0.000 0.000
#&gt; SRR1812716     5  0.5790     0.6745 0.000 0.092 0.408 0.000 0.500
#&gt; SRR1812715     2  0.4235     0.6528 0.424 0.576 0.000 0.000 0.000
#&gt; SRR1812714     2  0.4397     0.6285 0.432 0.564 0.000 0.000 0.004
#&gt; SRR1812719     5  0.4249     0.6321 0.000 0.000 0.432 0.000 0.568
#&gt; SRR1812713     2  0.0963     0.4909 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812712     2  0.0963     0.4909 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812711     2  0.4210     0.6551 0.412 0.588 0.000 0.000 0.000
#&gt; SRR1812710     2  0.4235     0.6528 0.424 0.576 0.000 0.000 0.000
#&gt; SRR1812709     5  0.3109     0.2829 0.000 0.200 0.000 0.000 0.800
#&gt; SRR1812708     1  0.5010     0.3562 0.572 0.392 0.000 0.000 0.036
#&gt; SRR1812707     2  0.4235     0.6528 0.424 0.576 0.000 0.000 0.000
#&gt; SRR1812705     2  0.4201     0.6555 0.408 0.592 0.000 0.000 0.000
#&gt; SRR1812706     5  0.4249     0.6321 0.000 0.000 0.432 0.000 0.568
#&gt; SRR1812704     5  0.6189     0.7027 0.000 0.140 0.384 0.000 0.476
#&gt; SRR1812703     2  0.0693     0.5038 0.012 0.980 0.000 0.000 0.008
#&gt; SRR1812702     2  0.0963     0.4909 0.000 0.964 0.000 0.000 0.036
#&gt; SRR1812741     4  0.4138     0.8594 0.000 0.000 0.000 0.616 0.384
#&gt; SRR1812740     3  0.0000     1.0000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812739     2  0.1750     0.5023 0.028 0.936 0.000 0.000 0.036
#&gt; SRR1812738     5  0.6189     0.7027 0.000 0.140 0.384 0.000 0.476
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-4-a').click(function(){
  $('#tab-ATC-hclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-hclust-get-classes-5'>
<p><a id='tab-ATC-hclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2 p3    p4    p5    p6
#&gt; SRR1812752     6  0.0000     0.6940 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1812753     6  0.0000     0.6940 0.000 0.000  0 0.000 0.000 1.000
#&gt; SRR1812751     1  0.0865     0.8056 0.964 0.036  0 0.000 0.000 0.000
#&gt; SRR1812750     1  0.0865     0.8056 0.964 0.036  0 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1812749     1  0.0865     0.8056 0.964 0.036  0 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1812745     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1812747     2  0.1700     0.8303 0.024 0.928  0 0.000 0.048 0.000
#&gt; SRR1812744     4  0.4252     0.1269 0.024 0.372  0 0.604 0.000 0.000
#&gt; SRR1812743     6  0.3607     0.8490 0.000 0.000  0 0.348 0.000 0.652
#&gt; SRR1812742     6  0.3607     0.8490 0.000 0.000  0 0.348 0.000 0.652
#&gt; SRR1812737     2  0.0458     0.8736 0.016 0.984  0 0.000 0.000 0.000
#&gt; SRR1812735     2  0.0458     0.8736 0.016 0.984  0 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1812733     5  0.6472     0.1130 0.024 0.360  0 0.224 0.392 0.000
#&gt; SRR1812736     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1812732     4  0.4252     0.1269 0.024 0.372  0 0.604 0.000 0.000
#&gt; SRR1812730     5  0.4037    -0.2888 0.012 0.000  0 0.380 0.608 0.000
#&gt; SRR1812731     4  0.0363     0.3850 0.000 0.012  0 0.988 0.000 0.000
#&gt; SRR1812729     2  0.0000     0.8730 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1812727     5  0.4037    -0.2888 0.012 0.000  0 0.380 0.608 0.000
#&gt; SRR1812726     2  0.0547     0.8722 0.020 0.980  0 0.000 0.000 0.000
#&gt; SRR1812728     4  0.3817     0.4715 0.000 0.000  0 0.568 0.432 0.000
#&gt; SRR1812724     4  0.3817     0.4715 0.000 0.000  0 0.568 0.432 0.000
#&gt; SRR1812725     5  0.6472     0.1130 0.024 0.360  0 0.224 0.392 0.000
#&gt; SRR1812723     2  0.0000     0.8730 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1812722     2  0.0000     0.8730 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1812721     6  0.3717     0.8410 0.000 0.000  0 0.384 0.000 0.616
#&gt; SRR1812718     2  0.3502     0.7095 0.024 0.800  0 0.016 0.160 0.000
#&gt; SRR1812717     2  0.2377     0.7789 0.004 0.868  0 0.004 0.124 0.000
#&gt; SRR1812716     5  0.3851    -0.4412 0.000 0.000  0 0.460 0.540 0.000
#&gt; SRR1812715     2  0.0458     0.8736 0.016 0.984  0 0.000 0.000 0.000
#&gt; SRR1812714     2  0.1951     0.8267 0.076 0.908  0 0.016 0.000 0.000
#&gt; SRR1812719     5  0.4037    -0.2888 0.012 0.000  0 0.380 0.608 0.000
#&gt; SRR1812713     5  0.6472     0.1130 0.024 0.360  0 0.224 0.392 0.000
#&gt; SRR1812712     5  0.6472     0.1130 0.024 0.360  0 0.224 0.392 0.000
#&gt; SRR1812711     2  0.0146     0.8734 0.004 0.996  0 0.000 0.000 0.000
#&gt; SRR1812710     2  0.0458     0.8736 0.016 0.984  0 0.000 0.000 0.000
#&gt; SRR1812709     4  0.0363     0.3850 0.000 0.012  0 0.988 0.000 0.000
#&gt; SRR1812708     1  0.7019     0.3831 0.480 0.208  0 0.168 0.144 0.000
#&gt; SRR1812707     2  0.0458     0.8736 0.016 0.984  0 0.000 0.000 0.000
#&gt; SRR1812705     2  0.0000     0.8730 0.000 1.000  0 0.000 0.000 0.000
#&gt; SRR1812706     5  0.4037    -0.2888 0.012 0.000  0 0.380 0.608 0.000
#&gt; SRR1812704     4  0.3817     0.4715 0.000 0.000  0 0.568 0.432 0.000
#&gt; SRR1812703     2  0.6351    -0.0627 0.024 0.436  0 0.196 0.344 0.000
#&gt; SRR1812702     5  0.6472     0.1130 0.024 0.360  0 0.224 0.392 0.000
#&gt; SRR1812741     6  0.3717     0.8410 0.000 0.000  0 0.384 0.000 0.616
#&gt; SRR1812740     3  0.0000     1.0000 0.000 0.000  1 0.000 0.000 0.000
#&gt; SRR1812739     2  0.6435    -0.0854 0.024 0.428  0 0.224 0.324 0.000
#&gt; SRR1812738     4  0.3817     0.4715 0.000 0.000  0 0.568 0.432 0.000
</code></pre>

<script>
$('#tab-ATC-hclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-hclust-get-classes-5-a').click(function(){
  $('#tab-ATC-hclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-hclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-hclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-hclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-hclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-hclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-hclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-hclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-hclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-hclust-signature_compare](figure_cola/ATC-hclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-hclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-hclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-hclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-hclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-hclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-hclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-hclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-hclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-hclust-collect-classes](figure_cola/ATC-hclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:kmeans






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "kmeans"]
# you can also extract it by
# res = res_list["ATC:kmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'kmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-kmeans-collect-plots](figure_cola/ATC-kmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-kmeans-select-partition-number](figure_cola/ATC-kmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.769           0.885       0.929         0.4807 0.500   0.500
#> 3 3 0.526           0.624       0.818         0.2922 0.818   0.658
#> 4 4 0.647           0.838       0.874         0.1330 0.784   0.517
#> 5 5 0.760           0.707       0.773         0.0832 0.993   0.976
#> 6 6 0.787           0.827       0.863         0.0592 0.852   0.524
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-kmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-kmeans-get-classes-1'>
<p><a id='tab-ATC-kmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.373      0.867 0.928 0.072
#&gt; SRR1812753     1   0.373      0.867 0.928 0.072
#&gt; SRR1812751     2   0.443      0.861 0.092 0.908
#&gt; SRR1812750     2   0.443      0.861 0.092 0.908
#&gt; SRR1812748     1   0.443      0.917 0.908 0.092
#&gt; SRR1812749     2   0.000      0.948 0.000 1.000
#&gt; SRR1812746     1   0.443      0.917 0.908 0.092
#&gt; SRR1812745     1   0.443      0.917 0.908 0.092
#&gt; SRR1812747     2   0.000      0.948 0.000 1.000
#&gt; SRR1812744     1   0.662      0.851 0.828 0.172
#&gt; SRR1812743     1   0.373      0.867 0.928 0.072
#&gt; SRR1812742     1   0.373      0.867 0.928 0.072
#&gt; SRR1812737     2   0.000      0.948 0.000 1.000
#&gt; SRR1812735     2   0.000      0.948 0.000 1.000
#&gt; SRR1812734     1   0.443      0.917 0.908 0.092
#&gt; SRR1812733     2   0.373      0.903 0.072 0.928
#&gt; SRR1812736     1   0.443      0.917 0.908 0.092
#&gt; SRR1812732     1   0.958      0.492 0.620 0.380
#&gt; SRR1812730     1   0.443      0.917 0.908 0.092
#&gt; SRR1812731     2   0.802      0.601 0.244 0.756
#&gt; SRR1812729     2   0.000      0.948 0.000 1.000
#&gt; SRR1812727     1   0.000      0.873 1.000 0.000
#&gt; SRR1812726     2   0.000      0.948 0.000 1.000
#&gt; SRR1812728     1   0.456      0.916 0.904 0.096
#&gt; SRR1812724     1   0.456      0.916 0.904 0.096
#&gt; SRR1812725     2   0.358      0.906 0.068 0.932
#&gt; SRR1812723     2   0.000      0.948 0.000 1.000
#&gt; SRR1812722     2   0.000      0.948 0.000 1.000
#&gt; SRR1812721     1   0.373      0.867 0.928 0.072
#&gt; SRR1812718     2   0.000      0.948 0.000 1.000
#&gt; SRR1812717     2   0.000      0.948 0.000 1.000
#&gt; SRR1812716     2   0.963      0.299 0.388 0.612
#&gt; SRR1812715     2   0.000      0.948 0.000 1.000
#&gt; SRR1812714     2   0.000      0.948 0.000 1.000
#&gt; SRR1812719     1   0.443      0.917 0.908 0.092
#&gt; SRR1812713     2   0.358      0.906 0.068 0.932
#&gt; SRR1812712     2   0.358      0.906 0.068 0.932
#&gt; SRR1812711     2   0.000      0.948 0.000 1.000
#&gt; SRR1812710     2   0.000      0.948 0.000 1.000
#&gt; SRR1812709     2   0.000      0.948 0.000 1.000
#&gt; SRR1812708     2   0.000      0.948 0.000 1.000
#&gt; SRR1812707     2   0.000      0.948 0.000 1.000
#&gt; SRR1812705     2   0.000      0.948 0.000 1.000
#&gt; SRR1812706     1   0.443      0.917 0.908 0.092
#&gt; SRR1812704     1   0.876      0.672 0.704 0.296
#&gt; SRR1812703     2   0.118      0.939 0.016 0.984
#&gt; SRR1812702     2   0.388      0.901 0.076 0.924
#&gt; SRR1812741     1   0.373      0.867 0.928 0.072
#&gt; SRR1812740     1   0.443      0.917 0.908 0.092
#&gt; SRR1812739     2   0.327      0.912 0.060 0.940
#&gt; SRR1812738     1   0.456      0.916 0.904 0.096
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-2'>
<p><a id='tab-ATC-kmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.1170     0.8465 0.976 0.008 0.016
#&gt; SRR1812753     1  0.1170     0.8465 0.976 0.008 0.016
#&gt; SRR1812751     2  0.3454     0.7732 0.104 0.888 0.008
#&gt; SRR1812750     2  0.3454     0.7732 0.104 0.888 0.008
#&gt; SRR1812748     3  0.6045     0.0486 0.380 0.000 0.620
#&gt; SRR1812749     2  0.2384     0.8129 0.056 0.936 0.008
#&gt; SRR1812746     3  0.2356     0.5560 0.072 0.000 0.928
#&gt; SRR1812745     3  0.2356     0.5560 0.072 0.000 0.928
#&gt; SRR1812747     2  0.0424     0.8504 0.000 0.992 0.008
#&gt; SRR1812744     3  0.6979     0.6077 0.140 0.128 0.732
#&gt; SRR1812743     1  0.2680     0.8756 0.924 0.008 0.068
#&gt; SRR1812742     1  0.2680     0.8756 0.924 0.008 0.068
#&gt; SRR1812737     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812735     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812734     3  0.5988     0.0764 0.368 0.000 0.632
#&gt; SRR1812733     3  0.5397     0.5164 0.000 0.280 0.720
#&gt; SRR1812736     3  0.6045     0.0486 0.380 0.000 0.620
#&gt; SRR1812732     3  0.7278     0.6014 0.136 0.152 0.712
#&gt; SRR1812730     3  0.2066     0.5622 0.060 0.000 0.940
#&gt; SRR1812731     2  0.9700    -0.0640 0.224 0.428 0.348
#&gt; SRR1812729     2  0.0424     0.8504 0.000 0.992 0.008
#&gt; SRR1812727     1  0.4291     0.7429 0.820 0.000 0.180
#&gt; SRR1812726     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812728     3  0.7003     0.5478 0.248 0.060 0.692
#&gt; SRR1812724     3  0.7372     0.5741 0.220 0.092 0.688
#&gt; SRR1812725     2  0.6215     0.3145 0.000 0.572 0.428
#&gt; SRR1812723     2  0.0424     0.8504 0.000 0.992 0.008
#&gt; SRR1812722     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812721     1  0.2680     0.8756 0.924 0.008 0.068
#&gt; SRR1812718     2  0.0424     0.8504 0.000 0.992 0.008
#&gt; SRR1812717     2  0.0424     0.8504 0.000 0.992 0.008
#&gt; SRR1812716     3  0.6232     0.5831 0.040 0.220 0.740
#&gt; SRR1812715     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812719     1  0.6235     0.1516 0.564 0.000 0.436
#&gt; SRR1812713     2  0.6215     0.3145 0.000 0.572 0.428
#&gt; SRR1812712     2  0.6215     0.3145 0.000 0.572 0.428
#&gt; SRR1812711     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812710     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812709     2  0.9252     0.0779 0.160 0.468 0.372
#&gt; SRR1812708     2  0.0424     0.8504 0.000 0.992 0.008
#&gt; SRR1812707     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812705     2  0.0000     0.8516 0.000 1.000 0.000
#&gt; SRR1812706     3  0.4555     0.5448 0.200 0.000 0.800
#&gt; SRR1812704     3  0.7635     0.5751 0.212 0.112 0.676
#&gt; SRR1812703     2  0.2796     0.7937 0.000 0.908 0.092
#&gt; SRR1812702     3  0.5291     0.5352 0.000 0.268 0.732
#&gt; SRR1812741     1  0.2680     0.8756 0.924 0.008 0.068
#&gt; SRR1812740     3  0.6045     0.0486 0.380 0.000 0.620
#&gt; SRR1812739     2  0.6008     0.4313 0.000 0.628 0.372
#&gt; SRR1812738     3  0.7372     0.5741 0.220 0.092 0.688
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-3'>
<p><a id='tab-ATC-kmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.2973      0.854 0.884 0.000 0.096 0.020
#&gt; SRR1812753     1  0.2973      0.854 0.884 0.000 0.096 0.020
#&gt; SRR1812751     2  0.6320      0.711 0.080 0.700 0.188 0.032
#&gt; SRR1812750     2  0.6320      0.711 0.080 0.700 0.188 0.032
#&gt; SRR1812748     3  0.4656      0.862 0.160 0.000 0.784 0.056
#&gt; SRR1812749     2  0.6320      0.711 0.080 0.700 0.188 0.032
#&gt; SRR1812746     3  0.4262      0.755 0.008 0.000 0.756 0.236
#&gt; SRR1812745     3  0.4360      0.746 0.008 0.000 0.744 0.248
#&gt; SRR1812747     2  0.1302      0.908 0.000 0.956 0.000 0.044
#&gt; SRR1812744     4  0.1953      0.831 0.012 0.032 0.012 0.944
#&gt; SRR1812743     1  0.3286      0.909 0.876 0.000 0.044 0.080
#&gt; SRR1812742     1  0.3286      0.909 0.876 0.000 0.044 0.080
#&gt; SRR1812737     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812735     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812734     3  0.4656      0.862 0.160 0.000 0.784 0.056
#&gt; SRR1812733     4  0.3370      0.808 0.000 0.080 0.048 0.872
#&gt; SRR1812736     3  0.4656      0.862 0.160 0.000 0.784 0.056
#&gt; SRR1812732     4  0.1953      0.831 0.012 0.032 0.012 0.944
#&gt; SRR1812730     4  0.3636      0.724 0.008 0.000 0.172 0.820
#&gt; SRR1812731     4  0.5058      0.783 0.128 0.104 0.000 0.768
#&gt; SRR1812729     2  0.0921      0.917 0.000 0.972 0.000 0.028
#&gt; SRR1812727     1  0.3542      0.858 0.852 0.000 0.028 0.120
#&gt; SRR1812726     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812728     4  0.3617      0.801 0.124 0.012 0.012 0.852
#&gt; SRR1812724     4  0.3676      0.812 0.112 0.020 0.012 0.856
#&gt; SRR1812725     4  0.3672      0.784 0.000 0.164 0.012 0.824
#&gt; SRR1812723     2  0.0000      0.928 0.000 1.000 0.000 0.000
#&gt; SRR1812722     2  0.0000      0.928 0.000 1.000 0.000 0.000
#&gt; SRR1812721     1  0.2149      0.899 0.912 0.000 0.000 0.088
#&gt; SRR1812718     2  0.1302      0.908 0.000 0.956 0.000 0.044
#&gt; SRR1812717     2  0.0817      0.919 0.000 0.976 0.000 0.024
#&gt; SRR1812716     4  0.3392      0.807 0.000 0.072 0.056 0.872
#&gt; SRR1812715     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812714     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812719     4  0.5639      0.458 0.324 0.000 0.040 0.636
#&gt; SRR1812713     4  0.3672      0.784 0.000 0.164 0.012 0.824
#&gt; SRR1812712     4  0.3672      0.784 0.000 0.164 0.012 0.824
#&gt; SRR1812711     2  0.0000      0.928 0.000 1.000 0.000 0.000
#&gt; SRR1812710     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812709     4  0.4784      0.805 0.100 0.112 0.000 0.788
#&gt; SRR1812708     2  0.3707      0.847 0.004 0.852 0.112 0.032
#&gt; SRR1812707     2  0.0188      0.929 0.000 0.996 0.004 0.000
#&gt; SRR1812705     2  0.0000      0.928 0.000 1.000 0.000 0.000
#&gt; SRR1812706     4  0.4070      0.783 0.132 0.000 0.044 0.824
#&gt; SRR1812704     4  0.3672      0.821 0.092 0.032 0.012 0.864
#&gt; SRR1812703     2  0.3895      0.750 0.000 0.804 0.012 0.184
#&gt; SRR1812702     4  0.3392      0.807 0.000 0.072 0.056 0.872
#&gt; SRR1812741     1  0.2149      0.899 0.912 0.000 0.000 0.088
#&gt; SRR1812740     3  0.4656      0.862 0.160 0.000 0.784 0.056
#&gt; SRR1812739     4  0.3672      0.784 0.000 0.164 0.012 0.824
#&gt; SRR1812738     4  0.3444      0.813 0.104 0.016 0.012 0.868
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-4'>
<p><a id='tab-ATC-kmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     4  0.1605      0.806 0.004 0.000 0.040 0.944 0.012
#&gt; SRR1812753     4  0.1605      0.806 0.004 0.000 0.040 0.944 0.012
#&gt; SRR1812751     2  0.5457      0.450 0.000 0.480 0.460 0.060 0.000
#&gt; SRR1812750     2  0.5457      0.450 0.000 0.480 0.460 0.060 0.000
#&gt; SRR1812748     3  0.5300      1.000 0.468 0.000 0.492 0.032 0.008
#&gt; SRR1812749     2  0.5547      0.457 0.004 0.484 0.456 0.056 0.000
#&gt; SRR1812746     1  0.4849      0.620 0.548 0.000 0.432 0.004 0.016
#&gt; SRR1812745     1  0.4849      0.620 0.548 0.000 0.432 0.004 0.016
#&gt; SRR1812747     2  0.2891      0.739 0.176 0.824 0.000 0.000 0.000
#&gt; SRR1812744     5  0.2621      0.739 0.112 0.004 0.000 0.008 0.876
#&gt; SRR1812743     4  0.2179      0.869 0.000 0.000 0.004 0.896 0.100
#&gt; SRR1812742     4  0.2179      0.869 0.000 0.000 0.004 0.896 0.100
#&gt; SRR1812737     2  0.0609      0.831 0.000 0.980 0.020 0.000 0.000
#&gt; SRR1812735     2  0.0510      0.832 0.000 0.984 0.016 0.000 0.000
#&gt; SRR1812734     1  0.5299     -0.556 0.496 0.000 0.464 0.032 0.008
#&gt; SRR1812733     5  0.4594      0.654 0.484 0.004 0.004 0.000 0.508
#&gt; SRR1812736     3  0.5300      1.000 0.468 0.000 0.492 0.032 0.008
#&gt; SRR1812732     5  0.2621      0.739 0.112 0.004 0.000 0.008 0.876
#&gt; SRR1812730     5  0.3622      0.704 0.124 0.000 0.056 0.000 0.820
#&gt; SRR1812731     5  0.2492      0.684 0.020 0.008 0.000 0.072 0.900
#&gt; SRR1812729     2  0.1671      0.808 0.076 0.924 0.000 0.000 0.000
#&gt; SRR1812727     4  0.4538      0.467 0.000 0.000 0.008 0.540 0.452
#&gt; SRR1812726     2  0.0324      0.833 0.004 0.992 0.004 0.000 0.000
#&gt; SRR1812728     5  0.0566      0.722 0.000 0.000 0.004 0.012 0.984
#&gt; SRR1812724     5  0.0324      0.730 0.000 0.004 0.000 0.004 0.992
#&gt; SRR1812725     5  0.4894      0.658 0.456 0.024 0.000 0.000 0.520
#&gt; SRR1812723     2  0.0703      0.828 0.024 0.976 0.000 0.000 0.000
#&gt; SRR1812722     2  0.0404      0.833 0.000 0.988 0.012 0.000 0.000
#&gt; SRR1812721     4  0.2280      0.868 0.000 0.000 0.000 0.880 0.120
#&gt; SRR1812718     2  0.3838      0.652 0.280 0.716 0.000 0.000 0.004
#&gt; SRR1812717     2  0.1608      0.809 0.072 0.928 0.000 0.000 0.000
#&gt; SRR1812716     5  0.4572      0.665 0.452 0.004 0.004 0.000 0.540
#&gt; SRR1812715     2  0.0510      0.832 0.000 0.984 0.016 0.000 0.000
#&gt; SRR1812714     2  0.0324      0.833 0.004 0.992 0.004 0.000 0.000
#&gt; SRR1812719     5  0.2321      0.666 0.024 0.000 0.008 0.056 0.912
#&gt; SRR1812713     5  0.4894      0.658 0.456 0.024 0.000 0.000 0.520
#&gt; SRR1812712     5  0.4894      0.658 0.456 0.024 0.000 0.000 0.520
#&gt; SRR1812711     2  0.0404      0.831 0.012 0.988 0.000 0.000 0.000
#&gt; SRR1812710     2  0.0609      0.831 0.000 0.980 0.020 0.000 0.000
#&gt; SRR1812709     5  0.1059      0.732 0.020 0.008 0.000 0.004 0.968
#&gt; SRR1812708     2  0.6587      0.487 0.156 0.488 0.344 0.000 0.012
#&gt; SRR1812707     2  0.0290      0.833 0.000 0.992 0.008 0.000 0.000
#&gt; SRR1812705     2  0.0290      0.831 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1812706     5  0.1116      0.720 0.028 0.000 0.004 0.004 0.964
#&gt; SRR1812704     5  0.0486      0.730 0.004 0.004 0.000 0.004 0.988
#&gt; SRR1812703     2  0.5232      0.350 0.456 0.500 0.000 0.000 0.044
#&gt; SRR1812702     5  0.4594      0.654 0.484 0.004 0.004 0.000 0.508
#&gt; SRR1812741     4  0.2329      0.868 0.000 0.000 0.000 0.876 0.124
#&gt; SRR1812740     3  0.5300      1.000 0.468 0.000 0.492 0.032 0.008
#&gt; SRR1812739     5  0.4968      0.656 0.456 0.028 0.000 0.000 0.516
#&gt; SRR1812738     5  0.0648      0.728 0.004 0.004 0.004 0.004 0.984
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-kmeans-get-classes-5'>
<p><a id='tab-ATC-kmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     6  0.2509      0.842 0.088 0.000 0.000 0.036 0.000 0.876
#&gt; SRR1812753     6  0.2509      0.842 0.088 0.000 0.000 0.036 0.000 0.876
#&gt; SRR1812751     1  0.4251      0.859 0.624 0.348 0.000 0.000 0.000 0.028
#&gt; SRR1812750     1  0.4251      0.859 0.624 0.348 0.000 0.000 0.000 0.028
#&gt; SRR1812748     3  0.0146      0.921 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1812749     1  0.4180      0.858 0.628 0.348 0.000 0.000 0.000 0.024
#&gt; SRR1812746     3  0.3395      0.887 0.136 0.000 0.812 0.048 0.004 0.000
#&gt; SRR1812745     3  0.3395      0.887 0.136 0.000 0.812 0.048 0.004 0.000
#&gt; SRR1812747     2  0.3776      0.653 0.052 0.760 0.000 0.188 0.000 0.000
#&gt; SRR1812744     5  0.3645      0.757 0.064 0.000 0.000 0.152 0.784 0.000
#&gt; SRR1812743     6  0.2060      0.916 0.016 0.000 0.000 0.000 0.084 0.900
#&gt; SRR1812742     6  0.2060      0.916 0.016 0.000 0.000 0.000 0.084 0.900
#&gt; SRR1812737     2  0.0260      0.853 0.008 0.992 0.000 0.000 0.000 0.000
#&gt; SRR1812735     2  0.0260      0.853 0.008 0.992 0.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.2203      0.914 0.084 0.000 0.896 0.016 0.000 0.004
#&gt; SRR1812733     4  0.2839      0.911 0.044 0.000 0.004 0.860 0.092 0.000
#&gt; SRR1812736     3  0.0146      0.921 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1812732     5  0.3645      0.757 0.064 0.000 0.000 0.152 0.784 0.000
#&gt; SRR1812730     5  0.5694      0.518 0.188 0.000 0.012 0.224 0.576 0.000
#&gt; SRR1812731     5  0.3549      0.776 0.020 0.032 0.000 0.020 0.836 0.092
#&gt; SRR1812729     2  0.2908      0.771 0.048 0.848 0.000 0.104 0.000 0.000
#&gt; SRR1812727     5  0.3777      0.727 0.084 0.000 0.000 0.004 0.788 0.124
#&gt; SRR1812726     2  0.0632      0.852 0.000 0.976 0.000 0.024 0.000 0.000
#&gt; SRR1812728     5  0.1745      0.855 0.068 0.000 0.000 0.012 0.920 0.000
#&gt; SRR1812724     5  0.0937      0.865 0.000 0.000 0.000 0.040 0.960 0.000
#&gt; SRR1812725     4  0.1957      0.923 0.000 0.000 0.000 0.888 0.112 0.000
#&gt; SRR1812723     2  0.2660      0.789 0.048 0.868 0.000 0.084 0.000 0.000
#&gt; SRR1812722     2  0.0260      0.853 0.008 0.992 0.000 0.000 0.000 0.000
#&gt; SRR1812721     6  0.1806      0.916 0.004 0.000 0.000 0.000 0.088 0.908
#&gt; SRR1812718     2  0.4853      0.112 0.056 0.488 0.000 0.456 0.000 0.000
#&gt; SRR1812717     2  0.3113      0.765 0.048 0.856 0.000 0.072 0.024 0.000
#&gt; SRR1812716     4  0.3049      0.902 0.048 0.000 0.004 0.844 0.104 0.000
#&gt; SRR1812715     2  0.0260      0.853 0.008 0.992 0.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.1010      0.846 0.004 0.960 0.000 0.036 0.000 0.000
#&gt; SRR1812719     5  0.2056      0.845 0.080 0.000 0.000 0.012 0.904 0.004
#&gt; SRR1812713     4  0.1957      0.923 0.000 0.000 0.000 0.888 0.112 0.000
#&gt; SRR1812712     4  0.1957      0.923 0.000 0.000 0.000 0.888 0.112 0.000
#&gt; SRR1812711     2  0.0790      0.851 0.000 0.968 0.000 0.032 0.000 0.000
#&gt; SRR1812710     2  0.0260      0.853 0.008 0.992 0.000 0.000 0.000 0.000
#&gt; SRR1812709     5  0.1549      0.857 0.020 0.000 0.000 0.044 0.936 0.000
#&gt; SRR1812708     1  0.6057      0.557 0.484 0.268 0.000 0.240 0.008 0.000
#&gt; SRR1812707     2  0.0000      0.854 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812705     2  0.0692      0.852 0.020 0.976 0.000 0.004 0.000 0.000
#&gt; SRR1812706     5  0.2376      0.852 0.068 0.000 0.000 0.044 0.888 0.000
#&gt; SRR1812704     5  0.0865      0.865 0.000 0.000 0.000 0.036 0.964 0.000
#&gt; SRR1812703     4  0.2839      0.718 0.044 0.092 0.000 0.860 0.004 0.000
#&gt; SRR1812702     4  0.2839      0.911 0.044 0.000 0.004 0.860 0.092 0.000
#&gt; SRR1812741     6  0.2062      0.915 0.008 0.000 0.000 0.004 0.088 0.900
#&gt; SRR1812740     3  0.0146      0.921 0.000 0.000 0.996 0.000 0.000 0.004
#&gt; SRR1812739     4  0.2979      0.895 0.044 0.000 0.000 0.840 0.116 0.000
#&gt; SRR1812738     5  0.0777      0.865 0.004 0.000 0.000 0.024 0.972 0.000
</code></pre>

<script>
$('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-kmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-kmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-kmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-kmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-kmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-kmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-kmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-kmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-kmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-kmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-kmeans-signature_compare](figure_cola/ATC-kmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-kmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-kmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-kmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-kmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-kmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-kmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-kmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-kmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-kmeans-collect-classes](figure_cola/ATC-kmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:skmeans*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "skmeans"]
# you can also extract it by
# res = res_list["ATC:skmeans"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'skmeans' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 6.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-skmeans-collect-plots](figure_cola/ATC-skmeans-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-skmeans-select-partition-number](figure_cola/ATC-skmeans-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.975       0.989         0.5088 0.492   0.492
#> 3 3 0.881           0.934       0.962         0.3112 0.711   0.479
#> 4 4 0.874           0.912       0.958         0.1118 0.906   0.722
#> 5 5 0.877           0.868       0.901         0.0560 0.929   0.740
#> 6 6 0.909           0.896       0.921         0.0447 0.953   0.789
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 6
#> attr(,"optional")
#> [1] 2
```

There is also optional best $k$ = 2 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-classes'>
<ul>
<li><a href='#tab-ATC-skmeans-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-skmeans-get-classes-1'>
<p><a id='tab-ATC-skmeans-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000      0.988 1.000 0.000
#&gt; SRR1812753     1   0.000      0.988 1.000 0.000
#&gt; SRR1812751     2   0.000      0.988 0.000 1.000
#&gt; SRR1812750     2   0.000      0.988 0.000 1.000
#&gt; SRR1812748     1   0.000      0.988 1.000 0.000
#&gt; SRR1812749     2   0.000      0.988 0.000 1.000
#&gt; SRR1812746     1   0.000      0.988 1.000 0.000
#&gt; SRR1812745     1   0.000      0.988 1.000 0.000
#&gt; SRR1812747     2   0.000      0.988 0.000 1.000
#&gt; SRR1812744     1   0.000      0.988 1.000 0.000
#&gt; SRR1812743     1   0.000      0.988 1.000 0.000
#&gt; SRR1812742     1   0.000      0.988 1.000 0.000
#&gt; SRR1812737     2   0.000      0.988 0.000 1.000
#&gt; SRR1812735     2   0.000      0.988 0.000 1.000
#&gt; SRR1812734     1   0.000      0.988 1.000 0.000
#&gt; SRR1812733     2   0.000      0.988 0.000 1.000
#&gt; SRR1812736     1   0.000      0.988 1.000 0.000
#&gt; SRR1812732     1   0.000      0.988 1.000 0.000
#&gt; SRR1812730     1   0.000      0.988 1.000 0.000
#&gt; SRR1812731     1   0.781      0.694 0.768 0.232
#&gt; SRR1812729     2   0.000      0.988 0.000 1.000
#&gt; SRR1812727     1   0.000      0.988 1.000 0.000
#&gt; SRR1812726     2   0.000      0.988 0.000 1.000
#&gt; SRR1812728     1   0.000      0.988 1.000 0.000
#&gt; SRR1812724     1   0.000      0.988 1.000 0.000
#&gt; SRR1812725     2   0.000      0.988 0.000 1.000
#&gt; SRR1812723     2   0.000      0.988 0.000 1.000
#&gt; SRR1812722     2   0.000      0.988 0.000 1.000
#&gt; SRR1812721     1   0.000      0.988 1.000 0.000
#&gt; SRR1812718     2   0.000      0.988 0.000 1.000
#&gt; SRR1812717     2   0.000      0.988 0.000 1.000
#&gt; SRR1812716     1   0.224      0.954 0.964 0.036
#&gt; SRR1812715     2   0.000      0.988 0.000 1.000
#&gt; SRR1812714     2   0.000      0.988 0.000 1.000
#&gt; SRR1812719     1   0.000      0.988 1.000 0.000
#&gt; SRR1812713     2   0.000      0.988 0.000 1.000
#&gt; SRR1812712     2   0.000      0.988 0.000 1.000
#&gt; SRR1812711     2   0.000      0.988 0.000 1.000
#&gt; SRR1812710     2   0.000      0.988 0.000 1.000
#&gt; SRR1812709     2   0.260      0.946 0.044 0.956
#&gt; SRR1812708     2   0.000      0.988 0.000 1.000
#&gt; SRR1812707     2   0.000      0.988 0.000 1.000
#&gt; SRR1812705     2   0.000      0.988 0.000 1.000
#&gt; SRR1812706     1   0.000      0.988 1.000 0.000
#&gt; SRR1812704     1   0.000      0.988 1.000 0.000
#&gt; SRR1812703     2   0.000      0.988 0.000 1.000
#&gt; SRR1812702     2   0.808      0.667 0.248 0.752
#&gt; SRR1812741     1   0.000      0.988 1.000 0.000
#&gt; SRR1812740     1   0.000      0.988 1.000 0.000
#&gt; SRR1812739     2   0.000      0.988 0.000 1.000
#&gt; SRR1812738     1   0.000      0.988 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-1-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-2'>
<p><a id='tab-ATC-skmeans-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812753     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812751     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812750     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812748     3   0.304      0.881 0.104 0.000 0.896
#&gt; SRR1812749     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812746     3   0.164      0.903 0.044 0.000 0.956
#&gt; SRR1812745     3   0.153      0.902 0.040 0.000 0.960
#&gt; SRR1812747     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812744     3   0.164      0.903 0.044 0.000 0.956
#&gt; SRR1812743     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812742     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812737     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812735     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812734     3   0.304      0.881 0.104 0.000 0.896
#&gt; SRR1812733     3   0.000      0.893 0.000 0.000 1.000
#&gt; SRR1812736     3   0.304      0.881 0.104 0.000 0.896
#&gt; SRR1812732     3   0.164      0.903 0.044 0.000 0.956
#&gt; SRR1812730     3   0.164      0.903 0.044 0.000 0.956
#&gt; SRR1812731     1   0.164      0.932 0.956 0.044 0.000
#&gt; SRR1812729     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812727     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812726     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812728     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812724     1   0.103      0.956 0.976 0.000 0.024
#&gt; SRR1812725     3   0.394      0.800 0.000 0.156 0.844
#&gt; SRR1812723     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812722     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812721     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812718     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812717     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812716     3   0.000      0.893 0.000 0.000 1.000
#&gt; SRR1812715     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812714     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812719     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812713     3   0.394      0.800 0.000 0.156 0.844
#&gt; SRR1812712     3   0.254      0.857 0.000 0.080 0.920
#&gt; SRR1812711     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812710     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812709     1   0.435      0.769 0.816 0.184 0.000
#&gt; SRR1812708     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812707     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812705     2   0.000      0.996 0.000 1.000 0.000
#&gt; SRR1812706     3   0.475      0.761 0.216 0.000 0.784
#&gt; SRR1812704     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812703     2   0.245      0.921 0.000 0.924 0.076
#&gt; SRR1812702     3   0.000      0.893 0.000 0.000 1.000
#&gt; SRR1812741     1   0.000      0.977 1.000 0.000 0.000
#&gt; SRR1812740     3   0.304      0.881 0.104 0.000 0.896
#&gt; SRR1812739     3   0.590      0.475 0.000 0.352 0.648
#&gt; SRR1812738     1   0.000      0.977 1.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-2-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-3'>
<p><a id='tab-ATC-skmeans-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0336     0.9019 0.992 0.000 0.000 0.008
#&gt; SRR1812753     1  0.0336     0.9019 0.992 0.000 0.000 0.008
#&gt; SRR1812751     2  0.0336     0.9842 0.000 0.992 0.000 0.008
#&gt; SRR1812750     2  0.0336     0.9842 0.000 0.992 0.000 0.008
#&gt; SRR1812748     3  0.0000     0.9734 0.000 0.000 1.000 0.000
#&gt; SRR1812749     2  0.0336     0.9842 0.000 0.992 0.000 0.008
#&gt; SRR1812746     3  0.0000     0.9734 0.000 0.000 1.000 0.000
#&gt; SRR1812745     3  0.0469     0.9684 0.000 0.000 0.988 0.012
#&gt; SRR1812747     2  0.0707     0.9712 0.000 0.980 0.000 0.020
#&gt; SRR1812744     3  0.0469     0.9684 0.000 0.000 0.988 0.012
#&gt; SRR1812743     1  0.0000     0.9038 1.000 0.000 0.000 0.000
#&gt; SRR1812742     1  0.0000     0.9038 1.000 0.000 0.000 0.000
#&gt; SRR1812737     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812735     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812734     3  0.0000     0.9734 0.000 0.000 1.000 0.000
#&gt; SRR1812733     4  0.0336     0.9240 0.000 0.000 0.008 0.992
#&gt; SRR1812736     3  0.0000     0.9734 0.000 0.000 1.000 0.000
#&gt; SRR1812732     3  0.0469     0.9684 0.000 0.000 0.988 0.012
#&gt; SRR1812730     3  0.0000     0.9734 0.000 0.000 1.000 0.000
#&gt; SRR1812731     1  0.0000     0.9038 1.000 0.000 0.000 0.000
#&gt; SRR1812729     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812727     1  0.2530     0.8720 0.888 0.000 0.112 0.000
#&gt; SRR1812726     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812728     1  0.2589     0.8699 0.884 0.000 0.116 0.000
#&gt; SRR1812724     1  0.2593     0.8725 0.892 0.000 0.104 0.004
#&gt; SRR1812725     4  0.0336     0.9240 0.000 0.000 0.008 0.992
#&gt; SRR1812723     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812722     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812721     1  0.0000     0.9038 1.000 0.000 0.000 0.000
#&gt; SRR1812718     4  0.4985     0.0979 0.000 0.468 0.000 0.532
#&gt; SRR1812717     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812716     4  0.0592     0.9181 0.000 0.000 0.016 0.984
#&gt; SRR1812715     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812719     1  0.4356     0.6756 0.708 0.000 0.292 0.000
#&gt; SRR1812713     4  0.0336     0.9240 0.000 0.000 0.008 0.992
#&gt; SRR1812712     4  0.0336     0.9240 0.000 0.000 0.008 0.992
#&gt; SRR1812711     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812710     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812709     1  0.2345     0.8251 0.900 0.100 0.000 0.000
#&gt; SRR1812708     2  0.2921     0.8321 0.000 0.860 0.000 0.140
#&gt; SRR1812707     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812705     2  0.0000     0.9887 0.000 1.000 0.000 0.000
#&gt; SRR1812706     3  0.3448     0.7586 0.168 0.000 0.828 0.004
#&gt; SRR1812704     1  0.3554     0.8496 0.844 0.000 0.136 0.020
#&gt; SRR1812703     4  0.0469     0.9163 0.000 0.012 0.000 0.988
#&gt; SRR1812702     4  0.0336     0.9240 0.000 0.000 0.008 0.992
#&gt; SRR1812741     1  0.0000     0.9038 1.000 0.000 0.000 0.000
#&gt; SRR1812740     3  0.0000     0.9734 0.000 0.000 1.000 0.000
#&gt; SRR1812739     4  0.0376     0.9221 0.000 0.004 0.004 0.992
#&gt; SRR1812738     1  0.4608     0.6550 0.692 0.000 0.304 0.004
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-3-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-4'>
<p><a id='tab-ATC-skmeans-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.3366      0.842 0.768 0.000 0.000 0.000 0.232
#&gt; SRR1812753     1  0.3480      0.861 0.752 0.000 0.000 0.000 0.248
#&gt; SRR1812751     2  0.3837      0.717 0.308 0.692 0.000 0.000 0.000
#&gt; SRR1812750     2  0.3837      0.717 0.308 0.692 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0162      0.992 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812749     2  0.3837      0.717 0.308 0.692 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0162      0.992 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812745     3  0.0324      0.990 0.000 0.000 0.992 0.004 0.004
#&gt; SRR1812747     2  0.0963      0.883 0.000 0.964 0.000 0.036 0.000
#&gt; SRR1812744     3  0.0566      0.981 0.012 0.000 0.984 0.004 0.000
#&gt; SRR1812743     1  0.3949      0.929 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1812742     1  0.3949      0.929 0.668 0.000 0.000 0.000 0.332
#&gt; SRR1812737     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0162      0.992 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812733     4  0.0000      0.994 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812736     3  0.0162      0.992 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812732     3  0.1179      0.966 0.016 0.000 0.964 0.004 0.016
#&gt; SRR1812730     3  0.0162      0.992 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812731     1  0.3983      0.899 0.660 0.000 0.000 0.000 0.340
#&gt; SRR1812729     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812727     5  0.2712      0.740 0.088 0.000 0.032 0.000 0.880
#&gt; SRR1812726     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812728     5  0.1403      0.779 0.024 0.000 0.024 0.000 0.952
#&gt; SRR1812724     5  0.1907      0.759 0.028 0.000 0.044 0.000 0.928
#&gt; SRR1812725     4  0.0000      0.994 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812723     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812722     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812721     1  0.3966      0.928 0.664 0.000 0.000 0.000 0.336
#&gt; SRR1812718     2  0.4278      0.220 0.000 0.548 0.000 0.452 0.000
#&gt; SRR1812717     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812716     4  0.0880      0.965 0.000 0.000 0.032 0.968 0.000
#&gt; SRR1812715     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0290      0.902 0.008 0.992 0.000 0.000 0.000
#&gt; SRR1812719     5  0.2864      0.773 0.024 0.000 0.112 0.000 0.864
#&gt; SRR1812713     4  0.0000      0.994 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812712     4  0.0162      0.992 0.000 0.000 0.000 0.996 0.004
#&gt; SRR1812711     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812710     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812709     5  0.3745      0.485 0.196 0.024 0.000 0.000 0.780
#&gt; SRR1812708     2  0.5393      0.641 0.312 0.608 0.000 0.080 0.000
#&gt; SRR1812707     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812705     2  0.0000      0.906 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812706     5  0.4182      0.400 0.000 0.000 0.400 0.000 0.600
#&gt; SRR1812704     5  0.1012      0.779 0.012 0.000 0.020 0.000 0.968
#&gt; SRR1812703     4  0.0000      0.994 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812702     4  0.0000      0.994 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812741     1  0.3966      0.928 0.664 0.000 0.000 0.000 0.336
#&gt; SRR1812740     3  0.0162      0.992 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812739     4  0.0162      0.992 0.004 0.000 0.000 0.996 0.000
#&gt; SRR1812738     5  0.2915      0.765 0.024 0.000 0.116 0.000 0.860
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-4-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-skmeans-get-classes-5'>
<p><a id='tab-ATC-skmeans-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     6  0.1720      0.940 0.040 0.000 0.000 0.000 0.032 0.928
#&gt; SRR1812753     6  0.1418      0.950 0.024 0.000 0.000 0.000 0.032 0.944
#&gt; SRR1812751     1  0.3126      0.968 0.752 0.248 0.000 0.000 0.000 0.000
#&gt; SRR1812750     1  0.3126      0.968 0.752 0.248 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0363      0.955 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; SRR1812749     1  0.3126      0.968 0.752 0.248 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0146      0.954 0.000 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812745     3  0.0291      0.953 0.000 0.000 0.992 0.004 0.004 0.000
#&gt; SRR1812747     2  0.1401      0.897 0.020 0.948 0.000 0.028 0.004 0.000
#&gt; SRR1812744     3  0.2839      0.868 0.092 0.000 0.860 0.000 0.044 0.004
#&gt; SRR1812743     6  0.0000      0.961 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812742     6  0.0000      0.961 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812737     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0260      0.955 0.000 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1812733     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812736     3  0.0363      0.955 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; SRR1812732     3  0.3633      0.826 0.124 0.000 0.808 0.000 0.052 0.016
#&gt; SRR1812730     3  0.0837      0.947 0.004 0.000 0.972 0.004 0.020 0.000
#&gt; SRR1812731     6  0.1462      0.923 0.056 0.000 0.000 0.000 0.008 0.936
#&gt; SRR1812729     2  0.0603      0.927 0.016 0.980 0.000 0.000 0.004 0.000
#&gt; SRR1812727     5  0.3394      0.745 0.000 0.000 0.024 0.000 0.776 0.200
#&gt; SRR1812726     2  0.0603      0.925 0.016 0.980 0.000 0.000 0.004 0.000
#&gt; SRR1812728     5  0.2106      0.827 0.000 0.000 0.032 0.000 0.904 0.064
#&gt; SRR1812724     5  0.4486      0.735 0.052 0.000 0.032 0.000 0.732 0.184
#&gt; SRR1812725     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812723     2  0.0603      0.927 0.016 0.980 0.000 0.000 0.004 0.000
#&gt; SRR1812722     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812721     6  0.0603      0.960 0.004 0.000 0.000 0.000 0.016 0.980
#&gt; SRR1812718     2  0.4481      0.188 0.024 0.556 0.000 0.416 0.004 0.000
#&gt; SRR1812717     2  0.0146      0.936 0.004 0.996 0.000 0.000 0.000 0.000
#&gt; SRR1812716     4  0.1219      0.941 0.004 0.000 0.048 0.948 0.000 0.000
#&gt; SRR1812715     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.1700      0.849 0.080 0.916 0.000 0.000 0.004 0.000
#&gt; SRR1812719     5  0.2527      0.825 0.000 0.000 0.084 0.000 0.876 0.040
#&gt; SRR1812713     4  0.0146      0.975 0.004 0.000 0.000 0.996 0.000 0.000
#&gt; SRR1812712     4  0.1257      0.955 0.028 0.000 0.000 0.952 0.020 0.000
#&gt; SRR1812711     2  0.0146      0.936 0.000 0.996 0.000 0.000 0.004 0.000
#&gt; SRR1812710     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812709     5  0.5457      0.600 0.160 0.012 0.000 0.000 0.612 0.216
#&gt; SRR1812708     1  0.3122      0.900 0.804 0.176 0.000 0.020 0.000 0.000
#&gt; SRR1812707     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812705     2  0.0000      0.937 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812706     5  0.3586      0.661 0.012 0.000 0.268 0.000 0.720 0.000
#&gt; SRR1812704     5  0.1528      0.823 0.012 0.000 0.016 0.000 0.944 0.028
#&gt; SRR1812703     4  0.0458      0.970 0.016 0.000 0.000 0.984 0.000 0.000
#&gt; SRR1812702     4  0.0000      0.976 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812741     6  0.0790      0.958 0.000 0.000 0.000 0.000 0.032 0.968
#&gt; SRR1812740     3  0.0363      0.955 0.000 0.000 0.988 0.000 0.012 0.000
#&gt; SRR1812739     4  0.1549      0.949 0.044 0.000 0.000 0.936 0.020 0.000
#&gt; SRR1812738     5  0.3149      0.814 0.020 0.000 0.076 0.000 0.852 0.052
</code></pre>

<script>
$('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-skmeans-get-classes-5-a').click(function(){
  $('#tab-ATC-skmeans-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-skmeans-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-skmeans-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-skmeans-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-skmeans-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-skmeans-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-skmeans-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-skmeans-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-skmeans-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-skmeans-signature_compare](figure_cola/ATC-skmeans-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-skmeans-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-skmeans-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-skmeans-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-skmeans-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-skmeans-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-skmeans-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-skmeans-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-skmeans-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-skmeans-collect-classes](figure_cola/ATC-skmeans-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:pam*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "pam"]
# you can also extract it by
# res = res_list["ATC:pam"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'pam' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 5.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-pam-collect-plots](figure_cola/ATC-pam-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-pam-select-partition-number](figure_cola/ATC-pam-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 1.000           0.995       0.998         0.5086 0.492   0.492
#> 3 3 0.779           0.893       0.934         0.2242 0.901   0.799
#> 4 4 0.957           0.925       0.971         0.1711 0.831   0.595
#> 5 5 0.909           0.876       0.950         0.0449 0.965   0.873
#> 6 6 0.934           0.906       0.954         0.0139 0.997   0.987
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 5
#> attr(,"optional")
#> [1] 2 4
```

There is also optional best $k$ = 2 4 that is worth to check.

Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-pam-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-classes'>
<ul>
<li><a href='#tab-ATC-pam-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-pam-get-classes-1'>
<p><a id='tab-ATC-pam-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1   0.000      0.998 1.000 0.000
#&gt; SRR1812753     1   0.000      0.998 1.000 0.000
#&gt; SRR1812751     2   0.000      0.997 0.000 1.000
#&gt; SRR1812750     2   0.000      0.997 0.000 1.000
#&gt; SRR1812748     1   0.000      0.998 1.000 0.000
#&gt; SRR1812749     2   0.000      0.997 0.000 1.000
#&gt; SRR1812746     1   0.000      0.998 1.000 0.000
#&gt; SRR1812745     1   0.000      0.998 1.000 0.000
#&gt; SRR1812747     2   0.000      0.997 0.000 1.000
#&gt; SRR1812744     1   0.000      0.998 1.000 0.000
#&gt; SRR1812743     1   0.000      0.998 1.000 0.000
#&gt; SRR1812742     1   0.000      0.998 1.000 0.000
#&gt; SRR1812737     2   0.000      0.997 0.000 1.000
#&gt; SRR1812735     2   0.000      0.997 0.000 1.000
#&gt; SRR1812734     1   0.000      0.998 1.000 0.000
#&gt; SRR1812733     2   0.388      0.917 0.076 0.924
#&gt; SRR1812736     1   0.000      0.998 1.000 0.000
#&gt; SRR1812732     1   0.000      0.998 1.000 0.000
#&gt; SRR1812730     1   0.000      0.998 1.000 0.000
#&gt; SRR1812731     1   0.000      0.998 1.000 0.000
#&gt; SRR1812729     2   0.000      0.997 0.000 1.000
#&gt; SRR1812727     1   0.000      0.998 1.000 0.000
#&gt; SRR1812726     2   0.000      0.997 0.000 1.000
#&gt; SRR1812728     1   0.000      0.998 1.000 0.000
#&gt; SRR1812724     1   0.000      0.998 1.000 0.000
#&gt; SRR1812725     2   0.000      0.997 0.000 1.000
#&gt; SRR1812723     2   0.000      0.997 0.000 1.000
#&gt; SRR1812722     2   0.000      0.997 0.000 1.000
#&gt; SRR1812721     1   0.000      0.998 1.000 0.000
#&gt; SRR1812718     2   0.000      0.997 0.000 1.000
#&gt; SRR1812717     2   0.000      0.997 0.000 1.000
#&gt; SRR1812716     1   0.000      0.998 1.000 0.000
#&gt; SRR1812715     2   0.000      0.997 0.000 1.000
#&gt; SRR1812714     2   0.000      0.997 0.000 1.000
#&gt; SRR1812719     1   0.000      0.998 1.000 0.000
#&gt; SRR1812713     2   0.000      0.997 0.000 1.000
#&gt; SRR1812712     1   0.242      0.958 0.960 0.040
#&gt; SRR1812711     2   0.000      0.997 0.000 1.000
#&gt; SRR1812710     2   0.000      0.997 0.000 1.000
#&gt; SRR1812709     1   0.000      0.998 1.000 0.000
#&gt; SRR1812708     2   0.000      0.997 0.000 1.000
#&gt; SRR1812707     2   0.000      0.997 0.000 1.000
#&gt; SRR1812705     2   0.000      0.997 0.000 1.000
#&gt; SRR1812706     1   0.000      0.998 1.000 0.000
#&gt; SRR1812704     1   0.000      0.998 1.000 0.000
#&gt; SRR1812703     2   0.000      0.997 0.000 1.000
#&gt; SRR1812702     1   0.000      0.998 1.000 0.000
#&gt; SRR1812741     1   0.000      0.998 1.000 0.000
#&gt; SRR1812740     1   0.000      0.998 1.000 0.000
#&gt; SRR1812739     2   0.000      0.997 0.000 1.000
#&gt; SRR1812738     1   0.000      0.998 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-1-a').click(function(){
  $('#tab-ATC-pam-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-2'>
<p><a id='tab-ATC-pam-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812751     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812750     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812748     3  0.3879      0.924 0.152 0.000 0.848
#&gt; SRR1812749     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812746     3  0.0592      0.849 0.012 0.000 0.988
#&gt; SRR1812745     3  0.0000      0.837 0.000 0.000 1.000
#&gt; SRR1812747     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812744     1  0.3879      0.874 0.848 0.000 0.152
#&gt; SRR1812743     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812742     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812737     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812735     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812734     3  0.3879      0.924 0.152 0.000 0.848
#&gt; SRR1812733     2  0.5639      0.735 0.016 0.752 0.232
#&gt; SRR1812736     3  0.3879      0.924 0.152 0.000 0.848
#&gt; SRR1812732     1  0.3879      0.874 0.848 0.000 0.152
#&gt; SRR1812730     1  0.4931      0.824 0.768 0.000 0.232
#&gt; SRR1812731     1  0.3879      0.874 0.848 0.000 0.152
#&gt; SRR1812729     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812727     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812726     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812728     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812724     1  0.3879      0.874 0.848 0.000 0.152
#&gt; SRR1812725     2  0.4750      0.774 0.000 0.784 0.216
#&gt; SRR1812723     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812722     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812721     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812718     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812717     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812716     1  0.4931      0.824 0.768 0.000 0.232
#&gt; SRR1812715     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812714     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812719     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812713     2  0.4931      0.755 0.000 0.768 0.232
#&gt; SRR1812712     1  0.6361      0.781 0.728 0.040 0.232
#&gt; SRR1812711     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812710     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812709     1  0.3879      0.874 0.848 0.000 0.152
#&gt; SRR1812708     2  0.2796      0.886 0.000 0.908 0.092
#&gt; SRR1812707     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812705     2  0.0000      0.948 0.000 1.000 0.000
#&gt; SRR1812706     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812704     1  0.3879      0.874 0.848 0.000 0.152
#&gt; SRR1812703     2  0.3879      0.836 0.000 0.848 0.152
#&gt; SRR1812702     1  0.4931      0.824 0.768 0.000 0.232
#&gt; SRR1812741     1  0.0000      0.887 1.000 0.000 0.000
#&gt; SRR1812740     3  0.3879      0.924 0.152 0.000 0.848
#&gt; SRR1812739     2  0.3879      0.836 0.000 0.848 0.152
#&gt; SRR1812738     1  0.1860      0.887 0.948 0.000 0.052
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-2-a').click(function(){
  $('#tab-ATC-pam-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-3'>
<p><a id='tab-ATC-pam-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812751     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812750     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1812749     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812746     3  0.0188      0.996 0.000 0.000 0.996 0.004
#&gt; SRR1812745     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1812747     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812744     4  0.4830      0.393 0.392 0.000 0.000 0.608
#&gt; SRR1812743     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812742     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812737     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1812733     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812736     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1812732     4  0.0592      0.892 0.016 0.000 0.000 0.984
#&gt; SRR1812730     4  0.4746      0.434 0.368 0.000 0.000 0.632
#&gt; SRR1812731     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812729     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812727     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812726     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812728     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812724     1  0.4008      0.639 0.756 0.000 0.000 0.244
#&gt; SRR1812725     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812723     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812722     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812721     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812718     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812717     2  0.4331      0.598 0.000 0.712 0.000 0.288
#&gt; SRR1812716     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812719     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812713     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812712     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812711     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812710     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812709     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812708     2  0.3444      0.777 0.000 0.816 0.000 0.184
#&gt; SRR1812707     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812705     2  0.0000      0.972 0.000 1.000 0.000 0.000
#&gt; SRR1812706     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812704     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812703     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812702     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812741     1  0.0000      0.981 1.000 0.000 0.000 0.000
#&gt; SRR1812740     3  0.0000      0.999 0.000 0.000 1.000 0.000
#&gt; SRR1812739     4  0.0000      0.903 0.000 0.000 0.000 1.000
#&gt; SRR1812738     1  0.0000      0.981 1.000 0.000 0.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-3-a').click(function(){
  $('#tab-ATC-pam-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-4'>
<p><a id='tab-ATC-pam-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.0000      0.644 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      0.644 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     2  0.2891      0.792 0.176 0.824 0.000 0.000 0.000
#&gt; SRR1812750     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812749     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.0162      0.994 0.000 0.000 0.996 0.000 0.004
#&gt; SRR1812745     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812747     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812744     5  0.4161      0.385 0.000 0.000 0.000 0.392 0.608
#&gt; SRR1812743     1  0.3999      0.567 0.656 0.000 0.000 0.344 0.000
#&gt; SRR1812742     1  0.4307      0.279 0.504 0.000 0.000 0.496 0.000
#&gt; SRR1812737     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812733     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812736     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812732     5  0.0510      0.891 0.000 0.000 0.000 0.016 0.984
#&gt; SRR1812730     5  0.4088      0.437 0.000 0.000 0.000 0.368 0.632
#&gt; SRR1812731     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812729     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812727     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812726     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812728     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812724     4  0.3452      0.561 0.000 0.000 0.000 0.756 0.244
#&gt; SRR1812725     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812723     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812722     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812721     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812718     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812717     2  0.3730      0.602 0.000 0.712 0.000 0.000 0.288
#&gt; SRR1812716     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812715     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812719     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812713     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812712     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812711     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812710     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812709     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812708     2  0.2966      0.763 0.000 0.816 0.000 0.000 0.184
#&gt; SRR1812707     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812705     2  0.0000      0.960 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812706     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812704     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812703     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812702     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812741     4  0.0880      0.917 0.032 0.000 0.000 0.968 0.000
#&gt; SRR1812740     3  0.0000      0.999 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812739     5  0.0000      0.903 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812738     4  0.0000      0.955 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-4-a').click(function(){
  $('#tab-ATC-pam-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-pam-get-classes-5'>
<p><a id='tab-ATC-pam-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812753     1  0.0000      1.000 1.000 0.000 0.000 0.000 0.000 0.000
#&gt; SRR1812751     2  0.2597      0.779 0.176 0.824 0.000 0.000 0.000 0.000
#&gt; SRR1812750     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812748     3  0.0000      0.936 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812749     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812746     3  0.2632      0.864 0.000 0.000 0.832 0.004 0.000 0.164
#&gt; SRR1812745     3  0.2491      0.866 0.000 0.000 0.836 0.000 0.000 0.164
#&gt; SRR1812747     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812744     4  0.3737      0.371 0.000 0.000 0.000 0.608 0.392 0.000
#&gt; SRR1812743     6  0.2491      1.000 0.164 0.000 0.000 0.000 0.000 0.836
#&gt; SRR1812742     6  0.2491      1.000 0.164 0.000 0.000 0.000 0.000 0.836
#&gt; SRR1812737     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812735     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812734     3  0.0000      0.936 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812733     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812736     3  0.0000      0.936 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812732     4  0.0458      0.894 0.000 0.000 0.000 0.984 0.016 0.000
#&gt; SRR1812730     4  0.5083      0.511 0.000 0.000 0.000 0.632 0.204 0.164
#&gt; SRR1812731     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812729     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812727     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812726     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812728     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812724     5  0.3101      0.640 0.000 0.000 0.000 0.244 0.756 0.000
#&gt; SRR1812725     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812723     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812722     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812721     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812718     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812717     2  0.3351      0.582 0.000 0.712 0.000 0.288 0.000 0.000
#&gt; SRR1812716     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812715     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812714     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812719     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812713     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812712     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812711     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812710     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812709     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812708     2  0.2664      0.751 0.000 0.816 0.000 0.184 0.000 0.000
#&gt; SRR1812707     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812705     2  0.0000      0.959 0.000 1.000 0.000 0.000 0.000 0.000
#&gt; SRR1812706     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812704     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812703     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812702     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812741     5  0.0790      0.936 0.032 0.000 0.000 0.000 0.968 0.000
#&gt; SRR1812740     3  0.0000      0.936 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812739     4  0.0000      0.907 0.000 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812738     5  0.0000      0.965 0.000 0.000 0.000 0.000 1.000 0.000
</code></pre>

<script>
$('#tab-ATC-pam-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-pam-get-classes-5-a').click(function(){
  $('#tab-ATC-pam-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-pam-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-pam-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-pam-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-pam-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-pam-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-pam-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-pam-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-pam-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-pam-signature_compare](figure_cola/ATC-pam-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-pam-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-pam-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-pam-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-pam-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-pam-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-pam-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-pam-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-pam-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-pam-collect-classes](figure_cola/ATC-pam-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:mclust






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "mclust"]
# you can also extract it by
# res = res_list["ATC:mclust"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'mclust' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 4.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-mclust-collect-plots](figure_cola/ATC-mclust-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-mclust-select-partition-number](figure_cola/ATC-mclust-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.405           0.818       0.887         0.3247 0.678   0.678
#> 3 3 0.365           0.574       0.730         0.6980 0.773   0.675
#> 4 4 0.490           0.699       0.801         0.2160 0.713   0.477
#> 5 5 0.557           0.487       0.704         0.0812 0.769   0.414
#> 6 6 0.696           0.706       0.823         0.1293 0.894   0.603
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 4
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-classes'>
<ul>
<li><a href='#tab-ATC-mclust-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-mclust-get-classes-1'>
<p><a id='tab-ATC-mclust-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     1  0.7139      0.755 0.804 0.196
#&gt; SRR1812753     1  0.7139      0.755 0.804 0.196
#&gt; SRR1812751     1  0.1184      0.717 0.984 0.016
#&gt; SRR1812750     1  0.2043      0.724 0.968 0.032
#&gt; SRR1812748     2  0.5059      0.763 0.112 0.888
#&gt; SRR1812749     1  0.1184      0.717 0.984 0.016
#&gt; SRR1812746     1  0.9977      0.577 0.528 0.472
#&gt; SRR1812745     2  0.2948      0.844 0.052 0.948
#&gt; SRR1812747     2  0.6148      0.835 0.152 0.848
#&gt; SRR1812744     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812743     2  0.0376      0.887 0.004 0.996
#&gt; SRR1812742     2  0.0376      0.887 0.004 0.996
#&gt; SRR1812737     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812735     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812734     1  0.9977      0.577 0.528 0.472
#&gt; SRR1812733     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812736     2  0.5408      0.743 0.124 0.876
#&gt; SRR1812732     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812730     2  0.0376      0.887 0.004 0.996
#&gt; SRR1812731     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812729     2  0.6973      0.816 0.188 0.812
#&gt; SRR1812727     1  0.9977      0.577 0.528 0.472
#&gt; SRR1812726     2  0.6801      0.821 0.180 0.820
#&gt; SRR1812728     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812724     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812725     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812723     2  0.6973      0.816 0.188 0.812
#&gt; SRR1812722     2  0.6801      0.820 0.180 0.820
#&gt; SRR1812721     2  0.0376      0.887 0.004 0.996
#&gt; SRR1812718     2  0.6148      0.835 0.152 0.848
#&gt; SRR1812717     2  0.4815      0.859 0.104 0.896
#&gt; SRR1812716     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812715     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812714     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812719     1  0.9977      0.577 0.528 0.472
#&gt; SRR1812713     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812712     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812711     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812710     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812709     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812708     1  0.6623      0.758 0.828 0.172
#&gt; SRR1812707     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812705     2  0.7139      0.810 0.196 0.804
#&gt; SRR1812706     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812704     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812703     2  0.3584      0.871 0.068 0.932
#&gt; SRR1812702     2  0.0000      0.889 0.000 1.000
#&gt; SRR1812741     2  0.0376      0.887 0.004 0.996
#&gt; SRR1812740     2  0.3114      0.839 0.056 0.944
#&gt; SRR1812739     2  0.1633      0.885 0.024 0.976
#&gt; SRR1812738     2  0.0000      0.889 0.000 1.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-1-a').click(function(){
  $('#tab-ATC-mclust-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-2'>
<p><a id='tab-ATC-mclust-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.7184      0.558 0.504 0.024 0.472
#&gt; SRR1812753     1  0.6521      0.551 0.504 0.004 0.492
#&gt; SRR1812751     1  0.6228      0.725 0.624 0.004 0.372
#&gt; SRR1812750     1  0.6330      0.726 0.600 0.004 0.396
#&gt; SRR1812748     3  0.3686      0.302 0.000 0.140 0.860
#&gt; SRR1812749     1  0.6314      0.727 0.604 0.004 0.392
#&gt; SRR1812746     3  0.1453      0.205 0.008 0.024 0.968
#&gt; SRR1812745     3  0.6505      0.181 0.004 0.468 0.528
#&gt; SRR1812747     2  0.4796      0.716 0.220 0.780 0.000
#&gt; SRR1812744     2  0.0661      0.723 0.008 0.988 0.004
#&gt; SRR1812743     3  0.8743      0.289 0.108 0.440 0.452
#&gt; SRR1812742     3  0.8737      0.306 0.108 0.428 0.464
#&gt; SRR1812737     2  0.6302      0.588 0.480 0.520 0.000
#&gt; SRR1812735     2  0.6302      0.588 0.480 0.520 0.000
#&gt; SRR1812734     3  0.1031      0.193 0.000 0.024 0.976
#&gt; SRR1812733     2  0.2313      0.726 0.032 0.944 0.024
#&gt; SRR1812736     3  0.3686      0.302 0.000 0.140 0.860
#&gt; SRR1812732     2  0.1636      0.729 0.016 0.964 0.020
#&gt; SRR1812730     2  0.1989      0.700 0.004 0.948 0.048
#&gt; SRR1812731     2  0.0237      0.726 0.004 0.996 0.000
#&gt; SRR1812729     2  0.4796      0.716 0.220 0.780 0.000
#&gt; SRR1812727     3  0.4589      0.284 0.008 0.172 0.820
#&gt; SRR1812726     2  0.6468      0.612 0.444 0.552 0.004
#&gt; SRR1812728     2  0.2486      0.681 0.008 0.932 0.060
#&gt; SRR1812724     2  0.1399      0.709 0.004 0.968 0.028
#&gt; SRR1812725     2  0.1163      0.737 0.028 0.972 0.000
#&gt; SRR1812723     2  0.5785      0.672 0.332 0.668 0.000
#&gt; SRR1812722     2  0.6260      0.612 0.448 0.552 0.000
#&gt; SRR1812721     2  0.8311     -0.109 0.112 0.596 0.292
#&gt; SRR1812718     2  0.4796      0.716 0.220 0.780 0.000
#&gt; SRR1812717     2  0.4702      0.718 0.212 0.788 0.000
#&gt; SRR1812716     2  0.0000      0.728 0.000 1.000 0.000
#&gt; SRR1812715     2  0.6302      0.588 0.480 0.520 0.000
#&gt; SRR1812714     2  0.6633      0.609 0.444 0.548 0.008
#&gt; SRR1812719     3  0.4409      0.281 0.004 0.172 0.824
#&gt; SRR1812713     2  0.2443      0.725 0.032 0.940 0.028
#&gt; SRR1812712     2  0.1163      0.737 0.028 0.972 0.000
#&gt; SRR1812711     2  0.6468      0.612 0.444 0.552 0.004
#&gt; SRR1812710     2  0.6302      0.588 0.480 0.520 0.000
#&gt; SRR1812709     2  0.0892      0.735 0.020 0.980 0.000
#&gt; SRR1812708     1  0.8929      0.583 0.460 0.124 0.416
#&gt; SRR1812707     2  0.6302      0.588 0.480 0.520 0.000
#&gt; SRR1812705     2  0.6235      0.618 0.436 0.564 0.000
#&gt; SRR1812706     2  0.1267      0.719 0.004 0.972 0.024
#&gt; SRR1812704     2  0.0000      0.728 0.000 1.000 0.000
#&gt; SRR1812703     2  0.4682      0.721 0.192 0.804 0.004
#&gt; SRR1812702     2  0.2187      0.728 0.028 0.948 0.024
#&gt; SRR1812741     3  0.8768      0.314 0.112 0.408 0.480
#&gt; SRR1812740     3  0.3686      0.302 0.000 0.140 0.860
#&gt; SRR1812739     2  0.1289      0.737 0.032 0.968 0.000
#&gt; SRR1812738     2  0.0424      0.724 0.008 0.992 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-2-a').click(function(){
  $('#tab-ATC-mclust-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-3'>
<p><a id='tab-ATC-mclust-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.4134      0.649 0.740 0.000 0.260 0.000
#&gt; SRR1812753     1  0.4134      0.649 0.740 0.000 0.260 0.000
#&gt; SRR1812751     1  0.4059      0.827 0.788 0.200 0.012 0.000
#&gt; SRR1812750     1  0.4356      0.825 0.780 0.200 0.016 0.004
#&gt; SRR1812748     4  0.6112      0.427 0.248 0.000 0.096 0.656
#&gt; SRR1812749     1  0.4114      0.827 0.788 0.200 0.008 0.004
#&gt; SRR1812746     3  0.6796      0.590 0.252 0.000 0.596 0.152
#&gt; SRR1812745     2  0.8637      0.260 0.208 0.492 0.068 0.232
#&gt; SRR1812747     2  0.1661      0.814 0.004 0.944 0.000 0.052
#&gt; SRR1812744     4  0.4336      0.756 0.000 0.128 0.060 0.812
#&gt; SRR1812743     3  0.1474      0.797 0.000 0.000 0.948 0.052
#&gt; SRR1812742     3  0.1474      0.797 0.000 0.000 0.948 0.052
#&gt; SRR1812737     2  0.2457      0.749 0.008 0.912 0.004 0.076
#&gt; SRR1812735     2  0.2125      0.757 0.000 0.920 0.004 0.076
#&gt; SRR1812734     4  0.7479      0.102 0.252 0.000 0.244 0.504
#&gt; SRR1812733     2  0.5711      0.605 0.012 0.656 0.028 0.304
#&gt; SRR1812736     4  0.6112      0.427 0.248 0.000 0.096 0.656
#&gt; SRR1812732     4  0.3610      0.753 0.000 0.200 0.000 0.800
#&gt; SRR1812730     4  0.4631      0.754 0.008 0.144 0.048 0.800
#&gt; SRR1812731     4  0.3982      0.737 0.000 0.220 0.004 0.776
#&gt; SRR1812729     2  0.1576      0.814 0.004 0.948 0.000 0.048
#&gt; SRR1812727     3  0.4072      0.770 0.052 0.000 0.828 0.120
#&gt; SRR1812726     2  0.1356      0.812 0.008 0.960 0.000 0.032
#&gt; SRR1812728     4  0.4746      0.688 0.004 0.064 0.140 0.792
#&gt; SRR1812724     4  0.4701      0.760 0.000 0.164 0.056 0.780
#&gt; SRR1812725     2  0.4744      0.652 0.012 0.704 0.000 0.284
#&gt; SRR1812723     2  0.1118      0.814 0.000 0.964 0.000 0.036
#&gt; SRR1812722     2  0.0817      0.793 0.000 0.976 0.000 0.024
#&gt; SRR1812721     3  0.1792      0.797 0.000 0.000 0.932 0.068
#&gt; SRR1812718     2  0.1824      0.812 0.004 0.936 0.000 0.060
#&gt; SRR1812717     2  0.2654      0.797 0.000 0.888 0.004 0.108
#&gt; SRR1812716     2  0.5337      0.390 0.012 0.564 0.000 0.424
#&gt; SRR1812715     2  0.2125      0.757 0.000 0.920 0.004 0.076
#&gt; SRR1812714     2  0.1968      0.812 0.008 0.940 0.008 0.044
#&gt; SRR1812719     3  0.5673      0.603 0.052 0.000 0.660 0.288
#&gt; SRR1812713     2  0.5392      0.664 0.016 0.708 0.024 0.252
#&gt; SRR1812712     2  0.4584      0.640 0.004 0.696 0.000 0.300
#&gt; SRR1812711     2  0.1356      0.812 0.008 0.960 0.000 0.032
#&gt; SRR1812710     2  0.1978      0.763 0.000 0.928 0.004 0.068
#&gt; SRR1812709     4  0.3801      0.736 0.000 0.220 0.000 0.780
#&gt; SRR1812708     1  0.6083      0.767 0.708 0.204 0.048 0.040
#&gt; SRR1812707     2  0.2197      0.757 0.000 0.916 0.004 0.080
#&gt; SRR1812705     2  0.1824      0.801 0.000 0.936 0.004 0.060
#&gt; SRR1812706     4  0.3626      0.762 0.000 0.184 0.004 0.812
#&gt; SRR1812704     4  0.4079      0.765 0.000 0.180 0.020 0.800
#&gt; SRR1812703     2  0.3047      0.791 0.012 0.872 0.000 0.116
#&gt; SRR1812702     2  0.5233      0.597 0.020 0.648 0.000 0.332
#&gt; SRR1812741     3  0.2101      0.796 0.012 0.000 0.928 0.060
#&gt; SRR1812740     4  0.6112      0.427 0.248 0.000 0.096 0.656
#&gt; SRR1812739     2  0.4535      0.650 0.004 0.704 0.000 0.292
#&gt; SRR1812738     4  0.4415      0.759 0.000 0.140 0.056 0.804
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-3-a').click(function(){
  $('#tab-ATC-mclust-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-4'>
<p><a id='tab-ATC-mclust-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.4465     0.4715 0.736 0.000 0.060 0.204 0.000
#&gt; SRR1812753     1  0.4624     0.4919 0.740 0.000 0.096 0.164 0.000
#&gt; SRR1812751     1  0.4779     0.7190 0.716 0.200 0.084 0.000 0.000
#&gt; SRR1812750     1  0.5086     0.7144 0.700 0.200 0.096 0.004 0.000
#&gt; SRR1812748     3  0.0963     0.7209 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1812749     1  0.4617     0.7160 0.736 0.196 0.064 0.000 0.004
#&gt; SRR1812746     3  0.0404     0.7088 0.012 0.000 0.988 0.000 0.000
#&gt; SRR1812745     3  0.4866     0.3792 0.028 0.000 0.580 0.000 0.392
#&gt; SRR1812747     2  0.4989     0.6546 0.056 0.648 0.000 0.000 0.296
#&gt; SRR1812744     5  0.0771     0.5636 0.000 0.000 0.004 0.020 0.976
#&gt; SRR1812743     4  0.3003     0.6978 0.188 0.000 0.000 0.812 0.000
#&gt; SRR1812742     4  0.3003     0.6978 0.188 0.000 0.000 0.812 0.000
#&gt; SRR1812737     2  0.0324     0.6203 0.004 0.992 0.000 0.000 0.004
#&gt; SRR1812735     2  0.0324     0.6203 0.004 0.992 0.000 0.000 0.004
#&gt; SRR1812734     3  0.0162     0.7087 0.004 0.000 0.996 0.000 0.000
#&gt; SRR1812733     5  0.6001    -0.0959 0.068 0.400 0.004 0.012 0.516
#&gt; SRR1812736     3  0.0963     0.7209 0.000 0.000 0.964 0.000 0.036
#&gt; SRR1812732     5  0.1270     0.5658 0.000 0.052 0.000 0.000 0.948
#&gt; SRR1812730     5  0.2833     0.4074 0.004 0.004 0.140 0.000 0.852
#&gt; SRR1812731     5  0.1774     0.5674 0.000 0.052 0.000 0.016 0.932
#&gt; SRR1812729     2  0.5142     0.6146 0.052 0.600 0.000 0.000 0.348
#&gt; SRR1812727     3  0.5838     0.1505 0.008 0.000 0.496 0.424 0.072
#&gt; SRR1812726     2  0.4240     0.6812 0.008 0.684 0.004 0.000 0.304
#&gt; SRR1812728     5  0.4876     0.4603 0.004 0.040 0.012 0.232 0.712
#&gt; SRR1812724     5  0.2673     0.5698 0.000 0.044 0.004 0.060 0.892
#&gt; SRR1812725     5  0.5425    -0.1233 0.060 0.420 0.000 0.000 0.520
#&gt; SRR1812723     2  0.5080     0.6197 0.048 0.604 0.000 0.000 0.348
#&gt; SRR1812722     2  0.3395     0.6942 0.000 0.764 0.000 0.000 0.236
#&gt; SRR1812721     4  0.0000     0.6701 0.000 0.000 0.000 1.000 0.000
#&gt; SRR1812718     2  0.5328     0.5973 0.064 0.584 0.000 0.000 0.352
#&gt; SRR1812717     2  0.3913     0.6720 0.000 0.676 0.000 0.000 0.324
#&gt; SRR1812716     5  0.3051     0.4914 0.060 0.076 0.000 0.000 0.864
#&gt; SRR1812715     2  0.0324     0.6203 0.004 0.992 0.000 0.000 0.004
#&gt; SRR1812714     2  0.4858     0.6899 0.052 0.688 0.004 0.000 0.256
#&gt; SRR1812719     3  0.6016     0.1629 0.012 0.000 0.496 0.412 0.080
#&gt; SRR1812713     5  0.5425    -0.1330 0.060 0.420 0.000 0.000 0.520
#&gt; SRR1812712     5  0.5467    -0.1025 0.064 0.412 0.000 0.000 0.524
#&gt; SRR1812711     2  0.4858     0.6899 0.052 0.688 0.004 0.000 0.256
#&gt; SRR1812710     2  0.0324     0.6203 0.004 0.992 0.000 0.000 0.004
#&gt; SRR1812709     5  0.3936     0.5466 0.004 0.052 0.000 0.144 0.800
#&gt; SRR1812708     1  0.5848     0.5191 0.644 0.012 0.160 0.000 0.184
#&gt; SRR1812707     2  0.1357     0.6220 0.004 0.948 0.000 0.000 0.048
#&gt; SRR1812705     2  0.3816     0.6846 0.000 0.696 0.000 0.000 0.304
#&gt; SRR1812706     5  0.3595     0.5160 0.004 0.008 0.004 0.188 0.796
#&gt; SRR1812704     5  0.3994     0.5435 0.004 0.044 0.004 0.148 0.800
#&gt; SRR1812703     5  0.5579    -0.1422 0.072 0.420 0.000 0.000 0.508
#&gt; SRR1812702     5  0.5499    -0.0871 0.068 0.400 0.000 0.000 0.532
#&gt; SRR1812741     4  0.4047     0.1975 0.004 0.000 0.320 0.676 0.000
#&gt; SRR1812740     3  0.1043     0.7194 0.000 0.000 0.960 0.000 0.040
#&gt; SRR1812739     5  0.5420    -0.1171 0.060 0.416 0.000 0.000 0.524
#&gt; SRR1812738     5  0.4195     0.5316 0.004 0.044 0.004 0.168 0.780
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-4-a').click(function(){
  $('#tab-ATC-mclust-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-mclust-get-classes-5'>
<p><a id='tab-ATC-mclust-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     6  0.3390      0.654 0.296 0.000 0.000 0.000 0.000 0.704
#&gt; SRR1812753     6  0.3528      0.653 0.296 0.000 0.000 0.000 0.004 0.700
#&gt; SRR1812751     1  0.0858      0.889 0.968 0.028 0.000 0.004 0.000 0.000
#&gt; SRR1812750     1  0.0858      0.889 0.968 0.028 0.000 0.004 0.000 0.000
#&gt; SRR1812748     3  0.0000      0.845 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812749     1  0.0972      0.886 0.964 0.028 0.000 0.008 0.000 0.000
#&gt; SRR1812746     3  0.1155      0.839 0.036 0.004 0.956 0.000 0.004 0.000
#&gt; SRR1812745     5  0.5692      0.298 0.008 0.004 0.400 0.108 0.480 0.000
#&gt; SRR1812747     2  0.5422      0.549 0.160 0.564 0.000 0.276 0.000 0.000
#&gt; SRR1812744     5  0.2838      0.750 0.000 0.004 0.000 0.188 0.808 0.000
#&gt; SRR1812743     6  0.0000      0.726 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812742     6  0.0000      0.726 0.000 0.000 0.000 0.000 0.000 1.000
#&gt; SRR1812737     2  0.2872      0.747 0.140 0.836 0.000 0.024 0.000 0.000
#&gt; SRR1812735     2  0.3062      0.738 0.160 0.816 0.000 0.024 0.000 0.000
#&gt; SRR1812734     3  0.0858      0.841 0.028 0.004 0.968 0.000 0.000 0.000
#&gt; SRR1812733     4  0.1908      0.836 0.000 0.096 0.004 0.900 0.000 0.000
#&gt; SRR1812736     3  0.0000      0.845 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812732     5  0.3583      0.734 0.004 0.008 0.000 0.260 0.728 0.000
#&gt; SRR1812730     5  0.5764      0.594 0.008 0.004 0.196 0.220 0.572 0.000
#&gt; SRR1812731     5  0.4616      0.735 0.016 0.120 0.000 0.124 0.736 0.004
#&gt; SRR1812729     2  0.2768      0.695 0.012 0.832 0.000 0.156 0.000 0.000
#&gt; SRR1812727     3  0.5571      0.472 0.220 0.000 0.552 0.000 0.228 0.000
#&gt; SRR1812726     2  0.1268      0.754 0.008 0.952 0.000 0.036 0.004 0.000
#&gt; SRR1812728     5  0.0436      0.791 0.004 0.004 0.004 0.000 0.988 0.000
#&gt; SRR1812724     5  0.2706      0.788 0.000 0.008 0.000 0.160 0.832 0.000
#&gt; SRR1812725     4  0.1387      0.848 0.000 0.068 0.000 0.932 0.000 0.000
#&gt; SRR1812723     2  0.1802      0.745 0.012 0.916 0.000 0.072 0.000 0.000
#&gt; SRR1812722     2  0.2896      0.742 0.160 0.824 0.000 0.016 0.000 0.000
#&gt; SRR1812721     6  0.2632      0.675 0.004 0.000 0.000 0.000 0.164 0.832
#&gt; SRR1812718     2  0.3895      0.522 0.016 0.696 0.000 0.284 0.004 0.000
#&gt; SRR1812717     2  0.3867      0.439 0.012 0.660 0.000 0.328 0.000 0.000
#&gt; SRR1812716     4  0.3547      0.263 0.000 0.004 0.000 0.696 0.300 0.000
#&gt; SRR1812715     2  0.2558      0.741 0.156 0.840 0.000 0.004 0.000 0.000
#&gt; SRR1812714     2  0.2629      0.761 0.040 0.868 0.000 0.092 0.000 0.000
#&gt; SRR1812719     3  0.4967      0.601 0.132 0.000 0.640 0.000 0.228 0.000
#&gt; SRR1812713     4  0.2300      0.810 0.000 0.144 0.000 0.856 0.000 0.000
#&gt; SRR1812712     4  0.2231      0.841 0.004 0.068 0.000 0.900 0.028 0.000
#&gt; SRR1812711     2  0.3297      0.765 0.112 0.820 0.000 0.068 0.000 0.000
#&gt; SRR1812710     2  0.2558      0.742 0.156 0.840 0.000 0.004 0.000 0.000
#&gt; SRR1812709     5  0.1897      0.806 0.004 0.004 0.000 0.084 0.908 0.000
#&gt; SRR1812708     1  0.3264      0.669 0.796 0.184 0.000 0.012 0.008 0.000
#&gt; SRR1812707     2  0.0858      0.755 0.004 0.968 0.000 0.028 0.000 0.000
#&gt; SRR1812705     2  0.3512      0.511 0.008 0.720 0.000 0.272 0.000 0.000
#&gt; SRR1812706     5  0.1457      0.802 0.004 0.004 0.016 0.028 0.948 0.000
#&gt; SRR1812704     5  0.1897      0.806 0.004 0.004 0.000 0.084 0.908 0.000
#&gt; SRR1812703     2  0.3997      0.042 0.004 0.508 0.000 0.488 0.000 0.000
#&gt; SRR1812702     4  0.0603      0.819 0.000 0.016 0.004 0.980 0.000 0.000
#&gt; SRR1812741     6  0.6195      0.583 0.224 0.000 0.052 0.000 0.164 0.560
#&gt; SRR1812740     3  0.0000      0.845 0.000 0.000 1.000 0.000 0.000 0.000
#&gt; SRR1812739     4  0.2743      0.780 0.008 0.164 0.000 0.828 0.000 0.000
#&gt; SRR1812738     5  0.0777      0.804 0.000 0.004 0.000 0.024 0.972 0.000
</code></pre>

<script>
$('#tab-ATC-mclust-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-mclust-get-classes-5-a').click(function(){
  $('#tab-ATC-mclust-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-mclust-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-mclust-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-mclust-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-mclust-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-mclust-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-mclust-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-mclust-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-mclust-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-mclust-signature_compare](figure_cola/ATC-mclust-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-mclust-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-mclust-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-mclust-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-mclust-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-mclust-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-mclust-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-mclust-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-mclust-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-mclust-collect-classes](figure_cola/ATC-mclust-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

---------------------------------------------------




### ATC:NMF*






The object with results only for a single top-value method and a single partition method 
can be extracted as:

```r
res = res_list["ATC", "NMF"]
# you can also extract it by
# res = res_list["ATC:NMF"]
```

A summary of `res` and all the functions that can be applied to it:

```r
res
```

```
#> A 'ConsensusPartition' object with k = 2, 3, 4, 5, 6.
#>   On a matrix with 14626 rows and 51 columns.
#>   Top rows (1000, 2000, 3000, 4000, 5000) are extracted by 'ATC' method.
#>   Subgroups are detected by 'NMF' method.
#>   Performed in total 1250 partitions by row resampling.
#>   Best k for subgroups seems to be 2.
#> 
#> Following methods can be applied to this 'ConsensusPartition' object:
#>  [1] "cola_report"             "collect_classes"         "collect_plots"          
#>  [4] "collect_stats"           "colnames"                "compare_signatures"     
#>  [7] "consensus_heatmap"       "dimension_reduction"     "functional_enrichment"  
#> [10] "get_anno_col"            "get_anno"                "get_classes"            
#> [13] "get_consensus"           "get_matrix"              "get_membership"         
#> [16] "get_param"               "get_signatures"          "get_stats"              
#> [19] "is_best_k"               "is_stable_k"             "membership_heatmap"     
#> [22] "ncol"                    "nrow"                    "plot_ecdf"              
#> [25] "rownames"                "select_partition_number" "show"                   
#> [28] "suggest_best_k"          "test_to_known_factors"
```

`collect_plots()` function collects all the plots made from `res` for all `k` (number of partitions)
into one single page to provide an easy and fast comparison between different `k`.

```r
collect_plots(res)
```

![plot of chunk ATC-NMF-collect-plots](figure_cola/ATC-NMF-collect-plots-1.png)

The plots are:

- The first row: a plot of the ECDF (Empirical cumulative distribution
  function) curves of the consensus matrix for each `k` and the heatmap of
  predicted classes for each `k`.
- The second row: heatmaps of the consensus matrix for each `k`.
- The third row: heatmaps of the membership matrix for each `k`.
- The fouth row: heatmaps of the signatures for each `k`.

All the plots in panels can be made by individual functions and they are
plotted later in this section.

`select_partition_number()` produces several plots showing different
statistics for choosing "optimized" `k`. There are following statistics:

- ECDF curves of the consensus matrix for each `k`;
- 1-PAC. [The PAC
  score](https://en.wikipedia.org/wiki/Consensus_clustering#Over-interpretation_potential_of_consensus_clustering)
  measures the proportion of the ambiguous subgrouping.
- Mean silhouette score.
- Concordance. The mean probability of fiting the consensus class ids in all
  partitions.
- Area increased. Denote $A_k$ as the area under the ECDF curve for current
  `k`, the area increased is defined as $A_k - A_{k-1}$.
- Rand index. The percent of pairs of samples that are both in a same cluster
  or both are not in a same cluster in the partition of k and k-1.
- Jaccard index. The ratio of pairs of samples are both in a same cluster in
  the partition of k and k-1 and the pairs of samples are both in a same
  cluster in the partition k or k-1.

The detailed explanations of these statistics can be found in [the cola
vignette](http://bioconductor.org/packages/devel/bioc/vignettes/cola/inst/doc/cola.html#toc_13).

Generally speaking, lower PAC score, higher mean silhouette score or higher
concordance corresponds to better partition. Rand index and Jaccard index
measure how similar the current partition is compared to partition with `k-1`.
If they are too similar, we won't accept `k` is better than `k-1`.

```r
select_partition_number(res)
```

![plot of chunk ATC-NMF-select-partition-number](figure_cola/ATC-NMF-select-partition-number-1.png)

The numeric values for all these statistics can be obtained by `get_stats()`.

```r
get_stats(res)
```

```
#>   k 1-PAC mean_silhouette concordance area_increased  Rand Jaccard
#> 2 2 0.910           0.903       0.960         0.4034 0.594   0.594
#> 3 3 0.571           0.817       0.901         0.4672 0.802   0.669
#> 4 4 0.531           0.642       0.807         0.1946 0.685   0.367
#> 5 5 0.682           0.681       0.826         0.1021 0.841   0.508
#> 6 6 0.658           0.529       0.772         0.0441 0.875   0.531
```

`suggest_best_k()` suggests the best $k$ based on these statistics. The rules are as follows:

- All $k$ with Jaccard index larger than 0.95 are removed because the increase of
  the partition number does not provides enough extra information. If all $k$ are removed,
  the best $k$ is assigned by `NA`.
- For $k$ with 1-PAC larger than 0.9, the maximal $k$ is taken as the "best k". Other $k$ is called "optional k".
- If it does not fit the second rule. The $k$ with the highest vote of highest
  1-PAC, mean silhouette and concordance is taken as the "best k".

```r
suggest_best_k(res)
```

```
#> [1] 2
```


Following shows the table of the partitions (You need to click the **show/hide
code output** link to see it). The membership matrix (columns with name `p*`)
is inferred by
[`clue::cl_consensus()`](https://www.rdocumentation.org/link/cl_consensus?package=clue)
function with the `SE` method. Basically the value in the membership matrix
represents the probability to belong to a certain group. The finall class
label for an item is determined with the group with highest probability it
belongs to.

In `get_classes()` function, the entropy is calculated from the membership
matrix and the silhouette score is calculated from the consensus matrix.



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-classes' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-classes'>
<ul>
<li><a href='#tab-ATC-NMF-get-classes-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-classes-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-classes-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-classes-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-classes-5'>k = 6</a></li>
</ul>

<div id='tab-ATC-NMF-get-classes-1'>
<p><a id='tab-ATC-NMF-get-classes-1-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 2), get_membership(res, k = 2))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2
#&gt; SRR1812752     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812753     2  0.0672     0.9612 0.008 0.992
#&gt; SRR1812751     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812750     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812748     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812749     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812746     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812745     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812747     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812744     1  0.9988     0.0848 0.520 0.480
#&gt; SRR1812743     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812742     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812737     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812735     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812734     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812733     1  0.4161     0.8793 0.916 0.084
#&gt; SRR1812736     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812732     2  0.8763     0.5582 0.296 0.704
#&gt; SRR1812730     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812731     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812729     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812727     1  0.6973     0.7732 0.812 0.188
#&gt; SRR1812726     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812728     2  0.3879     0.9045 0.076 0.924
#&gt; SRR1812724     2  0.4562     0.8826 0.096 0.904
#&gt; SRR1812725     2  0.3733     0.9086 0.072 0.928
#&gt; SRR1812723     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812722     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812721     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812718     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812717     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812716     1  0.6048     0.8216 0.852 0.148
#&gt; SRR1812715     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812714     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812719     1  0.1414     0.9160 0.980 0.020
#&gt; SRR1812713     2  0.2948     0.9277 0.052 0.948
#&gt; SRR1812712     2  0.2043     0.9443 0.032 0.968
#&gt; SRR1812711     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812710     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812709     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812708     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812707     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812705     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812706     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812704     2  0.1843     0.9474 0.028 0.972
#&gt; SRR1812703     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812702     1  0.2423     0.9073 0.960 0.040
#&gt; SRR1812741     2  0.0000     0.9667 0.000 1.000
#&gt; SRR1812740     1  0.0000     0.9214 1.000 0.000
#&gt; SRR1812739     2  0.0672     0.9618 0.008 0.992
#&gt; SRR1812738     2  0.9833     0.2068 0.424 0.576
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-1-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-1-a').click(function(){
  $('#tab-ATC-NMF-get-classes-1-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-2'>
<p><a id='tab-ATC-NMF-get-classes-2-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 3), get_membership(res, k = 3))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3
#&gt; SRR1812752     1  0.0000     0.8757 1.000 0.000 0.000
#&gt; SRR1812753     1  0.0000     0.8757 1.000 0.000 0.000
#&gt; SRR1812751     1  0.3192     0.8184 0.888 0.112 0.000
#&gt; SRR1812750     1  0.4062     0.7708 0.836 0.164 0.000
#&gt; SRR1812748     3  0.0000     0.8592 0.000 0.000 1.000
#&gt; SRR1812749     1  0.5621     0.4993 0.692 0.308 0.000
#&gt; SRR1812746     3  0.0424     0.8625 0.000 0.008 0.992
#&gt; SRR1812745     3  0.0424     0.8625 0.000 0.008 0.992
#&gt; SRR1812747     2  0.0424     0.8714 0.008 0.992 0.000
#&gt; SRR1812744     3  0.6062     0.5431 0.000 0.384 0.616
#&gt; SRR1812743     1  0.0000     0.8757 1.000 0.000 0.000
#&gt; SRR1812742     1  0.0000     0.8757 1.000 0.000 0.000
#&gt; SRR1812737     2  0.3816     0.8504 0.148 0.852 0.000
#&gt; SRR1812735     2  0.3816     0.8504 0.148 0.852 0.000
#&gt; SRR1812734     3  0.0000     0.8592 0.000 0.000 1.000
#&gt; SRR1812733     3  0.4235     0.8518 0.000 0.176 0.824
#&gt; SRR1812736     3  0.0000     0.8592 0.000 0.000 1.000
#&gt; SRR1812732     2  0.9013     0.4246 0.152 0.524 0.324
#&gt; SRR1812730     3  0.3816     0.8701 0.000 0.148 0.852
#&gt; SRR1812731     2  0.4235     0.8311 0.176 0.824 0.000
#&gt; SRR1812729     2  0.0237     0.8711 0.004 0.996 0.000
#&gt; SRR1812727     1  0.3816     0.7199 0.852 0.000 0.148
#&gt; SRR1812726     2  0.3752     0.8525 0.144 0.856 0.000
#&gt; SRR1812728     2  0.2711     0.8009 0.000 0.912 0.088
#&gt; SRR1812724     2  0.1529     0.8451 0.000 0.960 0.040
#&gt; SRR1812725     2  0.0237     0.8689 0.000 0.996 0.004
#&gt; SRR1812723     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812722     2  0.3816     0.8504 0.148 0.852 0.000
#&gt; SRR1812721     2  0.4178     0.8327 0.172 0.828 0.000
#&gt; SRR1812718     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812717     2  0.3412     0.8598 0.124 0.876 0.000
#&gt; SRR1812716     3  0.4555     0.8281 0.000 0.200 0.800
#&gt; SRR1812715     2  0.3816     0.8504 0.148 0.852 0.000
#&gt; SRR1812714     2  0.3752     0.8525 0.144 0.856 0.000
#&gt; SRR1812719     3  0.3752     0.8709 0.000 0.144 0.856
#&gt; SRR1812713     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812712     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812711     2  0.3412     0.8598 0.124 0.876 0.000
#&gt; SRR1812710     2  0.3816     0.8504 0.148 0.852 0.000
#&gt; SRR1812709     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812708     2  0.0747     0.8651 0.016 0.984 0.000
#&gt; SRR1812707     2  0.3816     0.8504 0.148 0.852 0.000
#&gt; SRR1812705     2  0.3551     0.8572 0.132 0.868 0.000
#&gt; SRR1812706     3  0.3816     0.8701 0.000 0.148 0.852
#&gt; SRR1812704     2  0.0237     0.8689 0.000 0.996 0.004
#&gt; SRR1812703     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812702     3  0.3879     0.8685 0.000 0.152 0.848
#&gt; SRR1812741     1  0.0000     0.8757 1.000 0.000 0.000
#&gt; SRR1812740     3  0.0000     0.8592 0.000 0.000 1.000
#&gt; SRR1812739     2  0.0000     0.8704 0.000 1.000 0.000
#&gt; SRR1812738     2  0.6168     0.0662 0.000 0.588 0.412
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-2-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-2-a').click(function(){
  $('#tab-ATC-NMF-get-classes-2-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-3'>
<p><a id='tab-ATC-NMF-get-classes-3-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 4), get_membership(res, k = 4))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4
#&gt; SRR1812752     1  0.1398     0.7661 0.956 0.040 0.004 0.000
#&gt; SRR1812753     1  0.1305     0.7654 0.960 0.036 0.004 0.000
#&gt; SRR1812751     1  0.3710     0.7438 0.804 0.192 0.000 0.004
#&gt; SRR1812750     1  0.4343     0.6850 0.732 0.264 0.000 0.004
#&gt; SRR1812748     3  0.3486     0.7827 0.000 0.000 0.812 0.188
#&gt; SRR1812749     1  0.3710     0.7438 0.804 0.192 0.000 0.004
#&gt; SRR1812746     3  0.4855     0.6278 0.000 0.000 0.600 0.400
#&gt; SRR1812745     3  0.4624     0.7082 0.000 0.000 0.660 0.340
#&gt; SRR1812747     2  0.1637     0.7863 0.000 0.940 0.000 0.060
#&gt; SRR1812744     4  0.5793     0.0847 0.012 0.036 0.288 0.664
#&gt; SRR1812743     2  0.7205     0.4002 0.172 0.532 0.296 0.000
#&gt; SRR1812742     2  0.7172     0.4048 0.168 0.536 0.296 0.000
#&gt; SRR1812737     2  0.0188     0.8145 0.004 0.996 0.000 0.000
#&gt; SRR1812735     2  0.0188     0.8146 0.000 0.996 0.004 0.000
#&gt; SRR1812734     3  0.3726     0.7810 0.000 0.000 0.788 0.212
#&gt; SRR1812733     4  0.0927     0.6846 0.000 0.016 0.008 0.976
#&gt; SRR1812736     3  0.3486     0.7827 0.000 0.000 0.812 0.188
#&gt; SRR1812732     3  0.6603    -0.2350 0.052 0.436 0.500 0.012
#&gt; SRR1812730     4  0.2345     0.5776 0.000 0.000 0.100 0.900
#&gt; SRR1812731     2  0.6066     0.5962 0.116 0.692 0.188 0.004
#&gt; SRR1812729     2  0.4509     0.3937 0.004 0.708 0.000 0.288
#&gt; SRR1812727     1  0.1109     0.7499 0.968 0.004 0.028 0.000
#&gt; SRR1812726     2  0.0779     0.8135 0.016 0.980 0.000 0.004
#&gt; SRR1812728     4  0.4307     0.7712 0.024 0.192 0.000 0.784
#&gt; SRR1812724     2  0.5700     0.5870 0.036 0.724 0.032 0.208
#&gt; SRR1812725     4  0.3569     0.7739 0.000 0.196 0.000 0.804
#&gt; SRR1812723     2  0.5165    -0.3130 0.004 0.512 0.000 0.484
#&gt; SRR1812722     2  0.0524     0.8149 0.004 0.988 0.000 0.008
#&gt; SRR1812721     2  0.6014     0.6073 0.112 0.696 0.188 0.004
#&gt; SRR1812718     4  0.4800     0.6433 0.004 0.340 0.000 0.656
#&gt; SRR1812717     2  0.1743     0.7888 0.004 0.940 0.000 0.056
#&gt; SRR1812716     4  0.1936     0.6764 0.000 0.028 0.032 0.940
#&gt; SRR1812715     2  0.0707     0.8080 0.000 0.980 0.020 0.000
#&gt; SRR1812714     2  0.0895     0.8129 0.004 0.976 0.000 0.020
#&gt; SRR1812719     1  0.5756     0.3667 0.592 0.000 0.036 0.372
#&gt; SRR1812713     4  0.3569     0.7739 0.000 0.196 0.000 0.804
#&gt; SRR1812712     4  0.3569     0.7739 0.000 0.196 0.000 0.804
#&gt; SRR1812711     2  0.0779     0.8126 0.004 0.980 0.000 0.016
#&gt; SRR1812710     2  0.0188     0.8145 0.004 0.996 0.000 0.000
#&gt; SRR1812709     4  0.5376     0.5111 0.016 0.396 0.000 0.588
#&gt; SRR1812708     1  0.7351     0.2826 0.524 0.212 0.000 0.264
#&gt; SRR1812707     2  0.0000     0.8151 0.000 1.000 0.000 0.000
#&gt; SRR1812705     2  0.0817     0.8095 0.000 0.976 0.000 0.024
#&gt; SRR1812706     4  0.0336     0.6688 0.000 0.000 0.008 0.992
#&gt; SRR1812704     4  0.4175     0.7636 0.012 0.212 0.000 0.776
#&gt; SRR1812703     4  0.4454     0.6823 0.000 0.308 0.000 0.692
#&gt; SRR1812702     4  0.0524     0.6787 0.000 0.008 0.004 0.988
#&gt; SRR1812741     1  0.1182     0.7539 0.968 0.016 0.016 0.000
#&gt; SRR1812740     3  0.3486     0.7827 0.000 0.000 0.812 0.188
#&gt; SRR1812739     4  0.5320     0.4670 0.012 0.416 0.000 0.572
#&gt; SRR1812738     4  0.5241     0.7191 0.040 0.140 0.040 0.780
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-3-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-3-a').click(function(){
  $('#tab-ATC-NMF-get-classes-3-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-4'>
<p><a id='tab-ATC-NMF-get-classes-4-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 5), get_membership(res, k = 5))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5
#&gt; SRR1812752     1  0.1168    0.70805 0.960 0.008 0.000 0.032 0.000
#&gt; SRR1812753     1  0.1082    0.70379 0.964 0.000 0.008 0.028 0.000
#&gt; SRR1812751     1  0.3074    0.66304 0.804 0.196 0.000 0.000 0.000
#&gt; SRR1812750     2  0.4437   -0.00448 0.464 0.532 0.000 0.004 0.000
#&gt; SRR1812748     3  0.0324    0.73161 0.004 0.000 0.992 0.004 0.000
#&gt; SRR1812749     1  0.3305    0.63800 0.776 0.224 0.000 0.000 0.000
#&gt; SRR1812746     3  0.4249    0.73958 0.000 0.000 0.688 0.016 0.296
#&gt; SRR1812745     3  0.3551    0.77933 0.000 0.000 0.772 0.008 0.220
#&gt; SRR1812747     2  0.1012    0.87388 0.000 0.968 0.000 0.020 0.012
#&gt; SRR1812744     5  0.5991    0.40919 0.008 0.000 0.172 0.204 0.616
#&gt; SRR1812743     4  0.5548    0.71043 0.056 0.096 0.132 0.716 0.000
#&gt; SRR1812742     4  0.5846    0.70237 0.060 0.156 0.096 0.688 0.000
#&gt; SRR1812737     2  0.0162    0.88063 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812735     2  0.0290    0.88066 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1812734     3  0.2843    0.78108 0.000 0.000 0.848 0.008 0.144
#&gt; SRR1812733     5  0.2813    0.61596 0.000 0.000 0.108 0.024 0.868
#&gt; SRR1812736     3  0.0290    0.73192 0.000 0.000 0.992 0.008 0.000
#&gt; SRR1812732     4  0.6147    0.58183 0.004 0.188 0.228 0.580 0.000
#&gt; SRR1812730     3  0.4238    0.66498 0.000 0.000 0.628 0.004 0.368
#&gt; SRR1812731     4  0.2932    0.74307 0.032 0.104 0.000 0.864 0.000
#&gt; SRR1812729     2  0.1830    0.84276 0.000 0.924 0.000 0.008 0.068
#&gt; SRR1812727     1  0.0898    0.69849 0.972 0.000 0.000 0.020 0.008
#&gt; SRR1812726     2  0.1341    0.84915 0.000 0.944 0.000 0.056 0.000
#&gt; SRR1812728     5  0.4563    0.69219 0.028 0.016 0.000 0.228 0.728
#&gt; SRR1812724     4  0.3777    0.62297 0.004 0.040 0.008 0.824 0.124
#&gt; SRR1812725     5  0.1377    0.72894 0.000 0.020 0.004 0.020 0.956
#&gt; SRR1812723     2  0.1800    0.85374 0.000 0.932 0.000 0.020 0.048
#&gt; SRR1812722     2  0.0566    0.87972 0.000 0.984 0.000 0.012 0.004
#&gt; SRR1812721     4  0.3110    0.71825 0.028 0.112 0.000 0.856 0.004
#&gt; SRR1812718     5  0.4873    0.44970 0.000 0.312 0.000 0.044 0.644
#&gt; SRR1812717     2  0.6036    0.33568 0.000 0.564 0.000 0.160 0.276
#&gt; SRR1812716     3  0.4764    0.53100 0.000 0.004 0.548 0.012 0.436
#&gt; SRR1812715     2  0.0290    0.88066 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1812714     2  0.0324    0.88001 0.004 0.992 0.000 0.004 0.000
#&gt; SRR1812719     5  0.5525    0.51004 0.288 0.000 0.000 0.100 0.612
#&gt; SRR1812713     5  0.0960    0.72091 0.000 0.004 0.016 0.008 0.972
#&gt; SRR1812712     5  0.0324    0.72618 0.000 0.000 0.004 0.004 0.992
#&gt; SRR1812711     2  0.0162    0.88051 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812710     2  0.0162    0.88063 0.000 0.996 0.000 0.004 0.000
#&gt; SRR1812709     5  0.5081    0.64483 0.020 0.032 0.000 0.284 0.664
#&gt; SRR1812708     1  0.5832   -0.05129 0.468 0.044 0.000 0.024 0.464
#&gt; SRR1812707     2  0.0290    0.88010 0.000 0.992 0.000 0.008 0.000
#&gt; SRR1812705     2  0.0865    0.87618 0.000 0.972 0.000 0.024 0.004
#&gt; SRR1812706     5  0.0451    0.72322 0.000 0.000 0.008 0.004 0.988
#&gt; SRR1812704     5  0.4605    0.68858 0.020 0.016 0.004 0.236 0.724
#&gt; SRR1812703     2  0.4774    0.56961 0.000 0.684 0.012 0.028 0.276
#&gt; SRR1812702     5  0.0807    0.71668 0.000 0.000 0.012 0.012 0.976
#&gt; SRR1812741     4  0.4047    0.49896 0.320 0.000 0.000 0.676 0.004
#&gt; SRR1812740     3  0.0000    0.73512 0.000 0.000 1.000 0.000 0.000
#&gt; SRR1812739     5  0.5230    0.63593 0.028 0.028 0.000 0.296 0.648
#&gt; SRR1812738     5  0.5227    0.60305 0.032 0.000 0.016 0.336 0.616
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-4-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-4-a').click(function(){
  $('#tab-ATC-NMF-get-classes-4-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>

<div id='tab-ATC-NMF-get-classes-5'>
<p><a id='tab-ATC-NMF-get-classes-5-a' style='color:#0366d6' href='#'>show/hide code output</a></p>
<pre><code class="r">cbind(get_classes(res, k = 6), get_membership(res, k = 6))
</code></pre>

<pre><code>#&gt;            class entropy silhouette    p1    p2    p3    p4    p5    p6
#&gt; SRR1812752     1  0.0146     0.6626 0.996 0.000 0.000 0.000 0.000 0.004
#&gt; SRR1812753     1  0.0363     0.6602 0.988 0.000 0.000 0.000 0.000 0.012
#&gt; SRR1812751     1  0.3273     0.6068 0.776 0.212 0.000 0.004 0.008 0.000
#&gt; SRR1812750     2  0.4091     0.3352 0.340 0.644 0.000 0.004 0.008 0.004
#&gt; SRR1812748     3  0.1049     0.7250 0.000 0.000 0.960 0.000 0.008 0.032
#&gt; SRR1812749     1  0.4105     0.4301 0.640 0.344 0.000 0.004 0.008 0.004
#&gt; SRR1812746     5  0.4749     0.1665 0.000 0.004 0.384 0.036 0.572 0.004
#&gt; SRR1812745     5  0.4184    -0.0953 0.000 0.000 0.488 0.000 0.500 0.012
#&gt; SRR1812747     2  0.1223     0.8051 0.012 0.960 0.000 0.008 0.004 0.016
#&gt; SRR1812744     6  0.7528     0.2174 0.000 0.004 0.236 0.168 0.196 0.396
#&gt; SRR1812743     6  0.1078     0.6879 0.012 0.016 0.008 0.000 0.000 0.964
#&gt; SRR1812742     6  0.1875     0.6897 0.020 0.032 0.020 0.000 0.000 0.928
#&gt; SRR1812737     2  0.0993     0.8047 0.012 0.964 0.000 0.000 0.000 0.024
#&gt; SRR1812735     2  0.1053     0.8081 0.000 0.964 0.000 0.004 0.012 0.020
#&gt; SRR1812734     3  0.3518     0.4929 0.000 0.000 0.732 0.000 0.256 0.012
#&gt; SRR1812733     5  0.1321     0.6377 0.000 0.000 0.020 0.024 0.952 0.004
#&gt; SRR1812736     3  0.1552     0.7266 0.000 0.000 0.940 0.004 0.020 0.036
#&gt; SRR1812732     6  0.3592     0.5890 0.000 0.020 0.240 0.000 0.000 0.740
#&gt; SRR1812730     3  0.4654     0.1688 0.000 0.012 0.564 0.008 0.404 0.012
#&gt; SRR1812731     6  0.2558     0.5740 0.004 0.000 0.000 0.156 0.000 0.840
#&gt; SRR1812729     2  0.3539     0.6926 0.000 0.768 0.000 0.008 0.208 0.016
#&gt; SRR1812727     1  0.2488     0.5985 0.864 0.000 0.004 0.124 0.000 0.008
#&gt; SRR1812726     2  0.2455     0.7518 0.012 0.872 0.000 0.112 0.000 0.004
#&gt; SRR1812728     4  0.1814     0.6263 0.000 0.000 0.000 0.900 0.100 0.000
#&gt; SRR1812724     4  0.5846     0.4987 0.000 0.008 0.012 0.580 0.228 0.172
#&gt; SRR1812725     5  0.4062     0.5135 0.000 0.044 0.024 0.128 0.792 0.012
#&gt; SRR1812723     2  0.1390     0.8035 0.000 0.948 0.000 0.004 0.032 0.016
#&gt; SRR1812722     2  0.2765     0.7555 0.000 0.848 0.000 0.004 0.132 0.016
#&gt; SRR1812721     4  0.4218     0.2543 0.004 0.012 0.000 0.584 0.000 0.400
#&gt; SRR1812718     2  0.5675     0.4014 0.000 0.584 0.016 0.108 0.284 0.008
#&gt; SRR1812717     2  0.7361    -0.0742 0.000 0.348 0.000 0.256 0.284 0.112
#&gt; SRR1812716     5  0.5578    -0.1175 0.000 0.036 0.440 0.028 0.480 0.016
#&gt; SRR1812715     2  0.0993     0.8076 0.012 0.964 0.000 0.000 0.000 0.024
#&gt; SRR1812714     2  0.1406     0.7984 0.016 0.952 0.000 0.004 0.008 0.020
#&gt; SRR1812719     4  0.5977     0.1877 0.152 0.000 0.008 0.512 0.320 0.008
#&gt; SRR1812713     5  0.2289     0.6135 0.000 0.020 0.024 0.036 0.912 0.008
#&gt; SRR1812712     5  0.2445     0.6162 0.000 0.000 0.008 0.120 0.868 0.004
#&gt; SRR1812711     2  0.0508     0.8079 0.012 0.984 0.000 0.000 0.000 0.004
#&gt; SRR1812710     2  0.0622     0.8071 0.012 0.980 0.000 0.000 0.000 0.008
#&gt; SRR1812709     4  0.2001     0.6239 0.000 0.000 0.000 0.912 0.048 0.040
#&gt; SRR1812708     5  0.6945     0.0989 0.236 0.032 0.012 0.280 0.436 0.004
#&gt; SRR1812707     2  0.0603     0.8096 0.000 0.980 0.000 0.000 0.004 0.016
#&gt; SRR1812705     2  0.2288     0.7865 0.000 0.896 0.000 0.004 0.072 0.028
#&gt; SRR1812706     5  0.2182     0.6286 0.000 0.000 0.020 0.068 0.904 0.008
#&gt; SRR1812704     4  0.3584     0.5583 0.000 0.000 0.004 0.740 0.244 0.012
#&gt; SRR1812703     2  0.4265     0.4526 0.000 0.596 0.000 0.004 0.384 0.016
#&gt; SRR1812702     5  0.1843     0.6256 0.000 0.004 0.004 0.080 0.912 0.000
#&gt; SRR1812741     1  0.6041    -0.0813 0.400 0.000 0.000 0.344 0.000 0.256
#&gt; SRR1812740     3  0.1285     0.7308 0.000 0.000 0.944 0.000 0.052 0.004
#&gt; SRR1812739     4  0.6067     0.1001 0.000 0.004 0.000 0.456 0.264 0.276
#&gt; SRR1812738     4  0.3107     0.6075 0.000 0.000 0.000 0.832 0.052 0.116
</code></pre>

<script>
$('#tab-ATC-NMF-get-classes-5-a').parent().next().next().hide();
$('#tab-ATC-NMF-get-classes-5-a').click(function(){
  $('#tab-ATC-NMF-get-classes-5-a').parent().next().next().toggle();
  return(false);
});
</script>
</div>
</div>

Heatmaps for the consensus matrix. It visualizes the probability of two
samples to be in a same group.



<script>
$( function() {
	$( '#tabs-ATC-NMF-consensus-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-consensus-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-consensus-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-consensus-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-consensus-heatmap-1'>
<pre><code class="r">consensus_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-2'>
<pre><code class="r">consensus_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-3'>
<pre><code class="r">consensus_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-4'>
<pre><code class="r">consensus_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-consensus-heatmap-5'>
<pre><code class="r">consensus_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-consensus-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-consensus-heatmap-5"/></p>

</div>
</div>

Heatmaps for the membership of samples in all partitions to see how consistent they are:




<script>
$( function() {
	$( '#tabs-ATC-NMF-membership-heatmap' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-membership-heatmap'>
<ul>
<li><a href='#tab-ATC-NMF-membership-heatmap-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-membership-heatmap-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-membership-heatmap-1'>
<pre><code class="r">membership_heatmap(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-1-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-1"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-2'>
<pre><code class="r">membership_heatmap(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-2-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-2"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-3'>
<pre><code class="r">membership_heatmap(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-3-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-3"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-4'>
<pre><code class="r">membership_heatmap(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-4-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-4"/></p>

</div>
<div id='tab-ATC-NMF-membership-heatmap-5'>
<pre><code class="r">membership_heatmap(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-membership-heatmap-5-1.png" alt="plot of chunk tab-ATC-NMF-membership-heatmap-5"/></p>

</div>
</div>

As soon as we have had the classes for columns, we can look for signatures
which are significantly different between classes which can be candidate marks
for certain classes. Following are the heatmaps for signatures.



Signature heatmaps where rows are scaled:



<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-1'>
<pre><code class="r">get_signatures(res, k = 2)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-2'>
<pre><code class="r">get_signatures(res, k = 3)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-3'>
<pre><code class="r">get_signatures(res, k = 4)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-4'>
<pre><code class="r">get_signatures(res, k = 5)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-5'>
<pre><code class="r">get_signatures(res, k = 6)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-5"/></p>

</div>
</div>



Signature heatmaps where rows are not scaled:


<script>
$( function() {
	$( '#tabs-ATC-NMF-get-signatures-no-scale' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-get-signatures-no-scale'>
<ul>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-get-signatures-no-scale-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-get-signatures-no-scale-1'>
<pre><code class="r">get_signatures(res, k = 2, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-1-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-1"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-2'>
<pre><code class="r">get_signatures(res, k = 3, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-2-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-2"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-3'>
<pre><code class="r">get_signatures(res, k = 4, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-3-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-3"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-4'>
<pre><code class="r">get_signatures(res, k = 5, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-4-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-4"/></p>

</div>
<div id='tab-ATC-NMF-get-signatures-no-scale-5'>
<pre><code class="r">get_signatures(res, k = 6, scale_rows = FALSE)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-get-signatures-no-scale-5-1.png" alt="plot of chunk tab-ATC-NMF-get-signatures-no-scale-5"/></p>

</div>
</div>



Compare the overlap of signatures from different k:

```r
compare_signatures(res)
```

![plot of chunk ATC-NMF-signature_compare](figure_cola/ATC-NMF-signature_compare-1.png)

`get_signature()` returns a data frame invisibly. TO get the list of signatures, the function
call should be assigned to a variable explicitly. In following code, if `plot` argument is set
to `FALSE`, no heatmap is plotted while only the differential analysis is performed.

```r
# code only for demonstration
tb = get_signature(res, k = ..., plot = FALSE)
```

An example of the output of `tb` is:

```
#>   which_row         fdr    mean_1    mean_2 scaled_mean_1 scaled_mean_2 km
#> 1        38 0.042760348  8.373488  9.131774    -0.5533452     0.5164555  1
#> 2        40 0.018707592  7.106213  8.469186    -0.6173731     0.5762149  1
#> 3        55 0.019134737 10.221463 11.207825    -0.6159697     0.5749050  1
#> 4        59 0.006059896  5.921854  7.869574    -0.6899429     0.6439467  1
#> 5        60 0.018055526  8.928898 10.211722    -0.6204761     0.5791110  1
#> 6        98 0.009384629 15.714769 14.887706     0.6635654    -0.6193277  2
...
```

The columns in `tb` are:

1. `which_row`: row indices corresponding to the input matrix.
2. `fdr`: FDR for the differential test. 
3. `mean_x`: The mean value in group x.
4. `scaled_mean_x`: The mean value in group x after rows are scaled.
5. `km`: Row groups if k-means clustering is applied to rows.



UMAP plot which shows how samples are separated.


<script>
$( function() {
	$( '#tabs-ATC-NMF-dimension-reduction' ).tabs();
} );
</script>
<div id='tabs-ATC-NMF-dimension-reduction'>
<ul>
<li><a href='#tab-ATC-NMF-dimension-reduction-1'>k = 2</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-2'>k = 3</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-3'>k = 4</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-4'>k = 5</a></li>
<li><a href='#tab-ATC-NMF-dimension-reduction-5'>k = 6</a></li>
</ul>
<div id='tab-ATC-NMF-dimension-reduction-1'>
<pre><code class="r">dimension_reduction(res, k = 2, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-1-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-1"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-2'>
<pre><code class="r">dimension_reduction(res, k = 3, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-2-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-2"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-3'>
<pre><code class="r">dimension_reduction(res, k = 4, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-3-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-3"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-4'>
<pre><code class="r">dimension_reduction(res, k = 5, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-4-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-4"/></p>

</div>
<div id='tab-ATC-NMF-dimension-reduction-5'>
<pre><code class="r">dimension_reduction(res, k = 6, method = &quot;UMAP&quot;)
</code></pre>

<p><img src="figure_cola/tab-ATC-NMF-dimension-reduction-5-1.png" alt="plot of chunk tab-ATC-NMF-dimension-reduction-5"/></p>

</div>
</div>


Following heatmap shows how subgroups are split when increasing `k`:

```r
collect_classes(res)
```

![plot of chunk ATC-NMF-collect-classes](figure_cola/ATC-NMF-collect-classes-1.png)


If matrix rows can be associated to genes, consider to use `GO_Enrichment(res,
...)` to perform function enrichment for the signature genes.


 

## Session info


```r
sessionInfo()
```

```
#> R version 3.6.0 (2019-04-26)
#> Platform: x86_64-pc-linux-gnu (64-bit)
#> Running under: CentOS Linux 7 (Core)
#> 
#> Matrix products: default
#> BLAS:   /usr/lib64/libblas.so.3.4.2
#> LAPACK: /usr/lib64/liblapack.so.3.4.2
#> 
#> locale:
#>  [1] LC_CTYPE=en_GB.UTF-8       LC_NUMERIC=C               LC_TIME=en_GB.UTF-8       
#>  [4] LC_COLLATE=en_GB.UTF-8     LC_MONETARY=en_GB.UTF-8    LC_MESSAGES=en_GB.UTF-8   
#>  [7] LC_PAPER=en_GB.UTF-8       LC_NAME=C                  LC_ADDRESS=C              
#> [10] LC_TELEPHONE=C             LC_MEASUREMENT=en_GB.UTF-8 LC_IDENTIFICATION=C       
#> 
#> attached base packages:
#>  [1] grid      parallel  stats4    stats     graphics  grDevices utils     datasets  methods  
#> [10] base     
#> 
#> other attached packages:
#>  [1] genefilter_1.66.0           ComplexHeatmap_2.1.1        markdown_1.1               
#>  [4] knitr_1.26                  cola_1.3.2                  SummarizedExperiment_1.14.1
#>  [7] DelayedArray_0.10.0         BiocParallel_1.18.1         matrixStats_0.55.0         
#> [10] Biobase_2.44.0              GenomicRanges_1.36.1        GenomeInfoDb_1.20.0        
#> [13] IRanges_2.18.3              S4Vectors_0.22.1            BiocGenerics_0.30.0        
#> [16] GetoptLong_0.1.7           
#> 
#> loaded via a namespace (and not attached):
#>  [1] bitops_1.0-6           bit64_0.9-7            doParallel_1.0.15      RColorBrewer_1.1-2    
#>  [5] httr_1.4.1             backports_1.1.5        tools_3.6.0            R6_2.4.1              
#>  [9] DBI_1.0.0              lazyeval_0.2.2         colorspace_1.4-1       withr_2.1.2           
#> [13] tidyselect_0.2.5       gridExtra_2.3          bit_1.1-14             compiler_3.6.0        
#> [17] xml2_1.2.2             microbenchmark_1.4-7   pkgmaker_0.28          slam_0.1-46           
#> [21] scales_1.1.0           NMF_0.23.6             stringr_1.4.0          digest_0.6.23         
#> [25] XVector_0.24.0         pkgconfig_2.0.3        bibtex_0.4.2           highr_0.8             
#> [29] rlang_0.4.2            GlobalOptions_0.1.1    RSQLite_2.1.2          impute_1.58.0         
#> [33] shape_1.4.4            mclust_5.4.5           dendextend_1.12.0      dplyr_0.8.3           
#> [37] RCurl_1.95-4.12        magrittr_1.5           GenomeInfoDbData_1.2.1 Matrix_1.2-17         
#> [41] Rcpp_1.0.3             munsell_0.5.0          viridis_0.5.1          lifecycle_0.1.0       
#> [45] stringi_1.4.3          zlibbioc_1.30.0        plyr_1.8.4             blob_1.2.0            
#> [49] crayon_1.3.4           lattice_0.20-38        splines_3.6.0          annotate_1.62.0       
#> [53] circlize_0.4.9         zeallot_0.1.0          pillar_1.4.2           rjson_0.2.20          
#> [57] rngtools_1.4           reshape2_1.4.3         codetools_0.2-16       XML_3.98-1.20         
#> [61] glue_1.3.1             evaluate_0.14          vctrs_0.2.0            png_0.1-7             
#> [65] foreach_1.4.7          polyclip_1.10-0        gtable_0.3.0           purrr_0.3.3           
#> [69] clue_0.3-57            assertthat_0.2.1       ggplot2_3.2.1          xfun_0.11             
#> [73] gridBase_0.4-7         eulerr_6.0.0           xtable_1.8-4           skmeans_0.2-11        
#> [77] survival_2.44-1.1      viridisLite_0.3.0      tibble_2.1.3           iterators_1.0.12      
#> [81] memoise_1.1.0          AnnotationDbi_1.46.1   registry_0.5-1         GTF_0.0.1             
#> [85] cluster_2.1.0          brew_1.0-6
```


