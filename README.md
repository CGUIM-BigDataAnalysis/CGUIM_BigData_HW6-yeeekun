長庚大學 大數據分析方法 作業六
================

分析議題背景
------------

PM 就是英文Particulate Matter（顆粒物）的縮寫。 PM10代表空氣中空氣動力學粒徑小於等於10微米的所有顆粒物； PM2.5代表空氣中空氣動力學粒徑小於等於2.5微米的所有顆粒物；又稱細顆粒，細顆粒進入肺泡。 由此可見，PM10中包含粒徑小於2.5微米的顆粒物，即PM10包含PM2.5。世界衛生組織（WHO）指出： 對於發展中國家的城市：PM10中有近一半是PM2.5； 對於已開發國家城市:PM10中PM2.5占50%至80%。由此，當空氣中PM2.5的濃度數據增加時，PM10的濃度數據就會升高。故目前歐盟國家仍然只監測PM10的濃度數據。 PM10能被人直接吸入呼吸道，但部分可通過痰液等排出體外，也會被鼻腔內部的絨毛阻擋，對人體健康危害相對較小。不過PM10對於能見度和溫度的影響非常明顯。 PM2.5通過呼吸可以到達肺部。這些顆粒物在肺泡上沉積下來，會干擾肺部的氣體交換，損傷肺泡和粘膜，引起肺組織的慢性纖維化，導致肺心病，加重哮喘病，引起慢性鼻咽炎、慢性支氣管炎等一系列病變，這些顆粒物還可以通過支氣管和肺泡進入血液，其中的有害氣體、重金屬等溶解在血液中，嚴重的可危及生命，對兒童和老年人的危害尤為明顯。

分析動機
--------

相信大家都對PM10及PM2.5這幾個名詞不陌生了，也知道它對人體健康會有危害，因此我們找出開放資料，就北部幾個區域於2016/12/31這一天的情況來比較看看到底是哪裡的空氣最髒，再來看看工廠在各區的分布多寡和空氣汙染的高低有沒有關聯。

使用資料
--------

關於讀入資料之欄位名稱及內容說明 普通測站資料註記說明： \`\# 表示儀器檢核為無效值 \* 表示程式檢核為無效值 x 表示人工檢核為無效值 NR 表示無降雨(已更改過) 空白 表示缺值

測項簡稱 單位 測項名稱 CO ppm 一氧化碳 O3 ppb 臭氧 PM10 μg/m3 懸浮微粒 PM2.5 μg/m3 細懸浮微粒 NOX ppb 氮氧化物 NO ppb 一氧化氮 NO2 ppb 二氧化氮 AMB\_TEMP ℃ 大氣溫度 RH % 相對溼度

載入使用資料們

資料處理與清洗
--------------

說明處理資料的步驟 我們要先清洗出各站2016/12/31的資料，再把他們結合成一張表方便比對，再來將所有的數據charactor轉成num，由於資料表型態原本讀入是‘tbl\_df’, ‘tbl’ and 'data.frame'，將字串轉成num時會有是資料是list所以物件無法強制轉成double的問題，因此在這裡先把資料表轉成data.frame再將資料型態轉換，再新增一個欄位求出當日的個數據平均。 而工廠清洗，我們先洗出還在生產中的工廠，再新增一個欄位Area填入區域，方便後便製圖以及比較，只用group\_by和summarise求出每個區域有多少工廠，然後再以工廠數量來排序。 處理資料

``` r
#這是R Code Chunk
library(dplyr)
```

    ## Warning: package 'dplyr' was built under R version 3.3.3

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(ggmap)
```

    ## Warning: package 'ggmap' was built under R version 3.3.3

    ## Loading required package: ggplot2

    ## Warning: package 'ggplot2' was built under R version 3.3.3

