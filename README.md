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
Banqiao <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Banqiao_20170217.csv")
Linkou <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Linkou_20170217.csv")
Sanchong <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Sanchong_20170217.csv")
Tanshui <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Tanshui_20170217.csv")
Tucheng <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Tucheng_20170217.csv")
WanLi <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_WanLi_20170217.csv")
Xinzhuang <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Xinzhuang_20170217.csv")
Xizhi <- read.csv("D:/BigDataHW/HW6/NewTaipeiCity/105_Xizhi_20170217.csv")
NewTaipeiFactoryLocation <- read_csv("D:/BigDataHW/HW6/NewTaipeiFactoryLocation_0002336861720153000188.csv")
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

說明處理資料的步驟 我們要先清洗出各站2016/12/31的資料，再把他們結合成一張表方便比對，再來將所有的數據Factor轉成num，由於轉型態Factor轉成num的過程中，是用level轉，所以資料會跟原來大不相同，因此這裡先將factor轉成charactor再轉成num， 處理資料

``` r
#這是R Code Chunk
AirPollution.20160231 <- rbind(Banqiao[grepl("2016/12/31",Banqiao$日期),],    Linkou[grepl("2016/12/31",Linkou$日期),],                                                Sanchong[grepl("2016/12/31",Sanchong$日期),],  Tanshui[grepl("2016/12/31",Tanshui$日期),],                                              Tucheng[grepl("2016/12/31",Tucheng$日期),],    WanLi[grepl("2016/12/31",WanLi$日期),],
                               Xinzhuang[grepl("2016/12/31",Xinzhuang$日期),],Xizhi[grepl("2016/12/31",Xizhi$日期),])
for (i in 4:ncol(AirPollution.20160231)) {
   if (!all(AirPollution.20160231[,i] %in% c("NR"," ","x","*","#"))) {
     AirPollution.20160231[,i] <- as.numeric(as.character(AirPollution.20160231[,i]))
   }
}
```

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

    ## Warning: 強制變更過程中產生了 NA

``` r
AirPollution.20160231$Average<- rowMeans(AirPollution.20160231[, 4:27],na.rm = TRUE)
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
