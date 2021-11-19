

This is a quick re-visualization of a much-maligned plot from [the Washington Post](https://www.washingtonpost.com/politics/2021/11/16/party-divide-vaccination/) using Kaiser Family Foundation polling data.


```r
dat <- tribble(
  ~Population,                ~PoliticalOrientation,              ~Percent,
  "Total population",         "Democrats/leaning independents",   43,
  "Total population",         "Republicans/leaning independents", 41,
  "Unvaccinated\nin October", "Democrats/leaning independents",   17,
  "Unvaccinated\nin October", "Republicans/leaning independents", 60,
  "Unvaccinated\nin April",   "Democrats/leaning independents",   36,
  "Unvaccinated\nin April",   "Republicans/leaning independents", 42
  ) %>% 
  mutate(
    PercentLabel = Percent,
    Percent = if_else(
      PoliticalOrientation == "Democrats/leaning independents",
      Percent * -1,
      Percent
    )
  )

g <- dat  %>% 
  ggplot(
    aes(
      x = Percent,
      y = Population,
      fill = PoliticalOrientation,
      color = PoliticalOrientation,
      label = scales::percent(PercentLabel, scale = 1, accuracy = 1)
    )
  )

g +
  geom_col(width = .8) +
  geom_text(
    data = subset(
      dat, 
      PoliticalOrientation == "Democrats/leaning independents"
    ),
    color = "white",
    hjust = 0
  ) +
  geom_text(
    data = subset(
      dat, 
      PoliticalOrientation == "Republicans/leaning independents"
    ),
    color = "white",
    hjust = 1
  ) +
  scale_fill_manual(values = c("#2E74C0", "#CB454A")) +
  scale_color_manual(values = c("#2E74C0", "#CB454A")) +
  scale_y_discrete(limits = rev, position = "right") +
  ggtitle("Who are the unvaccinated?") +
  theme_minimal() +
  theme(
    aspect.ratio = .5,
    axis.title = element_blank(),
    axis.text.x = element_blank(),
    panel.grid = element_blank(),
    legend.direction = "vertical",
    legend.position = c(1.02, 1.05),
    legend.title = element_blank(),
    legend.text = element_text(size = 12),
    axis.text.y = element_text(size = 14, face = "bold"),
    plot.title = element_text(size = 20, face = "bold", hjust = 0)
  )
```

![](README_files/figure-html/unnamed-chunk-1-1.png)<!-- -->

Compared to their prevalence in the overall US population:

  * Democrats/leaning independents are **under-represented** among the unvaccinated
  
  * Republicans/leaning independents are **over-represented** among the unvaccinated



```r
sessionInfo()
```

```
## R version 4.0.4 (2021-02-15)
## Platform: x86_64-w64-mingw32/x64 (64-bit)
## Running under: Windows 10 x64 (build 19042)
## 
## Matrix products: default
## 
## locale:
## [1] LC_COLLATE=English_United States.1252 
## [2] LC_CTYPE=English_United States.1252   
## [3] LC_MONETARY=English_United States.1252
## [4] LC_NUMERIC=C                          
## [5] LC_TIME=English_United States.1252    
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] forcats_0.5.1   stringr_1.4.0   dplyr_1.0.5     purrr_0.3.4    
## [5] readr_2.0.2     tidyr_1.1.3     tibble_3.1.0    ggplot2_3.3.5  
## [9] tidyverse_1.3.1
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.7            lubridate_1.7.10      assertthat_0.2.1     
##  [4] digest_0.6.27         utf8_1.2.1            V8_3.4.0             
##  [7] R6_2.5.0              cellranger_1.1.0      backports_1.2.1      
## [10] reprex_2.0.0          evaluate_0.14         highr_0.8            
## [13] httr_1.4.2            pillar_1.6.0          rlang_0.4.10         
## [16] curl_4.3              readxl_1.3.1          rstudioapi_0.13      
## [19] jquerylib_0.1.4       rmarkdown_2.7         labeling_0.4.2       
## [22] munsell_0.5.0         broom_0.7.9           compiler_4.0.4       
## [25] modelr_0.1.8          xfun_0.26             pkgconfig_2.0.3      
## [28] htmltools_0.5.1.1     tidyselect_1.1.0      fansi_0.4.2          
## [31] crayon_1.4.1          tzdb_0.1.2            dbplyr_2.1.1         
## [34] withr_2.4.2           grid_4.0.4            jsonlite_1.7.2       
## [37] gtable_0.3.0          lifecycle_1.0.0       DBI_1.1.1            
## [40] magrittr_2.0.1        scales_1.1.1          cli_3.0.1            
## [43] stringi_1.5.3         cachem_1.0.4          farver_2.1.0         
## [46] fs_1.5.0              xml2_1.3.2            bslib_0.2.4          
## [49] ellipsis_0.3.1        generics_0.1.0        vctrs_0.3.7          
## [52] tools_4.0.4           glue_1.4.2            hms_1.0.0            
## [55] fastmap_1.1.0         rdocsyntax_0.4.1.9000 yaml_2.2.1           
## [58] colorspace_2.0-0      rvest_1.0.0           memoise_2.0.0        
## [61] knitr_1.31            haven_2.3.1           sass_0.3.1
```