``` r
#Banqiao
names(Banqiao)[names(Banqiao)=="日期"]="Date";
names(Banqiao)[names(Banqiao)=="測站"]="Station";
names(Banqiao)[names(Banqiao)=="測項"]="Item";
#Linkou
names(Linkou)[names(Linkou)=="日期"]="Date";
names(Linkou)[names(Linkou)=="測站"]="Station";
names(Linkou)[names(Linkou)=="測項"]="Item";
#Sanchong
names(Sanchong)[names(Sanchong)=="日期"]="Date";
names(Sanchong)[names(Sanchong)=="測站"]="Station";
names(Sanchong)[names(Sanchong)=="測項"]="Item";
#Tucheng
names(Tucheng)[names(Tucheng)=="日期"]="Date";
names(Tucheng)[names(Tucheng)=="測站"]="Station";
names(Tucheng)[names(Tucheng)=="測項"]="Item";
#WanLi
names(WanLi)[names(WanLi)=="日期"]="Date";
names(WanLi)[names(WanLi)=="測站"]="Station";
names(WanLi)[names(WanLi)=="測項"]="Item";
#Xinzhuang
names(Xinzhuang)[names(Xinzhuang)=="日期"]="Date";
names(Xinzhuang)[names(Xinzhuang)=="測站"]="Station";
names(Xinzhuang)[names(Xinzhuang)=="測項"]="Item";
#Xizhi
names(Xizhi)[names(Xizhi)=="日期"]="Date";
names(Xizhi)[names(Xizhi)=="測站"]="Station";
names(Xizhi)[names(Xizhi)=="測項"]="Item";

#7月
AirPollution.201607 <- rbind(
  Banqiao[grepl("2016/07",Banqiao$Date),],
  Linkou[grepl("2016/07",Linkou$Date),],
  Sanchong[grepl("2016/07",Sanchong$Date),],
  Tucheng[grepl("2016/07",Tucheng$Date),],
  WanLi[grepl("2016/07",WanLi$Date),],
  Xinzhuang[grepl("2016/07",Xinzhuang$Date),],
  Xizhi[grepl("2016/07",Xizhi$Date),])
AirPollution.201607 <- as.data.frame(AirPollution.201607)
for (i in 4:ncol(AirPollution.201607)) {
   if (!all(AirPollution.201607[,i] %in% c(" ","x","*","#"))) {
     AirPollution.201607[,i] <- as.numeric(AirPollution.201607[,i])
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

``` r
AirPollution.201607$Average<- round(rowMeans(AirPollution.201607[, 4:27],na.rm = TRUE),2)
AirPollution.201607 <- AirPollution.201607[order(AirPollution.201607$Item, -AirPollution.201607$Average),]
#8月
AirPollution.201608 <- rbind(
  Banqiao[grepl("2016/08",Banqiao$Date),],
  Linkou[grepl("2016/08",Linkou$Date),],
  Sanchong[grepl("2016/08",Sanchong$Date),],
  Tucheng[grepl("2016/08",Tucheng$Date),],
  WanLi[grepl("2016/08",WanLi$Date),],
  Xinzhuang[grepl("2016/08",Xinzhuang$Date),],
  Xizhi[grepl("2016/08",Xizhi$Date),])
