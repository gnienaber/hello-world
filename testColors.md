---
title: "Nass TSA Netdown"
author: "G Nienaber"
date: "November 08, 2019"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
    number_sections: true
    theme: simplex
    highlight: tango
    df_print: paged
    code_folding: hide
---

[comment]: # Change the menu bar color and font size
[comment]: # code.r is code block text and pre is for output of knitr chunks
<style>
  .list-group-item.active, .list-group-item.active:focus, .list-group-item.active:hover {background-color: #4e9a06;}
  body, td {font-size: 14px;}
  code.r {font-size: 14px;}
  pre {font-size: 14px}
</style>

**Version Notes**

1.0 - 2019-11-08

* First Data Package Evaluation Sent

2.0 - 2019-10-06

* Added Maps



```r
#--------------------------
#Setup and Global Options
#--------------------------

#Code Chunk
knitr::opts_chunk$set(echo = TRUE)

#Libraries
library(DBI)
library(tidyr)
library(dplyr)
library(forcats)
library(ggplot2)
library(kableExtra)

#Postgres
db = dbConnect(RPostgreSQL::PostgreSQL(), host="localhost", user = "postgres")
knitr::opts_chunk$set(connection = "db")
knitr::opts_knit$set(sql.max.print = NA)

#ggplot default theme options
#knitr::opts_chunk$set(fig.height = 4, collapse = TRUE)
gg_theme <- theme_minimal() + theme(axis.text.x = element_text(angle = 30, hjust = 1))

#kable
options(knitr.kable.NA = '')

#kable default table
kable <- function(data) {
  knitr::kable(data, digits = 0, format.args = list(big.mark = ",")) %>% 
  kable_styling(bootstrap_options = "striped", full_width = TRUE)
}
```

This is an [R Markdown](http://rmarkdown.rstudio.com) Notebook. When you execute code within the notebook, the results appear beneath the code. 

# Level 1

Try executing this chunk by clicking the *Run* button within the chunk or by placing your cursor inside it and pressing *Ctrl+Shift+Enter*. 


```{.sql .fold-show}
select * from tsa43_vdyp_stats limit 50;
```


<div class="knitsql-table">
<table>
<caption>50 records</caption>
 <thead>
  <tr>
   <th style="text-align:right;"> feat_id </th>
   <th style="text-align:right;"> maxvol </th>
   <th style="text-align:right;"> age_maxvol </th>
   <th style="text-align:right;"> cmai </th>
   <th style="text-align:right;"> age_cmai </th>
   <th style="text-align:right;"> cmai95 </th>
   <th style="text-align:right;"> u_mai_age </th>
   <th style="text-align:right;"> u_mai </th>
   <th style="text-align:right;"> l_mai </th>
   <th style="text-align:right;"> age_cmai95 </th>
   <th style="text-align:right;"> vri_age </th>
   <th style="text-align:right;"> vri_vol </th>
   <th style="text-align:right;"> u_vol </th>
   <th style="text-align:right;"> l_vol </th>
   <th style="text-align:right;"> p_low </th>
   <th style="text-align:right;"> vdyp_vol </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:right;"> 3500292 </td>
   <td style="text-align:right;"> 190.9356 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 1.0383241 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 0.9864079 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1.0024969 </td>
   <td style="text-align:right;"> 0.9196640 </td>
   <td style="text-align:right;"> 158 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 191.880 </td>
   <td style="text-align:right;"> 190.9356 </td>
   <td style="text-align:right;"> 190.9356 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 190.9356 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500293 </td>
   <td style="text-align:right;"> 257.7901 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1.8041492 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 1.7139418 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 1.7776968 </td>
   <td style="text-align:right;"> 1.7057601 </td>
   <td style="text-align:right;"> 111 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 251.868 </td>
   <td style="text-align:right;"> 251.2796 </td>
   <td style="text-align:right;"> 252.5823 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 251.8007 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500294 </td>
   <td style="text-align:right;"> 334.7207 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 2.1798577 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 2.0708648 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 2.0784134 </td>
   <td style="text-align:right;"> 1.9373465 </td>
   <td style="text-align:right;"> 119 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 325.506 </td>
   <td style="text-align:right;"> 328.0294 </td>
   <td style="text-align:right;"> 328.0294 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 328.0294 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500295 </td>
   <td style="text-align:right;"> 449.0068 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 3.1854457 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 3.0261734 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 3.1031189 </td>
   <td style="text-align:right;"> 2.9363620 </td>
   <td style="text-align:right;"> 95 </td>
   <td style="text-align:right;"> 96 </td>
   <td style="text-align:right;"> 293.450 </td>
   <td style="text-align:right;"> 310.3119 </td>
   <td style="text-align:right;"> 264.2726 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 291.8962 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500296 </td>
   <td style="text-align:right;"> 211.8090 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 1.3456786 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 1.2783947 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 1.3180121 </td>
   <td style="text-align:right;"> 1.2440908 </td>
   <td style="text-align:right;"> 135 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 208.348 </td>
   <td style="text-align:right;"> 208.0649 </td>
   <td style="text-align:right;"> 208.9724 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 208.4279 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500297 </td>
   <td style="text-align:right;"> 344.3742 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 2.2927284 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 2.1780920 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 2.2510992 </td>
   <td style="text-align:right;"> 2.1443254 </td>
   <td style="text-align:right;"> 113 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 326.211 </td>
   <td style="text-align:right;"> 331.6806 </td>
   <td style="text-align:right;"> 331.6806 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 331.6806 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500298 </td>
   <td style="text-align:right;"> 309.7460 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 2.0654932 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 1.9622186 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 1.9711450 </td>
   <td style="text-align:right;"> 1.8325632 </td>
   <td style="text-align:right;"> 99 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 300.194 </td>
   <td style="text-align:right;"> 299.2947 </td>
   <td style="text-align:right;"> 301.8316 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 300.3095 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500299 </td>
   <td style="text-align:right;"> 418.4551 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 3.6749166 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> 3.4911707 </td>
   <td style="text-align:right;"> 70 </td>
   <td style="text-align:right;"> 3.5020171 </td>
   <td style="text-align:right;"> 3.0463732 </td>
   <td style="text-align:right;"> 70 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 400.397 </td>
   <td style="text-align:right;"> 398.7138 </td>
   <td style="text-align:right;"> 403.1810 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 400.5007 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3703518 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3708294 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 220 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3708345 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 80 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3708445 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3712467 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 190 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 190 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3719311 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5419705 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 73 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.7 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5424506 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 63 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.7 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5424630 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5428288 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 70 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 63 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.7 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5428463 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 73 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.7 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5438755 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 208 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.2 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5447543 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 190 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5450101 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 210 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5452688 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5452807 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 30 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5452928 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 240 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5455890 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5486732 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 14440127 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 240 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 1 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.9 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500302 </td>
   <td style="text-align:right;"> 227.9605 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 1.2504176 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 1.1878968 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1.2142394 </td>
   <td style="text-align:right;"> 1.1223507 </td>
   <td style="text-align:right;"> 157 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 230.614 </td>
   <td style="text-align:right;"> 227.9605 </td>
   <td style="text-align:right;"> 227.9605 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 227.9605 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500303 </td>
   <td style="text-align:right;"> 373.0354 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 2.5519715 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 2.4243729 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 2.4784355 </td>
   <td style="text-align:right;"> 2.3234030 </td>
   <td style="text-align:right;"> 107 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 360.322 </td>
   <td style="text-align:right;"> 363.4176 </td>
   <td style="text-align:right;"> 363.4176 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 363.4176 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500306 </td>
   <td style="text-align:right;"> 241.3779 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 1.6548436 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 1.5721014 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 1.5886758 </td>
   <td style="text-align:right;"> 1.4915865 </td>
   <td style="text-align:right;"> 118 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 235.939 </td>
   <td style="text-align:right;"> 235.5291 </td>
   <td style="text-align:right;"> 236.7894 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 236.0332 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500307 </td>
   <td style="text-align:right;"> 402.8894 </td>
   <td style="text-align:right;"> 190 </td>
   <td style="text-align:right;"> 2.7353285 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 2.5985621 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 2.7073850 </td>
   <td style="text-align:right;"> 2.5980219 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 96 </td>
   <td style="text-align:right;"> 222.448 </td>
   <td style="text-align:right;"> 240.8572 </td>
   <td style="text-align:right;"> 191.3237 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 221.0438 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500308 </td>
   <td style="text-align:right;"> 334.5872 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 2.2408177 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 2.1287768 </td>
   <td style="text-align:right;"> 120 </td>
   <td style="text-align:right;"> 2.2016050 </td>
   <td style="text-align:right;"> 2.1118492 </td>
   <td style="text-align:right;"> 112 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 334.563 </td>
   <td style="text-align:right;"> 334.5872 </td>
   <td style="text-align:right;"> 334.5872 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 334.5872 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500310 </td>
   <td style="text-align:right;"> 278.9675 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 1.7582766 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 1.6703628 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 1.7358107 </td>
   <td style="text-align:right;"> 1.6616292 </td>
   <td style="text-align:right;"> 131 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 279.757 </td>
   <td style="text-align:right;"> 278.9675 </td>
   <td style="text-align:right;"> 278.9675 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 278.9675 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3708179 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 200 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5430805 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 220 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 241 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.9 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5431549 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 240 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5433744 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 55 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.5 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5437819 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 100 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5452962 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 230 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5453613 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 70 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 0.000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 5461469 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 20 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 90 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0.0000000 </td>
   <td style="text-align:right;"> 0 </td>
   <td style="text-align:right;"> 242 </td>
   <td style="text-align:right;">  </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.0000 </td>
   <td style="text-align:right;"> 0.8 </td>
   <td style="text-align:right;"> 0.0000 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500312 </td>
   <td style="text-align:right;"> 211.8090 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 1.3456786 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 1.2783947 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 1.3180121 </td>
   <td style="text-align:right;"> 1.2440908 </td>
   <td style="text-align:right;"> 135 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 208.348 </td>
   <td style="text-align:right;"> 208.0649 </td>
   <td style="text-align:right;"> 208.9724 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 208.4279 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500313 </td>
   <td style="text-align:right;"> 158.0590 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 0.8204428 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.7794206 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 0.7963535 </td>
   <td style="text-align:right;"> 0.7283206 </td>
   <td style="text-align:right;"> 168 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 159.169 </td>
   <td style="text-align:right;"> 158.0590 </td>
   <td style="text-align:right;"> 158.0590 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 158.0590 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500315 </td>
   <td style="text-align:right;"> 366.3737 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 2.4505315 </td>
   <td style="text-align:right;"> 130 </td>
   <td style="text-align:right;"> 2.3280049 </td>
   <td style="text-align:right;"> 110 </td>
   <td style="text-align:right;"> 2.3700009 </td>
   <td style="text-align:right;"> 2.2147070 </td>
   <td style="text-align:right;"> 107 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 339.510 </td>
   <td style="text-align:right;"> 350.6961 </td>
   <td style="text-align:right;"> 350.6961 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 350.6961 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500316 </td>
   <td style="text-align:right;"> 425.7419 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 4.7151037 </td>
   <td style="text-align:right;"> 60 </td>
   <td style="text-align:right;"> 4.4793485 </td>
   <td style="text-align:right;"> 50 </td>
   <td style="text-align:right;"> 4.5835162 </td>
   <td style="text-align:right;"> 4.1188325 </td>
   <td style="text-align:right;"> 48 </td>
   <td style="text-align:right;"> 166 </td>
   <td style="text-align:right;"> 423.717 </td>
   <td style="text-align:right;"> 423.1251 </td>
   <td style="text-align:right;"> 424.9963 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 423.8736 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500317 </td>
   <td style="text-align:right;"> 228.8643 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 1.3221425 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1.2560354 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 1.2906354 </td>
   <td style="text-align:right;"> 1.2019407 </td>
   <td style="text-align:right;"> 146 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 229.058 </td>
   <td style="text-align:right;"> 228.8643 </td>
   <td style="text-align:right;"> 228.8643 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 228.8643 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500318 </td>
   <td style="text-align:right;"> 130.4989 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 0.7609338 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 0.7228871 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 0.7230174 </td>
   <td style="text-align:right;"> 0.6427171 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 128.217 </td>
   <td style="text-align:right;"> 127.8501 </td>
   <td style="text-align:right;"> 128.4616 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 128.0947 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500320 </td>
   <td style="text-align:right;"> 227.9605 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 1.2504176 </td>
   <td style="text-align:right;"> 170 </td>
   <td style="text-align:right;"> 1.1878968 </td>
   <td style="text-align:right;"> 160 </td>
   <td style="text-align:right;"> 1.2142394 </td>
   <td style="text-align:right;"> 1.1223507 </td>
   <td style="text-align:right;"> 157 </td>
   <td style="text-align:right;"> 250 </td>
   <td style="text-align:right;"> 230.614 </td>
   <td style="text-align:right;"> 227.9605 </td>
   <td style="text-align:right;"> 227.9605 </td>
   <td style="text-align:right;"> 0.0 </td>
   <td style="text-align:right;"> 227.9605 </td>
  </tr>
  <tr>
   <td style="text-align:right;"> 3500321 </td>
   <td style="text-align:right;"> 195.1560 </td>
   <td style="text-align:right;"> 180 </td>
   <td style="text-align:right;"> 1.2152407 </td>
   <td style="text-align:right;"> 150 </td>
   <td style="text-align:right;"> 1.1544786 </td>
   <td style="text-align:right;"> 140 </td>
   <td style="text-align:right;"> 1.1732979 </td>
   <td style="text-align:right;"> 1.0842892 </td>
   <td style="text-align:right;"> 138 </td>
   <td style="text-align:right;"> 206 </td>
   <td style="text-align:right;"> 191.915 </td>
   <td style="text-align:right;"> 191.6291 </td>
   <td style="text-align:right;"> 192.4809 </td>
   <td style="text-align:right;"> 0.4 </td>
   <td style="text-align:right;"> 191.9698 </td>
  </tr>
</tbody>
</table>

</div>

Add a new chunk by clicking the *Insert Chunk* button on the toolbar or by pressing *Ctrl+Alt+I*.

# Level Two

When you save the notebook, an HTML file containing the code and output will be saved alongside it (click the *Preview* button or press *Ctrl+Shift+K* to preview the HTML file).

## Level Three

The preview shows you a rendered HTML copy of the contents of the editor. Consequently, unlike *Knit*, *Preview* does not run any R code chunks. Instead, the output of the chunk when it was last run in the editor is displayed.
