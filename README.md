長庚大學 大數據分析方法 作業六
================

作業說明 （繳交時請直接刪除這個章節）
-------------------------------------

作業目的：期末專題暖身

依下列指示，完成期末分析專題作業規劃：

-   訂出分析問題，並將R Markdown的一級標題(第一行的title:)中的"長庚大學 大數據分析方法 作業六"取代為期末專題的分析問題，並在分析議題背景前加上組員姓名 (`10pt`)
-   分析議題背景 (`10pt`) 與動機 (`10pt`)
-   資料說明 (`10pt`) 與 載入 (`10pt`)
-   資料處理與清洗 (`10pt`) 說明 (`10pt`)
-   對資料們進行探索式資料分析，圖文並茂佳!(`20pt`)
-   期末專題分析規劃與假設 (`10pt`)

分析議題背景
------------

背景介紹背景介紹

分析動機
--------

分析動機分析動機

使用資料
--------

說明使用資料們

載入使用資料們

``` r
#這是R Code Chunk
library(readr)
```

    ## Warning: package 'readr' was built under R version 3.3.3

``` r
Banqiao <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Banqiao_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )

    ## See spec(...) for full column specifications.

``` r
Linkou <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Linkou_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )
    ## See spec(...) for full column specifications.

``` r
Sanchong <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Sanchong_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )
    ## See spec(...) for full column specifications.

``` r
Tanshui <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = ""),
    ##   `00` = col_double(),
    ##   `01` = col_double(),
    ##   `02` = col_double(),
    ##   `03` = col_double(),
    ##   `04` = col_double(),
    ##   `19` = col_double(),
    ##   `20` = col_double(),
    ##   `21` = col_double(),
    ##   `22` = col_double()
    ## )
    ## See spec(...) for full column specifications.

    ## Warning: 30 parsing failures.
    ##  row col               expected actual                                                      file
    ## 1159  20 no trailing characters      * 'D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv'
    ## 1159  21 no trailing characters      * 'D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv'
    ## 1216  00 no trailing characters      * 'D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv'
    ## 1573  21 no trailing characters      # 'D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv'
    ## 1585  19 no trailing characters      # 'D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv'
    ## .... ... ...................... ...... .........................................................
    ## See problems(...) for more details.

``` r
Tucheng <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Tucheng_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )
    ## See spec(...) for full column specifications.

``` r
WanLi <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_WanLi_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )
    ## See spec(...) for full column specifications.

``` r
Xinzhuang <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Xinzhuang_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )
    ## See spec(...) for full column specifications.

``` r
Xizhi <- read_csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Xizhi_20170217.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   `<U+00A4><e9><U+00B4><c1>` = col_date(format = "")
    ## )
    ## See spec(...) for full column specifications.

``` r
FactoryLocation <- read_csv("D:/BigDataHW/HW6/NewTaipeiFactoryLocation_0002336861720153000188.csv")
```

    ## Parsed with column specification:
    ## cols(
    ##   PRF = col_character(),
    ##   FACT_NAME = col_character(),
    ##   REGI_ID = col_character(),
    ##   FACT_ADDR = col_character(),
    ##   RES_NAME = col_character(),
    ##   PRF8 = col_character(),
    ##   PRD8 = col_character(),
    ##   REGI_APP_DATE = col_character(),
    ##   STATUS = col_character()
    ## )

資料處理與清洗
--------------

說明處理資料的步驟

處理資料

``` r
#這是R Code Chunk
```

探索式資料分析
--------------

圖文並茂圖文並茂

``` r
#這是R Code Chunk
```

期末專題分析規劃
----------------

期末專題要做XXOOO交叉分析