AirPollution.201608 <- as.data.frame(AirPollution.201608)
for (i in 4:ncol(AirPollution.201608)) {
   if (!all(AirPollution.201608[,i] %in% c(" ","x","*","#"))) {
     AirPollution.201608[,i] <- as.numeric(AirPollution.201608[,i])
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

``` r
AirPollution.201608$Average<- round(rowMeans(AirPollution.201608[, 4:27],na.rm = TRUE),2)
AirPollution.201608 <- AirPollution.201608[order(AirPollution.201608$Item, -AirPollution.201608$Average),]
#9月
AirPollution.201609 <- rbind(
  Banqiao[grepl("2016/09",Banqiao$Date),],
  Linkou[grepl("2016/09",Linkou$Date),],
  Sanchong[grepl("2016/09",Sanchong$Date),],
  Tucheng[grepl("2016/09",Tucheng$Date),],
  WanLi[grepl("2016/09",WanLi$Date),],
  Xinzhuang[grepl("2016/09",Xinzhuang$Date),],
  Xizhi[grepl("2016/09",Xizhi$Date),])
AirPollution.201609 <- as.data.frame(AirPollution.201609)
for (i in 4:ncol(AirPollution.201609)) {
   if (!all(AirPollution.201609[,i] %in% c(" ","x","*","#"))) {
     AirPollution.201609[,i] <- as.numeric(AirPollution.201609[,i])
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

``` r
AirPollution.201609$Average<- round(rowMeans(AirPollution.201609[, 4:27],na.rm = TRUE),2)
AirPollution.201609 <- AirPollution.201609[order(AirPollution.201609$Item, -AirPollution.201609$Average),]
#10月
AirPollution.201610 <- rbind(
  Banqiao[grepl("2016/10",Banqiao$Date),],
  Linkou[grepl("2016/10",Linkou$Date),],
  Sanchong[grepl("2016/10",Sanchong$Date),],
  Tucheng[grepl("2016/10",Tucheng$Date),],
  WanLi[grepl("2016/10",WanLi$Date),],
  Xinzhuang[grepl("2016/10",Xinzhuang$Date),],
  Xizhi[grepl("2016/10",Xizhi$Date),])
AirPollution.201610 <- as.data.frame(AirPollution.201610)
for (i in 4:ncol(AirPollution.201610)) {
   if (!all(AirPollution.201610[,i] %in% c(" ","x","*","#"))) {
     AirPollution.201610[,i] <- as.numeric(AirPollution.201610[,i])
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

``` r
AirPollution.201610$Average<- round(rowMeans(AirPollution.201610[, 4:27],na.rm = TRUE),2)
AirPollution.201610 <- AirPollution.201610[order(AirPollution.201610$Item, -AirPollution.201610$Average),]
#11月
AirPollution.201611 <- rbind(
  Banqiao[grepl("2016/11",Banqiao$Date),],
  Linkou[grepl("2016/11",Linkou$Date),],
  Sanchong[grepl("2016/11",Sanchong$Date),],
  Tucheng[grepl("2016/11",Tucheng$Date),],
  WanLi[grepl("2016/11",WanLi$Date),],
  Xinzhuang[grepl("2016/11",Xinzhuang$Date),],
  Xizhi[grepl("2016/11",Xizhi$Date),])
AirPollution.201611 <- as.data.frame(AirPollution.201611)
for (i in 4:ncol(AirPollution.201611)) {
   if (!all(AirPollution.201611[,i] %in% c(" ","x","*","#"))) {
     AirPollution.201611[,i] <- as.numeric(AirPollution.201611[,i])
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

``` r
AirPollution.201611$Average<- round(rowMeans(AirPollution.201611[, 4:27],na.rm = TRUE),2)
AirPollution.201611 <- AirPollution.201611[order(AirPollution.201611$Item, -AirPollution.201611$Average),]
#12月
AirPollution.201612 <- rbind(
  Banqiao[grepl("2016/12",Banqiao$Date),],
  Linkou[grepl("2016/12",Linkou$Date),],
  Sanchong[grepl("2016/12",Sanchong$Date),],
  Tucheng[grepl("2016/12",Tucheng$Date),],
  WanLi[grepl("2016/12",WanLi$Date),],
  Xinzhuang[grepl("2016/12",Xinzhuang$Date),],
  Xizhi[grepl("2016/12",Xizhi$Date),])
AirPollution.201612 <- as.data.frame(AirPollution.201612)
for (i in 4:ncol(AirPollution.201612)) {
   if (!all(AirPollution.201612[,i] %in% c(" ","x","*","#"))) {
     AirPollution.201612[,i] <- as.numeric(AirPollution.201612[,i])
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

``` r
AirPollution.201612$Average<- round(rowMeans(AirPollution.201612[, 4:27],na.rm = TRUE),2)
AirPollution.201612 <- AirPollution.201612[order(AirPollution.201612$Item, -AirPollution.201612$Average),]

##7+8+9+10+11+12月
Air_Tidy<-rbind(
  AirPollution.201607[,-4:-27] %>% group_by(Station,Item) %>%
    summarise(
      Month= 7,
      Mean = mean(Average,na.rm = T)),
  AirPollution.201608[,-4:-27] %>% group_by(Station,Item) %>%
    summarise(
      Month= 8,
      Mean = mean(Average,na.rm = T)),
  AirPollution.201609[,-4:-27] %>% group_by(Station,Item) %>%
    summarise(
      Month= 9,
      Mean = mean(Average,na.rm = T)),
  AirPollution.201610[,-4:-27] %>% group_by(Station,Item) %>%
    summarise(
      Month= 10,
      Mean = mean(Average,na.rm = T)),
  AirPollution.201611[,-4:-27] %>% group_by(Station,Item) %>%
    summarise(
      Month= 11,
      Mean = mean(Average,na.rm = T)),
  AirPollution.201612[,-4:-27] %>% group_by(Station,Item) %>%
    summarise(
      Month= 12,
      Mean = mean(Average,na.rm = T))
  )
#Air_Tidy

##工廠清洗
factory_operate <- NewTaipeiFactoryLocation[grepl("生產中",NewTaipeiFactoryLocation$STATUS),]
factory_operate$Area <- substr(factory_operate$FACT_ADDR,4,6)
#找出各區工廠數量，再以數量排序
factory_Area_Qunality <- group_by(factory_operate,Area)%>%summarise(nQuantity=n_distinct(REGI_ID))
factory_Area_Qunality <- arrange(factory_Area_Qunality,desc(nQuantity))
#table(factory_operate$Area)

#工廠經緯度轉換 要跑很久別一直RUN
factory_Area<-geocode(factory_Area_Qunality$Area)
```

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%96%B0%E8%8E%8A%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%A8%B9%E6%9E%97%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E4%B8%89%E9%87%8D%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E4%B8%AD%E5%92%8C%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%B1%90%E6%AD%A2%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E5%9C%9F%E5%9F%8E%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E4%BA%94%E8%82%A1%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%96%B0%E5%BA%97%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E9%B6%AF%E6%AD%8C%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%9D%BF%E6%A9%8B%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%B3%B0%E5%B1%B1%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%B7%A1%E6%B0%B4%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E8%98%86%E6%B4%B2%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E4%B8%89%E5%B3%BD%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E5%85%AB%E9%87%8C%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%9E%97%E5%8F%A3%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%B7%B1%E5%9D%91%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E7%91%9E%E8%8A%B3%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E4%B8%89%E8%8A%9D%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E6%B0%B8%E5%92%8C%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E9%87%91%E5%B1%B1%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E8%90%AC%E9%87%8C%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E7%9F%B3%E9%96%80%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E7%9F%B3%E7%A2%87%E5%8D%80&sensor=false

    ## .

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E8%B2%A2%E5%AF%AE%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E5%B9%B3%E6%BA%AA%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E5%9D%AA%E6%9E%97%E5%8D%80&sensor=false

    ## Information from URL : http://maps.googleapis.com/maps/api/geocode/json?address=%E9%9B%99%E6%BA%AA%E5%8D%80&sensor=false

``` r
factory_Area_Qunality<-cbind(factory_Area_Qunality,factory_Area)

##工廠數量&PM2.5之關係整理
a <- Air_Tidy[grepl("PM2.5",Air_Tidy$Item),c("Station","Item","Mean")] %>% group_by(Station) %>%
    summarise(Mean = mean(Mean,na.rm = T))
b <- rbind(factory_Area_Qunality[grepl("三重",factory_Area_Qunality$Area),c("Area","nQuantity")],
           factory_Area_Qunality[grepl("土城",factory_Area_Qunality$Area),c("Area","nQuantity")],
           factory_Area_Qunality[grepl("汐止",factory_Area_Qunality$Area),c("Area","nQuantity")],
           factory_Area_Qunality[grepl("板橋",factory_Area_Qunality$Area),c("Area","nQuantity")],
           factory_Area_Qunality[grepl("林口",factory_Area_Qunality$Area),c("Area","nQuantity")],
           factory_Area_Qunality[grepl("新莊",factory_Area_Qunality$Area),c("Area","nQuantity")],
           factory_Area_Qunality[grepl("萬里",factory_Area_Qunality$Area),c("Area","nQuantity")])
b$Area <-sub("區","",b$Area)   
names(b)[names(b)=="Area"]="Station"
Relationship <- merge(a, b, by = "Station")
```

探索式資料分析
--------------

圖文並茂圖文並茂

``` r
##作圖事前準備
library(gplots)
```

    ## Warning: package 'gplots' was built under R version 3.3.3

    ## 
    ## Attaching package: 'gplots'

    ## The following object is masked from 'package:stats':
    ## 
    ##     lowess

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.3.3

    ## Loading tidyverse: tibble
    ## Loading tidyverse: tidyr
    ## Loading tidyverse: purrr

    ## Warning: package 'tidyr' was built under R version 3.3.3

    ## Warning: package 'purrr' was built under R version 3.3.3

    ## Conflicts with tidy packages ----------------------------------------------

    ## filter(): dplyr, stats
    ## lag():    dplyr, stats

``` r
library(maps)
```

    ## Warning: package 'maps' was built under R version 3.3.3

    ## 
    ## Attaching package: 'maps'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     map

``` r
library(maptools)
```

    ## Warning: package 'maptools' was built under R version 3.3.3

    ## Loading required package: sp

    ## Warning: package 'sp' was built under R version 3.3.3

    ## Checking rgeos availability: TRUE

``` r
library(reshape2)
```

    ## 
    ## Attaching package: 'reshape2'

    ## The following object is masked from 'package:tidyr':
    ## 
    ##     smiths

``` r
library(tidyr)
library(ggplot2)
library(showtext)
```

    ## Warning: package 'showtext' was built under R version 3.3.3

    ## Loading required package: sysfonts

    ## Warning: package 'sysfonts' was built under R version 3.3.3

``` r
showtext.auto(enable = TRUE)

##長轉寬
Air_Tidy.s <- spread(Air_Tidy, Item, Mean)
##寬轉長
Air_Tidy.m <- melt(Air_Tidy, id.vars = c("Station","Month","Item"))

#雙變量----PM10跟PM2.5的相關性，資料超過30筆看起來似乎有相關
cor(Air_Tidy.s$PM10,Air_Tidy.s$PM2.5)
```

    ## [1] 0.5141622

``` r
#散佈圖(是否PM10越高則PM2.5越高，因為PM10包含2.5)
ggplot(data=Air_Tidy.s) +   
    # 散布圖對應的函式是geom_point()
    geom_point(aes(x=PM2.5,  # 用aes()，描繪散布圖內的各種屬性
                   y=PM10,
                   color=Month
                   )
               )+
  # 用geom_smooth()加上趨勢線
  geom_smooth(aes(x=PM2.5,
                  y=PM10),method='lm') +
  scale_color_continuous(low = "lightblue",high = "darkblue")+
  # 用labs()，進行文字上的標註(Annotation)
  labs(title="Scatter of PM10-PM2.5",
         x="PM2.5",
         y="PM10") + theme_bw()+  ##微軟正黑體
  theme(text=element_text(family="wqy-microhei"))
```

![](README_files/figure-markdown_github/unnamed-chunk-2-1.png)

``` r
##工廠分布圖
TaipeiMap <- get_map(
    location = c(121.34,24.93,121.64,25.24), 
    zoom = 11,maptype = 'terrain',language = "zh-TW") ## 地勢圖 中文
```

    ## Warning: bounding box given to google - spatial extent only approximate.

    ## converting bounding box to center/zoom specification. (experimental)

    ## Map from URL : http://maps.googleapis.com/maps/api/staticmap?center=25.085,121.49&zoom=11&size=640x640&scale=2&maptype=terrain&language=zh-TW&sensor=false

``` r
##ggmap(TaipeiMap)
TaipeiMapO <- ggmap(TaipeiMap)+ 
    geom_point(data=factory_Area_Qunality,
               aes(x=lon, y=lat,
                   color=nQuantity,size=5)
               )+
  scale_color_continuous(low = "yellow",high = "red")+
  guides(size=FALSE)+
  labs(title="工廠地區分布與數量",
         x="經度",
         y="緯度",color="數量") + theme_bw()+
  theme(text=element_text(family="wqy-microhei"))
TaipeiMapO
```

    ## Warning: Removed 6 rows containing missing values (geom_point).

![](README_files/figure-markdown_github/unnamed-chunk-2-2.png)

``` r
#長條圖//這裡要+
qplot(Month, data = Air_Tidy.s, geom = "bar", weight = PM2.5) + facet_grid(.~Station)+
  xlab("月份")+ylab("PM2.5")+
  geom_bar(stat = 'count', fill = 'orange', colour = 'red')+ theme_bw()+
  theme(text=element_text(family="wqy-microhei"))
```

![](README_files/figure-markdown_github/unnamed-chunk-2-3.png)

``` r
#製作空氣汙染與工廠數量關係圖表
RelationshipValue <- cor(as.numeric(Relationship$Mean),as.numeric(Relationship$nQuantity))
ggplot(data=Relationship) +   
    # 散布圖對應的函式是geom_point()
    geom_point(aes(x=Mean,  # 用aes()，描繪散布圖內的各種屬性
                   y=nQuantity,
                   color=Station,
                   size=5
                   )
               )+guides(size=FALSE)+
  # 用labs()，進行文字上的標註(Annotation)
  labs(title="地區工廠數量與PM2.5之關係",
         x="PM2.5",
         y="工廠數量",color="地區") + theme_bw()+
  theme(text=element_text(family="wqy-microhei"))
```

![](README_files/figure-markdown_github/unnamed-chunk-2-4.png)

``` r
##把風向風速降雨、雨導電、紫外線、PH、CO、CH4值等較無用資料拿掉
air_t.m<-Air_Tidy.m
airduce<-rbind(Air_Tidy.m[grepl("WIND_DIREC",Air_Tidy.m$Item),],Air_Tidy.m[grepl("WD_HR",Air_Tidy.m$Item),],Air_Tidy.m[grepl("WIND_SPEED",Air_Tidy.m$Item),],Air_Tidy.m[grepl("WS_HR",Air_Tidy.m$Item),],Air_Tidy.m[grepl("PH_RAIN",Air_Tidy.m$Item),],Air_Tidy.m[grepl("RAINFALL",Air_Tidy.m$Item),],Air_Tidy.m[grepl("UVB",Air_Tidy.m$Item),],Air_Tidy.m[grepl("THC",Air_Tidy.m$Item),],Air_Tidy.m[grepl("NMHC",Air_Tidy.m$Item),],Air_Tidy.m[grepl("RAIN_COND",Air_Tidy.m$Item),],Air_Tidy.m[grepl("CH4",Air_Tidy.m$Item),],Air_Tidy.m[grepl("SO2",Air_Tidy.m$Item),])
##存入(反向比對)
air_t.m<-anti_join(air_t.m,airduce)
```

    ## Joining, by = c("Station", "Month", "Item", "variable", "value")

``` r
##作圖
ggplot(air_t.m, aes(Station,Item)) + 
    geom_tile(aes(fill = value),
              colour = "white")+
    scale_fill_gradient(low = "white", high = "steelblue", na.value ="grey50")+
  labs(title="相對溼度與空氣中成份關係",
         x="地區",
         y="成份",color="數值") +
  theme(text=element_text(family="wqy-microhei"))+
theme_minimal()
```

![](README_files/figure-markdown_github/unnamed-chunk-2-5.png)

``` r
#互動式圖表 ##要markdown不能用這個
#library(plotly)
#ggplotly()
```

期末專題分析規劃
----------------

上面最後一張圖可以比較出七個區域中林口的PM2.5值最高，不知道與附近華亞工業區有沒有相關，後續將在期末報告做進一步研究觀察。 在期末報告中，我們將研究"各地區空氣品質汙染"及"各工廠種類"的關係，利用工廠資訊中的STATUS、PRF、FACT\_NAME、REGI\_ID等欄位，取出正在運作中的工廠及種類、數量，再將取出之結果與空氣品質汙染檢測出的數值加叉比對，找出是否不同種類的工廠、數量會影響空氣品質。
