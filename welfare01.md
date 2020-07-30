한국인의 삶 분석
================
유 지 희
July 30, 2020

# 한국인의 삶의 이해 ‘복지패널데이터’

## 1\. 한국복지패널데이터 분석 준비하기

한국복지패널데이터는 한국보건사회연구원에서 가구의 경제활동을 연구해 정책지원에 반영할 목적으로 발간한 조사자료다.

전국에서 7000여 가구를 선정해 2006년부터 매년 추적 조사한 자료로, 경제활동, 생활실태, 복지욕구 등 천여 개 변수로
구성되어 있다.

### 데이터 분석 준비하기

데이터를 분석하기 위한 준비작업

#### 1\. 데이터 준비하기

Koweps\_hpc10\_2015\_beta1.sav 파일을 다운로드해 프로젝트 폴더에 삽입

#### 2\. 패키지 설치 및 로드하기

해당 파일은 spss 전용 파일로 되어 있다. foreign 패키지를 활용해 파일 불러온다.

``` r
install.packages("foreign")  # foreign 패키지 설치
library(foreign)             # SPSS 파일 로드
library(dplyr)               # 전처리
library(ggplot2)             # 시각화
library(readxl)              # 엑셀 파일 불러오기
install.packages("dplyr")
install.packages("readxl")
```

#### 3\. 데이터 불러오기

foreign 패키지의 read.spss()를 이용해 복지패널데이터를 불러온다

``` r
#데이터 불러오기

raw_welfare <- read.spss(file="Koweps_hpc10_2015_beta1.sav", to.data.frame = T)
```

    ## Warning in read.spss(file = "Koweps_hpc10_2015_beta1.sav", to.data.frame = T):
    ## Koweps_hpc10_2015_beta1.sav: Compression bias (0) is not the usual value of 100

``` r
welfare <- raw_welfare
```

#### 4\. 데이터 검토하기

``` r
head(welfare)
```

    ##   h10_id h10_ind h10_sn h10_merkey h_new h10_cobf h10_reg5 h10_reg7 h10_din
    ## 1      1       1      1      10101     0       NA        1        1     864
    ## 2      2       1      1      20101     0       NA        1        1     600
    ## 3      3       1      1      30101     0       NA        1        1    1571
    ## 4      4       1      1      40101     0       NA        1        1    3579
    ## 5      4       1      1      40101     0       NA        1        1    3579
    ## 6      6       1      1      60101     0       NA        1        1    3030
    ##   h10_cin h10_flag   p10_wgl   p10_wsl   p10_wgc   p10_wsc h10_hc nh1001_1
    ## 1     864        0  776.9947 0.2567795  763.7189 0.2523922      2       NA
    ## 2     600        0  959.6458 0.3171417  949.2291 0.3136992      2       NA
    ## 3    1619        0 1059.1559 0.3500276 1047.6591 0.3462281      1       NA
    ## 4    3687        0 1012.1599 0.3344964  991.5721 0.3276926      1       NA
    ## 5    3687        0 1075.4212 0.3554029 1057.0466 0.3493305      1       NA
    ## 6    3486        0 2243.9456 0.7415743 2211.1575 0.7307386      1       NA
    ##   nh1001_2 h1001_1 h10_pind h10_pid h10_g1 h10_g2 h10_g3 h10_g4 h10_g6 h10_g7
    ## 1       NA       1        1     101      1     10      2   1936      2      0
    ## 2       NA       1        1     201      1     10      2   1945      4      5
    ## 3       NA       1        1     301      1     10      1   1948      3      5
    ## 4       NA       2        1     401      1     10      1   1942      7      3
    ## 5       NA       2        4     402      2      2      2   1923      2      0
    ## 6       NA       5        1     601      1     10      1   1962      6      5
    ##   h10_g8 h10_g9 h10_g10 h10_g11 h10_g12 h1001_110 h1001_5aq1 h1001_5aq2
    ## 1      0      0       2       2       1         1          0          0
    ## 2      0      0       2       2       1         1          0          0
    ## 3      0      0       2       2       1         1          0          0
    ## 4      0      0       3       1       1         5          0          0
    ## 5      0      0       2       1       1         5          0          0
    ## 6      0      0       1       1       1         5          0          0
    ##   h1001_5aq3 h1001_5aq4 h10_med1 h10_med2 h10_med3 h10_med4 h10_med5 h10_med6
    ## 1          0          0        1        3       60        0        0        0
    ## 2          0          0        1        4       28        0        0        0
    ## 3          0          0        1        3       12        0        0        0
    ## 4          0          0        1        3        3        0        0        0
    ## 5          0          0        2        4        6        0        0        0
    ## 6          0          0        1        3        5        0        0        0
    ##   h10_med7 h10_med8 h10_g9_1 h10_med9 h10_med10 h10_eco1 h10_eco2 h10_eco3
    ## 1        3        0        3        8         0        1        2       NA
    ## 2        2        1        3        5         0        1        3       NA
    ## 3        2        0        3        7         0        1        1       NA
    ## 4        1        1        3        3         0        1        1       NA
    ## 5        1        1        3       15         0        2        3       NA
    ## 6        2        1        3       23         1        1        1       NA
    ##   h10_eco4 h10_eco4_1 h10_eco5_1 h10_eco6 h10_eco_7_1 h10_eco_7_2 h10_eco_7_3
    ## 1        9         NA         NA       NA          NA          NA          NA
    ## 2        9         NA         NA       NA          NA          NA          NA
    ## 3        2         NA          1        2           1           2           1
    ## 4        2         NA          1        2           1           2           3
    ## 5        9         NA         NA       NA          NA          NA          NA
    ## 6        6         NA         NA       NA          NA          NA          NA
    ##   h10_eco8 h10_eco9 h10_eco10 h10_eco11 h10_soc1 h10_soc_2 h10_soc_3 h10_soc_4
    ## 1       NA       NA        NA        10        1         0        NA        NA
    ## 2       NA       NA        NA        10        1         0        NA        NA
    ## 3       75      942         3        NA        1         0        NA        NA
    ## 4       42      762         2        NA        1         1        NA        NA
    ## 5       NA       NA        NA        10        2         0        NA        NA
    ## 6       46      530         1        NA        1         2         1         2
    ##   h10_soc_5 h10_soc_6 h10_soc_7 h10_soc_8 h10_soc_9 h10_soc_10 h10_soc_11
    ## 1        NA        NA        NA        NA        NA         NA         NA
    ## 2        NA        NA        NA        NA        NA         NA         NA
    ## 3        NA        NA        NA        NA        NA         NA         NA
    ## 4        NA        NA        NA        NA        NA         NA         NA
    ## 5        NA        NA        NA        NA        NA         NA         NA
    ## 6         1        NA        NA        NA         0          0         NA
    ##   h10_soc8 h10_soc9 h10_soc11 h10_soc10 h10_soc_12 h10_soc_13 h1005_1
    ## 1        0        0         0         0          4          4       1
    ## 2        0        0         0         0          4          4       1
    ## 3        1        1         2         2          4          1       1
    ## 4        2        2         2         2          4          3       1
    ## 5        0        0         0         0          4          4       1
    ## 6        0        0         0         0          4          3       1
    ##   h1005_3aq1 h1005_2 h1005_3 h1005_4 h1005_5 h1005_6 h1005_7 nh1005_8 nh1005_9
    ## 1          2      NA      NA       2      NA      NA       0        4        5
    ## 2          2      NA      NA       2      NA      NA       0        3        4
    ## 3          1      NA      NA       2      NA      NA       0        3        3
    ## 4          2      NA      NA       2      NA      NA       0        3        3
    ## 5          2      NA      NA       2      NA      NA       0        3        3
    ## 6          2      NA      NA       2      NA      NA       1        3        4
    ##   h1005_3aq2 h1006_aq1 h1006_1 h1006_2 h1006_4 h1006_5 h1006_3 h1006_6 h1006_8
    ## 1          0         2       2       3       2      33       2    5000       1
    ## 2          0         2       2       3       3     198       1   60000       1
    ## 3          0         2       2       1       1      23       3     200       1
    ## 4          0         2       1       3       3      73       1   20000       1
    ## 5          0         2       1       3       3      73       1   20000       1
    ## 6         11         2       2       3       3      82       1   50700       1
    ##   h1006_9 h1006_aq2 h1006_aq3 h1006_10 h1006_11 h1006_12 h1006_13 h1006_14
    ## 1      88         0         0        1        1        1        2        1
    ## 2       4         0         0        1        1        1        2        1
    ## 3      88         0         0        1        2        2        2        1
    ## 4      88         0         0        1        1        1        2        1
    ## 5      88         0         0        1        1        1        2        1
    ## 6      88         0         0        1        2        2        2        1
    ##   h1006_21 h1006_22 h1006_23 h1006_24 h1006_25 h1006_27 h1006_30 h1006_33
    ## 1        1        1        1        1        5        2        2        2
    ## 2        1        1        1        1        5        2        2        2
    ## 3        1        1        1        1        5        2        2        2
    ## 4        1        1        1        1        5        2        2        2
    ## 5        1        1        1        1        5        2        2        2
    ## 6        1        1        1        1        5        2        2        2
    ##   h1006_36 h1006_39 h1006_3aq1 h1007_3aq1 h1007_3aq2 h1007_5aq1 h1007_3aq3
    ## 1        2        2          2         23          3          0          0
    ## 2        2        2          2         20          2          0          0
    ## 3        2        2          2         20         10          0         20
    ## 4        2        2          2         30         15          0          0
    ## 5        2        2          2         30         15          0          0
    ## 6        2        2          2         40          3          0          0
    ##   h1007_3aq4 h1007_3aq5 h1007_6aq1 h1007_3aq6 h1007_5aq2 h1007_3aq7 h1007_3aq8
    ## 1        0.0          8        5.0        3.0          0          4         16
    ## 2        0.0          6        2.7        0.5          0          2          4
    ## 3        0.0          6        3.0        1.0          0          2          3
    ## 4        0.0         11        7.3        2.0          0          4         14
    ## 5        0.0         11        7.3        2.0          0          4         14
    ## 6        0.2         15        9.3        2.0          0          8         43
    ##   h1007_3aq9 h1007_3aq10 h1007_3aq11 h1007_5aq3 h1007_5aq4 h1007_3aq13
    ## 1          0           0           1          1          2          13
    ## 2          0           0           1          2          3           7
    ## 3          0           0           1          3          3          14
    ## 4          0           0           2          3         11          36
    ## 5          0           0           2          3         11          36
    ## 6          2          10           2         25         12          68
    ##   h1007_6aq4 h1007_6aq6 h1007_3aq14 h1007_3aq15 h1007_3aq16 h1007_3aq17 h1007_4
    ## 1        0.0        0.0           0           0           0           0       0
    ## 2        1.7        0.0           0           0           0           0       0
    ## 3        1.7        0.0           0           0           0           0       0
    ## 4       25.0        2.5           0           0           0           0       1
    ## 5       25.0        2.5           0           0           0           0       1
    ## 6        2.5       10.0           0           0           0           0       6
    ##   h1007_6aq7 h1007_6aq8 h1007_6aq9 h1007_6aq10 h1007_6aq11 h1007_5 h1007_6aq12
    ## 1          0        0.0        0.0           0           0       0           0
    ## 2          0        0.0        0.0           0           0       0           0
    ## 3          0        0.0        0.0           0           0       4           0
    ## 4          0        1.0        0.0           0           0       8           0
    ## 5          0        1.0        0.0           0           0       8           0
    ## 6          0        4.1        1.8           0           0      32          15
    ##   h1007_6aq13 h1007_6aq14 h1007_9 h1009_9 h1009_6aq4 h10_inc1 h10_inc2_1
    ## 1         0.0         0.0      74      50        100        1          0
    ## 2         0.0         0.0      48      48         70        1          0
    ## 3         3.6         0.7      87      87        100        1          0
    ## 4         8.0         0.0     137     150        200        1          0
    ## 5         8.0         0.0     137     150        200        2          0
    ## 6        17.0         0.0     268     200        300        1          0
    ##   h10_inc2_2 h10_inc3_1 h10_inc3_2 h10_inc4_1 h10_inc4_2 h10_inc5_1 h10_inc5_2
    ## 1         NA          0         NA          0         NA          0         NA
    ## 2         NA          0         NA          0         NA          0         NA
    ## 3         NA          1         12          0         NA          0         NA
    ## 4         NA          1         12          0         NA          0         NA
    ## 5         NA          0         NA          0         NA          0         NA
    ## 6         NA          0         NA          1         12          0         NA
    ##   h10_inc6_1 h10_inc6_2 h10_inc7_1 h10_inc7_2 h1008_106 h1008_107 h1008_108
    ## 1          0         NA          1         12         0         0         0
    ## 2          0         NA          1         12         0         0         0
    ## 3          0         NA          0         NA         0         1         0
    ## 4          0         NA          0         NA         0         1         0
    ## 5          0         NA          1         12         0         1         0
    ## 6          0         NA          0         NA         0         0         1
    ##   h1008_109 h1008_110 h1008_111 h10_inc2_3 h10_inc2 h10_inc3_6 h10_inc3
    ## 1         0         0         1          1       NA          1       NA
    ## 2         0         0         1          1       NA          1       NA
    ## 3         0         0         0          1       NA          1     1440
    ## 4         0         0         1          1       NA          1     2400
    ## 5         0         0         1          2       NA          2       NA
    ## 6         0         0         3          1       NA          1       NA
    ##   h10_inc4_7 h10_inc4 h10_inc4_8 h10_inc4_9 h1008_155 h1008_156 h1008_157
    ## 1          1       NA          1         NA        NA        NA        NA
    ## 2          1       NA          1         NA        NA        NA        NA
    ## 3          1       NA          1         NA        NA        NA        NA
    ## 4          1       NA          1         NA        NA        NA        NA
    ## 5          2       NA          2         NA        NA        NA        NA
    ## 6          1     3000          1       3000        NA        NA        NA
    ##   h1008_158 h1008_160 h1008_159 h1008_3aq3 h1008_161 h1008_162 h1008_163
    ## 1        NA        NA        NA         NA        NA        NA        NA
    ## 2        NA        NA        NA         NA        NA        NA        NA
    ## 3        NA        NA        NA         NA        NA        NA        NA
    ## 4        NA        NA        NA         NA        NA        NA        NA
    ## 5        NA        NA        NA         NA        NA        NA        NA
    ## 6        NA        NA        NA         NA        NA        NA        NA
    ##   h1008_164 h1008_166 h1008_165 h1008_3aq4 h1008_167 h1008_168 h1008_169
    ## 1        NA        NA        NA         NA        NA        NA        NA
    ## 2        NA        NA        NA         NA        NA        NA        NA
    ## 3        NA        NA        NA         NA        NA        NA        NA
    ## 4        NA        NA        NA         NA        NA        NA        NA
    ## 5        NA        NA        NA         NA        NA        NA        NA
    ## 6        NA        NA        NA         NA        NA        NA        NA
    ##   h1008_170 h10_inc7_3 h10_inc7 h1008_aq9 h1008_aq10 h1008_aq11 h1008_aq12
    ## 1        NA          1        0         0          0          0          0
    ## 2        NA          1        0         0          0          0          0
    ## 3        NA          1        0         0          0          0          0
    ## 4        NA          1        0         0          0          0        508
    ## 5        NA          2        0         0          0          0        508
    ## 6        NA          1        0         0          0          0          0
    ##   h1008_aq13 h1008_aq14 h1008_aq15 h1008_6aq1 h1008_aq16 h1008_aq17 h1008_10aq1
    ## 1          0          0          0          0          0         59         120
    ## 2          0          0          0          0          0          0           0
    ## 3          0          0          0          0          0         59         120
    ## 4          0          0          0          0          0         59         120
    ## 5          0          0          0          0          0         59         120
    ## 6          0          0          0          0          0         94         192
    ##   h1008_aq19 h1008_aq20 h1008_aq21 h1008_5aq3 h1008_7aq1 h1008_aq22 h1008_7aq2
    ## 1          0          0          0          0          0          0          0
    ## 2          0          0          0          0          0          0          0
    ## 3          0          0          0          0          0          0          0
    ## 4          0          0          0          0          0          0          0
    ## 5          0          0          0          0          0          0          0
    ## 6          0          0          0          0          0          0          0
    ##   h1008_aq23 h1008_aq24 h1008_4aq116 h1008_4aq117 h1008_5aq1 h1008_7aq4
    ## 1          0          0            0            0          0          0
    ## 2          0          0            0            0          0          0
    ## 3          0          0            0            0          0          0
    ## 4          0          0            0            0          0          0
    ## 5          0          0            0            0          0          0
    ## 6          0          0            0            0          0          0
    ##   h1008_7aq5 h1008_7aq6 h1008_7aq7 h1008_7aq8 h1008_7aq9 h1008_aq25 h1008_7aq10
    ## 1          0          0          0          0          0          0           0
    ## 2          0          0          0          0          0          0           0
    ## 3          0          0          0          0          0          0           0
    ## 4          0          0          0          0          0          0           0
    ## 5          0          0          0          0          0          0           0
    ## 6          0          0          0          0          0          0           0
    ##   h1008_aq26 h1008_aq27 h1008_aq28 h1008_aq29 h1008_3aq5 h1008_4aq118
    ## 1          0          0          0          0          0            0
    ## 2          0          0          0          0          0            0
    ## 3          0          0          0          0          0            0
    ## 4          0          0          0          0          0            0
    ## 5          0          0          0          0          0            0
    ## 6          0          0          0          0          0            0
    ##   h1008_aq30 h1008_6aq3 h1008_3aq6 h1008_3aq7 nh1008_3aq1 h1008_aq32 h1008_aq33
    ## 1          3          0          0        682          NA          0          0
    ## 2          0          0          0        600          NA          0          0
    ## 3          0          0          0          0          NA          0          0
    ## 4          3          0          0          0          NA          0          0
    ## 5          3          0          0          0          NA          0          0
    ## 6          0          0          0          0          NA          0          0
    ##   h1008_aq34 h1008_3aq8 h1008_195 h1008_7aq11 h1009_aq1 h1009_aq2 h1009_aq3
    ## 1          3          0         0           0         0         0         0
    ## 2          0          0         0           0      7000         0         0
    ## 3          0          0         0           0         0         0         0
    ## 4        600          0         0           0         0     20000         0
    ## 5        600          0         0           0         0     20000         0
    ## 6        200          0         0           0         0         0         0
    ##   h1009_aq4 h1009_aq5 h1009_aq6 h1009_aq7 h1009_aq8 h1010_aq1 h1010_aq2
    ## 1         0         0         0         0         0         0         0
    ## 2     24000         0         0         0         0         0         0
    ## 3         0         0         0         0         0         0         0
    ## 4         0         0         0         0      1920         0         0
    ## 5         0         0         0         0      1920         0         0
    ## 6     11000         0         0         0         0         0         0
    ##   h1010_aq3 h1010_aq4 h1010_aq5 h1010_aq6 h1010_aq7 h1010_aq8 h1010_aq9
    ## 1         0         0         0       100         0         0         0
    ## 2         0         0         0       200         0         0         0
    ## 3         0         0         0       500         0         0         0
    ## 4         0         0         0       100         0         0         0
    ## 5         0         0         0       100         0         0         0
    ## 6         0         0         0       500         0         0         0
    ##   h1010_aq10 h1010_aq11 h1010_aq12 h1010_aq13 h1010_aq14 h1010_aq15 h1010_aq16
    ## 1          0          0          0          0          0          0          0
    ## 2          0          0          0          0          0          0          0
    ## 3          0          0          0          0          0          0          0
    ## 4          0          0          0          0          0          0          0
    ## 5          0          0          0          0          0          0          0
    ## 6          0          0          0          0          0          0          0
    ##   h1010_aq17 h1010_aq18 h1010_aq19 h1010_aq20 h1010_26 h1010_27 h1010_aq23
    ## 1          0          0          0          0        0        0          0
    ## 2          0          0          0          0        0        0          0
    ## 3          0          0          0          0        0        0          0
    ## 4          0          0          0          0        0        0          0
    ## 5          0          0          0          0        0        0          0
    ## 6          0          0          0          0        1      500          0
    ##   h1010_aq24 h1010_aq25 h1010_aq26 h1011_2 h1011_3 h1011_4 h1011_5 h1011_6
    ## 1          0          0          0       2       2       2       3       2
    ## 2          0          0          0       3       2       2       3       2
    ## 3          0          0          0       2       2       2       3       2
    ## 4          0          0          0       3       2       2       3       2
    ## 5          0          0          0       3       2       2       3       2
    ## 6        400          0          0       3       2       2       2       2
    ##   h1011_7 h1011_8 h1011_3aq1 h1011_3aq2 h1011_3aq3 h1011_3aq4 h1011_3aq5
    ## 1       2       2          2          3          3          2         NA
    ## 2       2       2          2          3          3          2         NA
    ## 3       2       2          2          3          3          2         NA
    ## 4       2       2          2          3          2          2         NA
    ## 5       2       2          2          3          2          2         NA
    ## 6       2       2          2          3          2          2         NA
    ##   h1011_3aq6 h1011_3aq7 h1012_1 nh1012_1 h1012_2 h1012_3 h1012_4 h1012_5
    ## 1          2          2       2       NA      NA      NA      NA      NA
    ## 2          2          2       2       NA      NA      NA      NA      NA
    ## 3          2          2       2       NA      NA      NA      NA      NA
    ## 4          2          2       2       NA      NA      NA      NA      NA
    ## 5          2          2       2       NA      NA      NA      NA      NA
    ## 6          2          2       2       NA      NA      NA      NA      NA
    ##   h1012_6 h1012_7 nh1012_7 nh1012_8 h1012_9 h1012_10 h1012_11 h1012_12 h1012_13
    ## 1      NA       1       NA       NA      NA       NA       NA       NA       NA
    ## 2      NA       1       NA       NA      NA       NA       NA       NA       NA
    ## 3      NA       1       NA       NA      NA       NA       NA       NA       NA
    ## 4      NA       1       NA       NA      NA       NA       NA       NA       NA
    ## 5      NA       1       NA       NA      NA       NA       NA       NA       NA
    ## 6      NA       1       NA       NA      NA       NA       NA       NA       NA
    ##   h1012_14 h1012_15 h1012_3aq1 h1012_16 h1012_17 h1012_18 h1012_19 h1012_1_4aq1
    ## 1       NA       NA         NA       NA       NA       NA       NA            1
    ## 2       NA       NA         NA       NA       NA       NA       NA            1
    ## 3       NA       NA         NA       NA       NA       NA       NA            1
    ## 4       NA       NA         NA       NA       NA       NA       NA            1
    ## 5       NA       NA         NA       NA       NA       NA       NA            1
    ## 6       NA       NA         NA       NA       NA       NA       NA            2
    ##   h1012_1_5aq1 h1012_1_5aq2 h1012_1_5aq3 h1012_1_5aq4 h1012_1_5aq5 h1012_1_4aq2
    ## 1            2           NA           NA           NA           NA            1
    ## 2            2           NA           NA           NA           NA            1
    ## 3            2           NA           NA           NA           NA            1
    ## 4            2           NA           NA           NA           NA            2
    ## 5            2           NA           NA           NA           NA            2
    ## 6            2           NA           NA           NA           NA            1
    ##   h1012_1_4aq3 h1013_2 h1013_6 h1013_10 h1013_14 h1013_18 h1013_22 h1013_26
    ## 1            2       1       1        2        2        2        2        2
    ## 2            2       2       2        2        2        2        2        2
    ## 3            2       1       2        2        2        2        2        2
    ## 4            2       1       2        2        2        2        2        2
    ## 5            2       1       2        2        2        2        2        2
    ## 6            2       1       2        2        2        2        2        2
    ##   h1013_8aq1 h1013_5aq1 h1013_8aq2 h1013_4aq1 h1013_4aq2 h1013_4aq4 h1013_4aq6
    ## 1          2          2          2          2         NA         NA         NA
    ## 2          2          2          2          2         NA         NA         NA
    ## 3          2          2          2          2         NA         NA         NA
    ## 4          2          2          2          2         NA         NA         NA
    ## 5          2          2          2          2         NA         NA         NA
    ## 6          2          2          2          2         NA         NA         NA
    ##   h1013_4aq8 h1013_4aq10 h1013_5aq4 h1013_5aq6 h1013_5aq8 h1013_6aq1
    ## 1         NA          NA         NA         NA         NA         NA
    ## 2         NA          NA         NA         NA         NA         NA
    ## 3         NA          NA         NA         NA         NA         NA
    ## 4         NA          NA         NA         NA         NA         NA
    ## 5         NA          NA         NA         NA         NA         NA
    ## 6         NA          NA         NA         NA         NA         NA
    ##   h1013_4aq14
    ## 1          NA
    ## 2          NA
    ## 3          NA
    ## 4          NA
    ## 5          NA
    ## 6          NA
    ##                                                                                                                                                                                                                                                       h1013_4aq15
    ## 1 .                                                                                                                                                                                                                                                              
    ## 2 .                                                                                                                                                                                                                                                              
    ## 3 .                                                                                                                                                                                                                                                              
    ## 4 .                                                                                                                                                                                                                                                              
    ## 5 .                                                                                                                                                                                                                                                              
    ## 6 .                                                                                                                                                                                                                                                              
    ##   h1013_4aq16 h1013_4aq17 h1013_4aq18 h1013_4aq20 h1013_4aq22 h1013_4aq24
    ## 1           2          NA          NA          NA          NA          NA
    ## 2           2          NA          NA          NA          NA          NA
    ## 3           2          NA          NA          NA          NA          NA
    ## 4           2          NA          NA          NA          NA          NA
    ## 5           2          NA          NA          NA          NA          NA
    ## 6           2          NA          NA          NA          NA          NA
    ##   h1013_4aq26 h1013_4aq28 h1013_4aq30 h1013_4aq32 h1014_4 h1014_8 h1014_12
    ## 1          NA          NA          NA          NA       1       1        2
    ## 2          NA          NA          NA          NA       2       2        2
    ## 3          NA          NA          NA          NA       1       2        2
    ## 4          NA          NA          NA          NA       1       2        2
    ## 5          NA          NA          NA          NA       1       2        2
    ## 6          NA          NA          NA          NA       1       2        2
    ##   h1014_16 h1014_20 h1014_24 h1014_28 h1014_32 h1014_36 h1014_3aq1 h1014_4aq1
    ## 1        2        2        2        2        2        2          2          2
    ## 2        2        2        2        2        2        2          2          2
    ## 3        2        2        2        2        2        2          2          2
    ## 4        2        2        2        2        2        2          2          2
    ## 5        2        2        2        2        2        2          2          2
    ## 6        2        2        2        2        2        2          2          2
    ##   h1015_4 h1015_8 h1015_12 h1015_20 h1015_25 h1015_29 h1015_33 h1015_37
    ## 1      NA      NA       NA       NA       NA       NA       NA       NA
    ## 2      NA      NA       NA       NA       NA       NA       NA       NA
    ## 3      NA      NA       NA       NA       NA       NA       NA       NA
    ## 4      NA      NA       NA       NA       NA       NA       NA       NA
    ## 5      NA      NA       NA       NA       NA       NA       NA       NA
    ## 6       2       2        2        2        1        2        2        2
    ##   h1015_4aq1 h1015_7aq1 h1015_aq1 h1015_40 h1015_41 h1015_42 h1015_43 h1015_44
    ## 1         NA         NA        NA       NA       NA       NA       NA       NA
    ## 2         NA         NA        NA       NA       NA       NA       NA       NA
    ## 3         NA         NA        NA       NA       NA       NA       NA       NA
    ## 4         NA         NA        NA       NA       NA       NA       NA       NA
    ## 5         NA         NA        NA       NA       NA       NA       NA       NA
    ## 6          2          2         2       NA       NA       NA       NA       NA
    ##   h1015_45 h1015_46 h1015_47 h1015_48 h1015_49 h1015_50 h1015_51 h1015_52
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   h1015_53 h1015_54 h1015_55 h1015_56 h1015_57 h1015_60 h1015_aq2 h1015_61
    ## 1       NA       NA       NA       NA       NA       NA        NA       NA
    ## 2       NA       NA       NA       NA       NA       NA        NA       NA
    ## 3       NA       NA       NA       NA       NA       NA        NA       NA
    ## 4       NA       NA       NA       NA       NA       NA        NA       NA
    ## 5       NA       NA       NA       NA       NA       NA        NA       NA
    ## 6       NA       NA       NA       NA       NA        3         1       10
    ##   h1015_62 h1015_63 h1015_66 h1015_67 h1015_68 h1015_aq3 h1015_69 h1015_70
    ## 1       NA       NA       NA       NA       NA        NA       NA       NA
    ## 2       NA       NA       NA       NA       NA        NA       NA       NA
    ## 3       NA       NA       NA       NA       NA        NA       NA       NA
    ## 4       NA       NA       NA       NA       NA        NA       NA       NA
    ## 5       NA       NA       NA       NA       NA        NA       NA       NA
    ## 6       NA       NA       10        0       NA        NA       NA       NA
    ##   h1015_71 h1015_74 h1015_75 h1015_76 h1015_aq4 h1015_77 h1015_78 h1015_79
    ## 1       NA       NA       NA       NA        NA       NA       NA       NA
    ## 2       NA       NA       NA       NA        NA       NA       NA       NA
    ## 3       NA       NA       NA       NA        NA       NA       NA       NA
    ## 4       NA       NA       NA       NA        NA       NA       NA       NA
    ## 5       NA       NA       NA       NA        NA       NA       NA       NA
    ## 6       NA       NA       NA       NA        NA       NA       NA       NA
    ##   h1015_82 h1015_83 h1015_84 h1015_aq5 h1015_85 h1015_86 h1015_87 h1015_90
    ## 1       NA       NA       NA        NA       NA       NA       NA       NA
    ## 2       NA       NA       NA        NA       NA       NA       NA       NA
    ## 3       NA       NA       NA        NA       NA       NA       NA       NA
    ## 4       NA       NA       NA        NA       NA       NA       NA       NA
    ## 5       NA       NA       NA        NA       NA       NA       NA       NA
    ## 6       NA       NA       NA        NA       NA       NA       NA       NA
    ##   h1015_91 h1015_92 h1015_aq6 h1015_93 h1015_94 h1015_95 h1015_98 h1015_99
    ## 1       NA       NA        NA       NA       NA       NA       NA       NA
    ## 2       NA       NA        NA       NA       NA       NA       NA       NA
    ## 3       NA       NA        NA       NA       NA       NA       NA       NA
    ## 4       NA       NA        NA       NA       NA       NA       NA       NA
    ## 5       NA       NA        NA       NA       NA       NA       NA       NA
    ## 6       NA       NA        NA       NA       NA       NA       NA       NA
    ##   h1015_100 h1015_aq7 h1015_101 h1015_102 h1015_103 h1015_106 h1015_107
    ## 1        NA        NA        NA        NA        NA        NA        NA
    ## 2        NA        NA        NA        NA        NA        NA        NA
    ## 3        NA        NA        NA        NA        NA        NA        NA
    ## 4        NA        NA        NA        NA        NA        NA        NA
    ## 5        NA        NA        NA        NA        NA        NA        NA
    ## 6        NA        NA        NA        NA        NA        NA        NA
    ##   h1016_6aq1 h1016_6aq4 h1016_8 h1016_24 h1016_4aq1 h1016_4aq4 h1016_32
    ## 1         NA         NA      NA       NA         NA         NA       NA
    ## 2         NA         NA      NA       NA         NA         NA       NA
    ## 3         NA         NA      NA       NA         NA         NA       NA
    ## 4         NA         NA      NA       NA         NA         NA       NA
    ## 5         NA         NA      NA       NA         NA         NA       NA
    ## 6         NA         NA      NA       NA         NA         NA       NA
    ##   h1016_36 h1016_40 h1016_4aq7 h1016_56 h1016_60 h1016_64 h1017_1 h1017_2
    ## 1       NA       NA         NA       NA       NA       NA       7       4
    ## 2       NA       NA         NA       NA       NA       NA       4       0
    ## 3       NA       NA         NA       NA       NA       NA       0      NA
    ## 4       NA       NA         NA       NA       NA       NA       4       1
    ## 5       NA       NA         NA       NA       NA       NA       4       1
    ## 6       NA       NA         NA       NA       NA       NA       4       1
    ##   h1017_3 h1017_4 h1017_5 h1017_6 h1017_7 p10_fnum p10_tq p10_cp p1001_1
    ## 1       4       2       3       1       1        1      3      1       2
    ## 2       2       2       3       2       2        1      3      1       2
    ## 3       2       1       2       1       1        1      3      1       2
    ## 4       2       2       3       1       1        1      3      1       1
    ## 5       2       2       3       1       1        2      3      1       2
    ## 6       3       1       2       2       1        1      3      1       2
    ##   p1001_2 p1001_aq1 p1001_3 p1001_4 p1001_5 p1001_6 p1001_10 p1001_11 p1001_12
    ## 1      NA        NA      NA      NA      NA      NA       NA       NA       NA
    ## 2      NA        NA      NA      NA      NA      NA       NA       NA       NA
    ## 3      NA        NA      NA      NA      NA      NA       NA       NA       NA
    ## 4       1        NA       1       0      12     508       NA       NA       NA
    ## 5      NA        NA      NA      NA      NA      NA       NA       NA       NA
    ## 6      NA        NA      NA      NA      NA      NA       NA       NA       NA
    ##   p1001_13 p1001_14 p1001_7 p1001_8 p1001_9 p1001_15 p1001_16 p1001_17 p1001_18
    ## 1       NA       NA      NA      NA      NA        2       NA       NA       NA
    ## 2       NA       NA      NA      NA      NA        2       NA       NA       NA
    ## 3       NA       NA      NA      NA      NA        2       NA       NA       NA
    ## 4       NA       NA      NA      NA      NA        2       NA       NA       NA
    ## 5       NA       NA      NA      NA      NA        2       NA       NA       NA
    ## 6       NA       NA      NA      NA      NA        2       NA       NA       NA
    ##   p1001_19 p1001_20 p1001_21 p1001_22 p1001_23 p1001_24 p1001_25 p1001_27
    ## 1       NA        2       NA       NA       NA       NA       NA        2
    ## 2       NA        2       NA       NA       NA       NA       NA        2
    ## 3       NA        2       NA       NA       NA       NA       NA        2
    ## 4       NA        2       NA       NA       NA       NA       NA        2
    ## 5       NA        2       NA       NA       NA       NA       NA        2
    ## 6       NA        2       NA       NA       NA       NA       NA        2
    ##   p1001_28 p1001_29 p1001_30 p1001_31 p1001_32 p1001_33 p1001_34 p1002_1
    ## 1       NA       NA       NA        2       NA       NA       NA       4
    ## 2       NA       NA       NA        2       NA       NA       NA       4
    ## 3       NA       NA       NA        2       NA       NA       NA       1
    ## 4       NA       NA       NA        2       NA       NA       NA       1
    ## 5       NA       NA       NA        2       NA       NA       NA       4
    ## 6       NA       NA       NA        2       NA       NA       NA       2
    ##   p1002_2 p1002_3 p1002_4 p1002_5 p1002_6 p1002_7 p1002_8 p1002_8aq1 p1002_9
    ## 1      NA      NA      NA      NA      NA      NA      NA         NA      NA
    ## 2      NA      NA      NA      NA      NA      NA      NA         NA      NA
    ## 3       2      NA    2013       1      12      15      84        120      NA
    ## 4       1      16    2014      10      12      22      40        200      NA
    ## 5      NA      NA      NA      NA      NA      NA      NA         NA      NA
    ## 6       2      NA    2000       3      12      22      35         NA      NA
    ##   p1002_8aq2 p1002_4aq1 p1002_10 p1002_11 p1002_12 p1002_8aq3 p1002_8aq4
    ## 1         NA         NA        2       NA       NA         NA         NA
    ## 2         NA         NA        2       NA       NA         NA         NA
    ## 3         NA          1       NA       NA       NA         NA         NA
    ## 4         NA          1       NA       NA       NA         NA         NA
    ## 5         NA         NA        2       NA       NA         NA         NA
    ## 6         NA         NA       NA       NA       NA         NA         NA
    ##   p1002_31 p1002_4aq2 p1002_4aq3 p1002_4aq4 p1002_8aq5 p1002_8aq6 p1002_8aq7
    ## 1       NA          2         NA         NA          0          0          0
    ## 2       NA          2         NA         NA          0          0          0
    ## 3       NA         NA         NA         NA          0          0          0
    ## 4       NA         NA         NA         NA          0          0          0
    ## 5       NA          2         NA         NA          0          0          0
    ## 6       NA         NA         NA         NA          0          0          0
    ##   p1002_8aq8 p1002_8aq9 p1002_8aq10 p1002_8aq11 p1002_8aq12 p1002_8aq13
    ## 1          0          0           0           0           0           0
    ## 2          0          0           0           0           0           0
    ## 3          0          0           0           0           0           0
    ## 4          0          0           0           0           0           0
    ## 5          0          0           0           0           0           0
    ## 6          0          0           0           0           0           0
    ##   p1002_8aq14 p1002_8aq15 p1002_aq2 p1002_aq3 p1002_aq4 p1002_aq5 p1003_1
    ## 1           0           0         0        NA        NA        NA       2
    ## 2           0           0         0        NA        NA        NA       2
    ## 3           0           0         0        NA        NA        NA       2
    ## 4           0           0         0        NA        NA        NA       2
    ## 5           0           0         0        NA        NA        NA       2
    ## 6           0           0         0        NA        NA        NA       1
    ##   p1003_2 p1003_5 p1003_6 p1003_7 p1003_8 p1003_9 p1003_10 p1003_11 p1003_12
    ## 1       0       2       3       3       4       3        2        3        3
    ## 2       0       1       3       1       4       3        3        3        3
    ## 3       2       3       2       1       3       3        3        3        3
    ## 4       2       4       4       3       3       3        3        3        4
    ## 5       0       4       4       3       3       3        3        3        4
    ## 6       2       2       2       4       4       4        3        3        3
    ##   p1004_1 p1004_2 p1004_3 p1004_4 p1004_5 p1004_6 p1004_7 p1004_8 p1004_9
    ## 1       1       4       2       2      NA      NA       2      NA      NA
    ## 2       2       4       3       2      NA      NA       2      NA      NA
    ## 3       2       4       3       2      NA      NA       2      NA      NA
    ## 4       2       4       3       2      NA      NA       2      NA      NA
    ## 5       3       4       3       2      NA      NA       2      NA      NA
    ## 6       2       4       4       2      NA      NA       2      NA      NA
    ##   p1004_10 p1004_11 p1004_12 p1004_13 p1004_3aq1 p1004_3aq2 p1004_3aq3
    ## 1       NA       NA       NA       NA          4          4          5
    ## 2       NA       NA       NA       NA          5          2          5
    ## 3       NA       NA       NA       NA          2          2          4
    ## 4       NA       NA       NA       NA          3          2          4
    ## 5       NA       NA       NA       NA          4          2          4
    ## 6       NA       NA       NA       NA          4          3          4
    ##   p1004_3aq4 p1004_3aq5 p1004_3aq6 p1004_3aq7 p1004_3aq8 p1005_3aq1 p1005_3aq2
    ## 1          4          2          1          1          1         NA         NA
    ## 2          1          5          5          4          4         NA         NA
    ## 3          2          4          2          2          2         NA         NA
    ## 4          2          4          2          2          2         NA         NA
    ## 5          3          4          2          2          2         NA         NA
    ## 6          2          4          2          4          4         NA         NA
    ##   p1005_3aq3 p1005_3aq4 p1005_3aq5 p1005_3aq6 p1005_3aq7 p1005_3aq8 p1005_3aq9
    ## 1         NA         NA          2         NA         NA         NA          1
    ## 2         NA         NA          2         NA         NA         NA          1
    ## 3         NA         NA          2         NA         NA         NA          1
    ## 4         NA         NA          2         NA         NA         NA          1
    ## 5         NA         NA          2         NA         NA         NA          1
    ## 6         NA         NA          2         NA         NA         NA          1
    ##   p1005_3aq10 p1005_2 p1005_3 p1005_4aq1 p1005_4aq2 p1005_4aq3 p1005_4aq4
    ## 1          NA       5      NA         NA         NA         NA         NA
    ## 2          NA       5      NA         NA         NA         NA         NA
    ## 3          NA       5      NA         NA         NA         NA         NA
    ## 4          NA       5      NA         NA         NA         NA         NA
    ## 5          NA       5      NA         NA         NA         NA         NA
    ## 6          NA       5      NA         NA         NA         NA         NA
    ##   p1005_4aq5 p1005_4aq6 p1005_4aq7 p1005_4aq8 p1005_5 p1005_6 p1005_7 p1005_8
    ## 1         NA         NA         NA         NA      NA      NA      NA      NA
    ## 2         NA         NA         NA         NA      NA      NA      NA      NA
    ## 3         NA         NA         NA         NA      NA      NA      NA      NA
    ## 4         NA         NA         NA         NA      NA      NA      NA      NA
    ## 5         NA         NA         NA         NA      NA      NA      NA      NA
    ## 6         NA         NA         NA         NA      NA      NA      NA      NA
    ##   p1005_3aq11 p1005_9 p1005_10 p1005_11 p1005_12 p1005_13 p1005_14 p1005_15
    ## 1           2       1        4        1        2        1        2        4
    ## 2           2       2        3        1        2        2        1        3
    ## 3           0       1        4        1        1        1        1        4
    ## 4           0       1        4        1        1        1        1        4
    ## 5           2       1        4        1        1        1        1        4
    ## 6           0       2        3        2        2        2        1        2
    ##   p1005_16 p1005_17 p1005_18 p1005_19 p1005_20 p1005_21 p1005_22 p1005_23
    ## 1        1        1        1        1        2        3        3        1
    ## 2        1        1        1        2        2        2        1        2
    ## 3        1        1        1        1        2        2        2        2
    ## 4        1        1        1        1        4        3        1        4
    ## 5        1        1        1        1        2        2        1        2
    ## 6        1        2        1        2        2        2        2        2
    ##   p1005_24 p1005_25 p1005_26 p1005_27 p1005_28 p1005_29 p1005_4aq9 p1005_4aq10
    ## 1        4        2        1        2        3        2          0           0
    ## 2        2        2        2        2        1        1          0           0
    ## 3        2        2        2        2        1        1          0           0
    ## 4        2        3        3        2        1        1          0           0
    ## 5        2        2        2        2        1        1          0           0
    ## 6        2        2        2        2        1        1          2           1
    ##   p1005_4aq11 p1005_aq1 p1005_aq2 p1005_aq3 p1005_aq4 p1005_6aq1 p1005_6aq2
    ## 1           0         4         0         5         1         NA         NA
    ## 2           0         4         0         4         4         NA         NA
    ## 3           0         3         0         3         0         NA         NA
    ## 4           0         6         0         5         5         NA         NA
    ## 5           0         5         0         5         5         NA         NA
    ## 6           1         6         6         5         0         NA         NA
    ##   p1005_6aq3 p1005_6aq4 p1005_6aq5 p1005_6aq6 p1005_6aq7 p1005_6aq8 p1005_6aq9
    ## 1         NA         NA         NA         NA         NA         NA         NA
    ## 2         NA         NA         NA         NA         NA         NA         NA
    ## 3         NA         NA         NA         NA         NA         NA         NA
    ## 4         NA         NA         NA         NA         NA         NA         NA
    ## 5         NA         NA         NA         NA         NA         NA         NA
    ## 6         NA         NA         NA         NA         NA         NA         NA
    ##   p1005_7aq1 p1005_7aq2 p1005_7aq3 p1007_3aq1 p1007_3aq2 p1007_3aq3 p1007_3aq4
    ## 1          2          2          2         NA         NA         NA         NA
    ## 2          2          2          2         NA         NA         NA         NA
    ## 3          2          2          2         NA         NA         NA         NA
    ## 4          2          2          2         NA         NA         NA         NA
    ## 5          2          2          2         NA         NA         NA         NA
    ## 6          2          2          2         NA         NA         NA         NA
    ##   p1007_3aq5 p1007_3aq6 p1007_3aq7 np1006_1 np1006_2 np1006_3 np1006_4 np1006_5
    ## 1         NA         NA         NA       NA       NA       NA       NA       NA
    ## 2         NA         NA         NA       NA       NA       NA       NA       NA
    ## 3         NA         NA         NA       NA       NA       NA       NA       NA
    ## 4         NA         NA         NA       NA       NA       NA       NA       NA
    ## 5         NA         NA         NA       NA       NA       NA       NA       NA
    ## 6         NA         NA         NA       NA       NA       NA       NA       NA
    ##   np1006_6 np1006_7 np1006_8 np1006_9 np1006_10 np1006_11 np1006_12 np1006_13
    ## 1       NA       NA       NA       NA        NA        NA        NA        NA
    ## 2       NA       NA       NA       NA        NA        NA        NA        NA
    ## 3       NA       NA       NA       NA        NA        NA        NA        NA
    ## 4       NA       NA       NA       NA        NA        NA        NA        NA
    ## 5       NA       NA       NA       NA        NA        NA        NA        NA
    ## 6       NA       NA       NA       NA        NA        NA        NA        NA
    ##   np1006_14 np1006_15 np1006_16 np1006_17 np1006_18 np1006_19 np1006_20
    ## 1        NA        NA        NA        NA        NA        NA        NA
    ## 2        NA        NA        NA        NA        NA        NA        NA
    ## 3        NA        NA        NA        NA        NA        NA        NA
    ## 4        NA        NA        NA        NA        NA        NA        NA
    ## 5        NA        NA        NA        NA        NA        NA        NA
    ## 6        NA        NA        NA        NA        NA        NA        NA
    ##   np1006_21 np1006_22 np1006_23 np1006_24 np1006_25 np1006_26 np1006_27
    ## 1        NA        NA        NA        NA        NA        NA        NA
    ## 2        NA        NA        NA        NA        NA        NA        NA
    ## 3        NA        NA        NA        NA        NA        NA        NA
    ## 4        NA        NA        NA        NA        NA        NA        NA
    ## 5        NA        NA        NA        NA        NA        NA        NA
    ## 6        NA        NA        NA        NA        NA        NA        NA
    ##   np1006_28 np1006_29 np1006_30 np1006_31 np1006_32 np1006_33 np1006_34
    ## 1        NA        NA        NA        NA        NA        NA        NA
    ## 2        NA        NA        NA        NA        NA        NA        NA
    ## 3        NA        NA        NA        NA        NA        NA        NA
    ## 4        NA        NA        NA        NA        NA        NA        NA
    ## 5        NA        NA        NA        NA        NA        NA        NA
    ## 6        NA        NA        NA        NA        NA        NA        NA
    ##   np1006_35 np1006_36 np1006_37 np1006_38 np1006_39 np1006_40 np1006_41
    ## 1        NA        NA        NA        NA        NA        NA        NA
    ## 2        NA        NA        NA        NA        NA        NA        NA
    ## 3        NA        NA        NA        NA        NA        NA        NA
    ## 4        NA        NA        NA        NA        NA        NA        NA
    ## 5        NA        NA        NA        NA        NA        NA        NA
    ## 6        NA        NA        NA        NA        NA        NA        NA
    ##   np1006_42 np1006_43 np1006_44 p1006_3aq1 c10_fnum c10_cp c10_grade c1001_1
    ## 1        NA        NA        NA         NA       NA     NA        NA      NA
    ## 2        NA        NA        NA         NA       NA     NA        NA      NA
    ## 3        NA        NA        NA         NA       NA     NA        NA      NA
    ## 4        NA        NA        NA         NA       NA     NA        NA      NA
    ## 5        NA        NA        NA         NA       NA     NA        NA      NA
    ## 6        NA        NA        NA         NA       NA     NA        NA      NA
    ##   c1001_2 c1001_3 c1001_4 c1001_5 c1001_6 c1001_7 c1001_8 c1001_9 c1001_10
    ## 1      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 2      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 3      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 4      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 5      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 6      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ##   c1001_11 c1001_12 c1001_13 c1001_7aq5 c1001_7aq6 c1001_7aq7 c1001_7aq8
    ## 1       NA       NA       NA         NA         NA         NA         NA
    ## 2       NA       NA       NA         NA         NA         NA         NA
    ## 3       NA       NA       NA         NA         NA         NA         NA
    ## 4       NA       NA       NA         NA         NA         NA         NA
    ## 5       NA       NA       NA         NA         NA         NA         NA
    ## 6       NA       NA       NA         NA         NA         NA         NA
    ##   c1001_4aq1 c1001_4aq2 c1001_4aq3 c1001_4aq4 c1001_4aq5 c1001_4aq6 c1002_1
    ## 1         NA         NA         NA         NA         NA         NA      NA
    ## 2         NA         NA         NA         NA         NA         NA      NA
    ## 3         NA         NA         NA         NA         NA         NA      NA
    ## 4         NA         NA         NA         NA         NA         NA      NA
    ## 5         NA         NA         NA         NA         NA         NA      NA
    ## 6         NA         NA         NA         NA         NA         NA      NA
    ##   c1002_2 c1002_3 c1002_4 c1002_5 c1002_6 c1002_7 c1002_8 c1002_9 c1002_10
    ## 1      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 2      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 3      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 4      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 5      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 6      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ##   c1002_11 c1002_12 c1002_13 c1002_14 c1002_15 c1002_16 c1002_17 c1002_18
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_19 c1002_20 c1002_21 c1002_22 c1002_23 c1002_24 c1002_25 c1002_26
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_4aq1 c1002_27 c1002_28 c1002_29 c1002_30 c1002_31 c1002_32 c1002_33
    ## 1         NA       NA       NA       NA       NA       NA       NA       NA
    ## 2         NA       NA       NA       NA       NA       NA       NA       NA
    ## 3         NA       NA       NA       NA       NA       NA       NA       NA
    ## 4         NA       NA       NA       NA       NA       NA       NA       NA
    ## 5         NA       NA       NA       NA       NA       NA       NA       NA
    ## 6         NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_34 c1002_35 c1002_36 c1002_37 c1002_38 c1002_39 c1002_40 c1002_41
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_42 c1002_43 c1002_44 c1002_45 c1002_46 c1002_47 c1002_48 c1002_49
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_50 c1002_51 c1002_52 c1002_53 c1002_54 c1002_55 c1002_56 c1002_57
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_58 c1002_4aq2 c1002_59 c1002_60 c1002_61 c1002_62 c1002_63 c1002_64
    ## 1       NA         NA       NA       NA       NA       NA       NA       NA
    ## 2       NA         NA       NA       NA       NA       NA       NA       NA
    ## 3       NA         NA       NA       NA       NA       NA       NA       NA
    ## 4       NA         NA       NA       NA       NA       NA       NA       NA
    ## 5       NA         NA       NA       NA       NA       NA       NA       NA
    ## 6       NA         NA       NA       NA       NA       NA       NA       NA
    ##   c1002_65 c1002_66 c1002_67 c1002_68 c1002_69 c1002_70 c1002_71 c1002_72
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_73 c1002_74 c1002_75 c1002_76 c1002_77 c1002_7aq1 c1002_7aq2 c1002_7aq3
    ## 1       NA       NA       NA       NA       NA         NA         NA         NA
    ## 2       NA       NA       NA       NA       NA         NA         NA         NA
    ## 3       NA       NA       NA       NA       NA         NA         NA         NA
    ## 4       NA       NA       NA       NA       NA         NA         NA         NA
    ## 5       NA       NA       NA       NA       NA         NA         NA         NA
    ## 6       NA       NA       NA       NA       NA         NA         NA         NA
    ##   c1002_7aq4 c1002_7aq5 c1002_7aq6 c1002_7aq7 c1002_7aq8 c1002_7aq9 c1002_7aq10
    ## 1         NA         NA         NA         NA         NA         NA          NA
    ## 2         NA         NA         NA         NA         NA         NA          NA
    ## 3         NA         NA         NA         NA         NA         NA          NA
    ## 4         NA         NA         NA         NA         NA         NA          NA
    ## 5         NA         NA         NA         NA         NA         NA          NA
    ## 6         NA         NA         NA         NA         NA         NA          NA
    ##   c1002_7aq11 c1002_7aq12 c1002_7aq13 c1002_7aq14 c1002_78 c1002_79 c1002_80
    ## 1          NA          NA          NA          NA       NA       NA       NA
    ## 2          NA          NA          NA          NA       NA       NA       NA
    ## 3          NA          NA          NA          NA       NA       NA       NA
    ## 4          NA          NA          NA          NA       NA       NA       NA
    ## 5          NA          NA          NA          NA       NA       NA       NA
    ## 6          NA          NA          NA          NA       NA       NA       NA
    ##   c1002_81 c1002_82 c1002_83 c1002_84 c1002_85 c1002_86 c1002_87 c1002_88
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_89 c1002_90 c1002_91 c1002_92 c1002_93 c1002_94 c1002_95 c1002_96
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1002_97 c1002_98 c1002_4aq3 c1002_4aq4 c1002_4aq5 c1002_4aq6 c1002_4aq7
    ## 1       NA       NA         NA         NA         NA         NA         NA
    ## 2       NA       NA         NA         NA         NA         NA         NA
    ## 3       NA       NA         NA         NA         NA         NA         NA
    ## 4       NA       NA         NA         NA         NA         NA         NA
    ## 5       NA       NA         NA         NA         NA         NA         NA
    ## 6       NA       NA         NA         NA         NA         NA         NA
    ##   c1002_4aq8 c1002_7aq15 c1003_4aq1 c1003_4aq2 c1003_4aq3 c1003_4aq4 c1003_4aq5
    ## 1         NA          NA         NA         NA         NA         NA         NA
    ## 2         NA          NA         NA         NA         NA         NA         NA
    ## 3         NA          NA         NA         NA         NA         NA         NA
    ## 4         NA          NA         NA         NA         NA         NA         NA
    ## 5         NA          NA         NA         NA         NA         NA         NA
    ## 6         NA          NA         NA         NA         NA         NA         NA
    ##   c1003_4aq6 c1003_4aq7 c1003_4aq8 c1003_4aq9 c1003_6 c1003_7 c1003_8 c1003_9
    ## 1         NA         NA         NA         NA      NA      NA      NA      NA
    ## 2         NA         NA         NA         NA      NA      NA      NA      NA
    ## 3         NA         NA         NA         NA      NA      NA      NA      NA
    ## 4         NA         NA         NA         NA      NA      NA      NA      NA
    ## 5         NA         NA         NA         NA      NA      NA      NA      NA
    ## 6         NA         NA         NA         NA      NA      NA      NA      NA
    ##   c1003_13 c1003_14 c1003_15 c1004_4aq1 c1004_4aq2 c1004_4aq3 c1004_4aq4
    ## 1       NA       NA       NA         NA         NA         NA         NA
    ## 2       NA       NA       NA         NA         NA         NA         NA
    ## 3       NA       NA       NA         NA         NA         NA         NA
    ## 4       NA       NA       NA         NA         NA         NA         NA
    ## 5       NA       NA       NA         NA         NA         NA         NA
    ## 6       NA       NA       NA         NA         NA         NA         NA
    ##   c1004_4aq5 c1004_4aq6 c1004_1 c1004_2 c1004_3 c1004_4 c1004_5 c1004_6 c1004_7
    ## 1         NA         NA      NA      NA      NA      NA      NA      NA      NA
    ## 2         NA         NA      NA      NA      NA      NA      NA      NA      NA
    ## 3         NA         NA      NA      NA      NA      NA      NA      NA      NA
    ## 4         NA         NA      NA      NA      NA      NA      NA      NA      NA
    ## 5         NA         NA      NA      NA      NA      NA      NA      NA      NA
    ## 6         NA         NA      NA      NA      NA      NA      NA      NA      NA
    ##   c1004_8 c1004_9 c1004_10 c1004_4aq7 c1004_4aq8 c1005_12 c1005_13 c1005_14
    ## 1      NA      NA       NA         NA         NA       NA       NA       NA
    ## 2      NA      NA       NA         NA         NA       NA       NA       NA
    ## 3      NA      NA       NA         NA         NA       NA       NA       NA
    ## 4      NA      NA       NA         NA         NA       NA       NA       NA
    ## 5      NA      NA       NA         NA         NA       NA       NA       NA
    ## 6      NA      NA       NA         NA         NA       NA       NA       NA
    ##   c1005_15 c1005_16 c1005_17 c1005_4aq1 c1005_18 c1005_19 c1005_7aq1 c1005_7aq2
    ## 1       NA       NA       NA         NA                           NA         NA
    ## 2       NA       NA       NA         NA                           NA         NA
    ## 3       NA       NA       NA         NA                           NA         NA
    ## 4       NA       NA       NA         NA                           NA         NA
    ## 5       NA       NA       NA         NA                           NA         NA
    ## 6       NA       NA       NA         NA                           NA         NA
    ##   c1005_7aq3 c1005_7aq4 c1005_7aq5 c1005_7aq6 c1005_7aq7 c1005_7aq8 c1005_7aq9
    ## 1         NA         NA         NA         NA         NA         NA         NA
    ## 2         NA         NA         NA         NA         NA         NA         NA
    ## 3         NA         NA         NA         NA         NA         NA         NA
    ## 4         NA         NA         NA         NA         NA         NA         NA
    ## 5         NA         NA         NA         NA         NA         NA         NA
    ## 6         NA         NA         NA         NA         NA         NA         NA
    ##   c1005_7aq10 c1005_7aq11 c1005_7aq12 c1005_7aq13 c1005_7aq14 c1005_7aq15
    ## 1          NA          NA          NA          NA          NA          NA
    ## 2          NA          NA          NA          NA          NA          NA
    ## 3          NA          NA          NA          NA          NA          NA
    ## 4          NA          NA          NA          NA          NA          NA
    ## 5          NA          NA          NA          NA          NA          NA
    ## 6          NA          NA          NA          NA          NA          NA
    ##   c1005_7aq16 c1005_7aq17 c1005_7aq18 c1005_7aq19 c1005_7aq20 c1005_20 c1005_21
    ## 1          NA          NA          NA          NA          NA       NA       NA
    ## 2          NA          NA          NA          NA          NA       NA       NA
    ## 3          NA          NA          NA          NA          NA       NA       NA
    ## 4          NA          NA          NA          NA          NA       NA       NA
    ## 5          NA          NA          NA          NA          NA       NA       NA
    ## 6          NA          NA          NA          NA          NA       NA       NA
    ##   c1005_22 c1005_23 c1005_24 c1005_25 c1005_26 c1005_27 c1005_28 c1005_29
    ## 1       NA       NA       NA       NA       NA       NA       NA       NA
    ## 2       NA       NA       NA       NA       NA       NA       NA       NA
    ## 3       NA       NA       NA       NA       NA       NA       NA       NA
    ## 4       NA       NA       NA       NA       NA       NA       NA       NA
    ## 5       NA       NA       NA       NA       NA       NA       NA       NA
    ## 6       NA       NA       NA       NA       NA       NA       NA       NA
    ##   c1005_30 c1005_31 c1005_32 c1005_36 c1005_4aq2 c1005_38 c1005_4aq3 c1005_40
    ## 1       NA       NA       NA       NA         NA       NA         NA       NA
    ## 2       NA       NA       NA       NA         NA       NA         NA       NA
    ## 3       NA       NA       NA       NA         NA       NA         NA       NA
    ## 4       NA       NA       NA       NA         NA       NA         NA       NA
    ## 5       NA       NA       NA       NA         NA       NA         NA       NA
    ## 6       NA       NA       NA       NA         NA       NA         NA       NA
    ##   c1005_10aq1 c1005_42 c1005_4aq4 c1005_44 c1005_4aq5 c1005_46 c1005_4aq6
    ## 1          NA       NA         NA       NA         NA       NA         NA
    ## 2          NA       NA         NA       NA         NA       NA         NA
    ## 3          NA       NA         NA       NA         NA       NA         NA
    ## 4          NA       NA         NA       NA         NA       NA         NA
    ## 5          NA       NA         NA       NA         NA       NA         NA
    ## 6          NA       NA         NA       NA         NA       NA         NA
    ##   c1005_48 c1005_4aq7 c1005_4aq8 c1005_4aq9 c1005_4aq10 c1005_4aq11 c1005_4aq12
    ## 1       NA         NA         NA         NA          NA          NA          NA
    ## 2       NA         NA         NA         NA          NA          NA          NA
    ## 3       NA         NA         NA         NA          NA          NA          NA
    ## 4       NA         NA         NA         NA          NA          NA          NA
    ## 5       NA         NA         NA         NA          NA          NA          NA
    ## 6       NA         NA         NA         NA          NA          NA          NA
    ##   c1005_4aq13 c1005_4aq14 c1005_4aq15 c1007_4aq1 c1007_4aq2 c1007_7aq1
    ## 1          NA          NA          NA         NA         NA         NA
    ## 2          NA          NA          NA         NA         NA         NA
    ## 3          NA          NA          NA         NA         NA         NA
    ## 4          NA          NA          NA         NA         NA         NA
    ## 5          NA          NA          NA         NA         NA         NA
    ## 6          NA          NA          NA         NA         NA         NA
    ##   c1007_7aq2 c1007_7aq3 c1007_4aq3 c1007_4aq4 c1007_4aq5 c1007_4aq6 c1007_4aq7
    ## 1         NA         NA         NA         NA         NA         NA         NA
    ## 2         NA         NA         NA         NA         NA         NA         NA
    ## 3         NA         NA         NA         NA         NA         NA         NA
    ## 4         NA         NA         NA         NA         NA         NA         NA
    ## 5         NA         NA         NA         NA         NA         NA         NA
    ## 6         NA         NA         NA         NA         NA         NA         NA
    ##   c1007_4aq8 c1007_4aq9 c1007_4aq10 c1007_4aq11 c1007_4aq12 c1007_4aq13
    ## 1         NA         NA          NA          NA          NA          NA
    ## 2         NA         NA          NA          NA          NA          NA
    ## 3         NA         NA          NA          NA          NA          NA
    ## 4         NA         NA          NA          NA          NA          NA
    ## 5         NA         NA          NA          NA          NA          NA
    ## 6         NA         NA          NA          NA          NA          NA
    ##   c1007_4aq14 c1007_4aq15 c1007_4aq16 c1007_4aq17 c1007_4aq18 c1007_4aq19
    ## 1          NA          NA          NA          NA          NA          NA
    ## 2          NA          NA          NA          NA          NA          NA
    ## 3          NA          NA          NA          NA          NA          NA
    ## 4          NA          NA          NA          NA          NA          NA
    ## 5          NA          NA          NA          NA          NA          NA
    ## 6          NA          NA          NA          NA          NA          NA
    ##   c1007_4aq20 c1007_4aq21 c1007_4aq22 c1007_4aq23 h10_pers_income1
    ## 1          NA          NA          NA          NA               NA
    ## 2          NA          NA          NA          NA               NA
    ## 3          NA          NA          NA          NA               NA
    ## 4          NA          NA          NA          NA               NA
    ## 5          NA          NA          NA          NA               NA
    ## 6          NA          NA          NA          NA               NA
    ##   h10_pers_income2 h10_pers_income3 h10_pers_income4 h10_pers_income5
    ## 1               NA               NA                0               NA
    ## 2               NA               NA                0               NA
    ## 3             1440               NA                0               NA
    ## 4             2400               NA                0               NA
    ## 5               NA               NA                0               NA
    ## 6               NA             3000                0               NA

``` r
tail(welfare)
```

    ##       h10_id h10_ind h10_sn h10_merkey h_new h10_cobf h10_reg5 h10_reg7 h10_din
    ## 16659   9800       7      1   98000701     1       NA        4        5    9764
    ## 16660   9800       7      1   98000701     1       NA        4        5    9764
    ## 16661   9800       7      1   98000701     1       NA        4        5    9764
    ## 16662   9800       7      1   98000701     1       NA        4        5    9764
    ## 16663   9800       7      1   98000701     1       NA        4        5    9764
    ## 16664   9800       7      1   98000701     1       NA        4        5    9764
    ##       h10_cin h10_flag   p10_wgl   p10_wsl   p10_wgc   p10_wsc h10_hc nh1001_1
    ## 16659   11600        0  560.9736 0.1853893  602.8348 0.1992235      1       NA
    ## 16660   11600        0  536.4383 0.1772810  525.2045 0.1735685      1       NA
    ## 16661   11600        0  687.1090 0.2270743  679.6506 0.2246095      1       NA
    ## 16662   11600        0 1262.0316 0.4170735 1248.3326 0.4125463      1       NA
    ## 16663   11600        0  578.4468 0.1911638  572.1679 0.1890888      1       NA
    ## 16664   11600        0  554.8621 0.1833696  548.8392 0.1813792      1       NA
    ##       nh1001_2 h1001_1 h10_pind h10_pid h10_g1 h10_g2 h10_g3 h10_g4 h10_g6
    ## 16659       NA       6        7  980001      1     10      1   1967      5
    ## 16660       NA       6        7  980002      2     20      2   1967      5
    ## 16661       NA       6        7  980003      3     11      2   1992      5
    ## 16662       NA       6        7  980004      4     12      1   1995      5
    ## 16663       NA       6        7  980005      5     13      2   1998      5
    ## 16664       NA       6        7  980006      6     14      1   2001      4
    ##       h10_g7 h10_g8 h10_g9 h10_g10 h10_g11 h10_g12 h1001_110 h1001_5aq1
    ## 16659      5      0      0       1       1       1         5          0
    ## 16660      5      0      0       1       1       1         5          0
    ## 16661      5      0      0       5       1       1         5          0
    ## 16662      5      0      0       5       1       1         5          0
    ## 16663      1      0      0       0       1       1         5          0
    ## 16664      1      0      0       0       1       1         5          0
    ##       h1001_5aq2 h1001_5aq3 h1001_5aq4 h10_med1 h10_med2 h10_med3 h10_med4
    ## 16659          0          0          0        1        2       38        1
    ## 16660          0          0          0        2        1        0        0
    ## 16661          0          0          0        3        1        0        0
    ## 16662          0          0          0        4        1        3        0
    ## 16663          0          0          0        5        1        0        0
    ## 16664          0          0          0        6        1        4        0
    ##       h10_med5 h10_med6 h10_med7 h10_med8 h10_g9_1 h10_med9 h10_med10 h10_eco1
    ## 16659        1        1        2        1        0       30         6        1
    ## 16660        0        0        0        1        0        0         6        2
    ## 16661        0        0        0        0        0        0         2        3
    ## 16662        0        0        2        0        0        0         1        4
    ## 16663        0        0        0        0        0        0         2        5
    ## 16664        0        0        2        0        0       30         2        6
    ##       h10_eco2 h10_eco3 h10_eco4 h10_eco4_1 h10_eco5_1 h10_eco6 h10_eco_7_1
    ## 16659        1       NA        6         NA         NA       NA          NA
    ## 16660        1       NA        9         NA         NA       NA          NA
    ## 16661        1       NA        1         NA          1        2           2
    ## 16662        1       NA        9         NA         NA       NA          NA
    ## 16663        1       NA        9         NA         NA       NA          NA
    ## 16664        0       NA       NA         NA         NA       NA          NA
    ##       h10_eco_7_2 h10_eco_7_3 h10_eco8 h10_eco9 h10_eco10 h10_eco11 h10_soc1
    ## 16659          NA          NA       49      874         1        NA        1
    ## 16660          NA          NA       NA       NA        NA         6        2
    ## 16661           1          NA       21      314         7        NA        3
    ## 16662          NA          NA       NA       NA        NA         5        4
    ## 16663          NA          NA       NA       NA        NA         3        5
    ## 16664          NA          NA       NA       NA        NA        NA        6
    ##       h10_soc_2 h10_soc_3 h10_soc_4 h10_soc_5 h10_soc_6 h10_soc_7 h10_soc_8
    ## 16659         2         1         2         2         1         5         9
    ## 16660         0        NA        NA        NA        NA        NA        NA
    ## 16661         2         1         1         1        NA        NA        NA
    ## 16662         0        NA        NA        NA        NA        NA        NA
    ## 16663         0        NA        NA        NA        NA        NA        NA
    ## 16664         0        NA        NA        NA        NA        NA        NA
    ##       h10_soc_9 h10_soc_10 h10_soc_11 h10_soc8 h10_soc9 h10_soc11 h10_soc10
    ## 16659        NA         NA         NA        0        0         0         0
    ## 16660        NA         NA         NA        0        0         0         0
    ## 16661         0          0         NA        1        1         1         2
    ## 16662        NA         NA         NA        0        0         0         0
    ## 16663        NA         NA         NA        0        0         0         0
    ## 16664        NA         NA         NA        0        0         0         0
    ##       h10_soc_12 h10_soc_13 h1005_1 h1005_3aq1 h1005_2 h1005_3 h1005_4 h1005_5
    ## 16659          4          3       1          2      NA      NA       2      NA
    ## 16660          4          4       1          2      NA      NA       2      NA
    ## 16661          4          1       1          2      NA      NA       2      NA
    ## 16662          4          4       1          2      NA      NA       2      NA
    ## 16663          4          4       1          2      NA      NA       2      NA
    ## 16664          4          4       1          2      NA      NA       2      NA
    ##       h1005_6 h1005_7 nh1005_8 nh1005_9 h1005_3aq2 h1006_aq1 h1006_1 h1006_2
    ## 16659      NA       0        4        4         82         1       5       3
    ## 16660      NA       0        4        4         82         1       5       3
    ## 16661      NA       0        4        4         82         1       5       3
    ## 16662      NA       0        4        4         82         1       5       3
    ## 16663      NA       0        4        4         82         1       5       3
    ## 16664      NA       0        4        4         82         1       5       3
    ##       h1006_4 h1006_5 h1006_3 h1006_6 h1006_8 h1006_9 h1006_aq2 h1006_aq3
    ## 16659       3     112       2    6000       1      88         0         0
    ## 16660       3     112       2    6000       1      88         0         0
    ## 16661       3     112       2    6000       1      88         0         0
    ## 16662       3     112       2    6000       1      88         0         0
    ## 16663       3     112       2    6000       1      88         0         0
    ## 16664       3     112       2    6000       1      88         0         0
    ##       h1006_10 h1006_11 h1006_12 h1006_13 h1006_14 h1006_21 h1006_22 h1006_23
    ## 16659        1        1        1        2        1        1        1        1
    ## 16660        1        1        1        2        1        1        1        1
    ## 16661        1        1        1        2        1        1        1        1
    ## 16662        1        1        1        2        1        1        1        1
    ## 16663        1        1        1        2        1        1        1        1
    ## 16664        1        1        1        2        1        1        1        1
    ##       h1006_24 h1006_25 h1006_27 h1006_30 h1006_33 h1006_36 h1006_39 h1006_3aq1
    ## 16659        1        5        2        2        2        2        2          2
    ## 16660        1        5        2        2        2        2        2          2
    ## 16661        1        5        2        2        2        2        2          2
    ## 16662        1        5        2        2        2        2        2          2
    ## 16663        1        5        2        2        2        2        2          2
    ## 16664        1        5        2        2        2        2        2          2
    ##       h1007_3aq1 h1007_3aq2 h1007_5aq1 h1007_3aq3 h1007_3aq4 h1007_3aq5
    ## 16659        140         72          1          0         22         16
    ## 16660        140         72          1          0         22         16
    ## 16661        140         72          1          0         22         16
    ## 16662        140         72          1          0         22         16
    ## 16663        140         72          1          0         22         16
    ## 16664        140         72          1          0         22         16
    ##       h1007_6aq1 h1007_3aq6 h1007_5aq2 h1007_3aq7 h1007_3aq8 h1007_3aq9
    ## 16659        6.5         19          0         57         19          5
    ## 16660        6.5         19          0         57         19          5
    ## 16661        6.5         19          0         57         19          5
    ## 16662        6.5         19          0         57         19          5
    ## 16663        6.5         19          0         57         19          5
    ## 16664        6.5         19          0         57         19          5
    ##       h1007_3aq10 h1007_3aq11 h1007_5aq3 h1007_5aq4 h1007_3aq13 h1007_6aq4
    ## 16659           0          17         64         37         190         10
    ## 16660           0          17         64         37         190         10
    ## 16661           0          17         64         37         190         10
    ## 16662           0          17         64         37         190         10
    ## 16663           0          17         64         37         190         10
    ## 16664           0          17         64         37         190         10
    ##       h1007_6aq6 h1007_3aq14 h1007_3aq15 h1007_3aq16 h1007_3aq17 h1007_4
    ## 16659          5           0          13           0           0     105
    ## 16660          5           0          13           0           0     105
    ## 16661          5           0          13           0           0     105
    ## 16662          5           0          13           0           0     105
    ## 16663          5           0          13           0           0     105
    ## 16664          5           0          13           0           0     105
    ##       h1007_6aq7 h1007_6aq8 h1007_6aq9 h1007_6aq10 h1007_6aq11 h1007_5
    ## 16659          3        1.2          5        95.8           0      48
    ## 16660          3        1.2          5        95.8           0      48
    ## 16661          3        1.2          5        95.8           0      48
    ## 16662          3        1.2          5        95.8           0      48
    ## 16663          3        1.2          5        95.8           0      48
    ## 16664          3        1.2          5        95.8           0      48
    ##       h1007_6aq12 h1007_6aq13 h1007_6aq14 h1007_9 h1009_9 h1009_6aq4 h10_inc1
    ## 16659        13.7        32.6         1.5     825     500        800        1
    ## 16660        13.7        32.6         1.5     825     500        800        2
    ## 16661        13.7        32.6         1.5     825     500        800        3
    ## 16662        13.7        32.6         1.5     825     500        800        4
    ## 16663        13.7        32.6         1.5     825     500        800        5
    ## 16664        13.7        32.6         1.5     825     500        800        6
    ##       h10_inc2_1 h10_inc2_2 h10_inc3_1 h10_inc3_2 h10_inc4_1 h10_inc4_2
    ## 16659          0         NA          0         NA          1         12
    ## 16660          0         NA          0         NA          0         NA
    ## 16661          1         12          0         NA          0         NA
    ## 16662          0         NA          1          7          0         NA
    ## 16663          0         NA          0         NA          0         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       h10_inc5_1 h10_inc5_2 h10_inc6_1 h10_inc6_2 h10_inc7_1 h10_inc7_2
    ## 16659          0         NA          0         NA          0         NA
    ## 16660          0         NA          0         NA          1         12
    ## 16661          0         NA          0         NA          0         NA
    ## 16662          0         NA          0         NA          1          5
    ## 16663          0         NA          0         NA          1         12
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       h1008_106 h1008_107 h1008_108 h1008_109 h1008_110 h1008_111 h10_inc2_3
    ## 16659         1         1         1         0         0         3          1
    ## 16660         1         1         1         0         0         3          2
    ## 16661         1         1         1         0         0         3          3
    ## 16662         1         1         1         0         0         3          4
    ## 16663         1         1         1         0         0         3          5
    ## 16664         1         1         1         0         0         3          6
    ##       h10_inc2 h10_inc3_6 h10_inc3 h10_inc4_7 h10_inc4 h10_inc4_8 h10_inc4_9
    ## 16659       NA          1       NA          1     7163          1       7163
    ## 16660       NA          2       NA          2       NA          2         NA
    ## 16661     3630          3       NA          3       NA          3         NA
    ## 16662       NA          4      700          4       NA          4         NA
    ## 16663       NA          5       NA          5       NA          5         NA
    ## 16664       NA          6       NA          6       NA          6         NA
    ##       h1008_155 h1008_156 h1008_157 h1008_158 h1008_160 h1008_159 h1008_3aq3
    ## 16659        NA        NA        NA        NA        NA        NA         NA
    ## 16660        NA        NA        NA        NA        NA        NA         NA
    ## 16661        NA        NA        NA        NA        NA        NA         NA
    ## 16662        NA        NA        NA        NA        NA        NA         NA
    ## 16663        NA        NA        NA        NA        NA        NA         NA
    ## 16664        NA        NA        NA        NA        NA        NA         NA
    ##       h1008_161 h1008_162 h1008_163 h1008_164 h1008_166 h1008_165 h1008_3aq4
    ## 16659        NA        NA        NA        NA        NA        NA         NA
    ## 16660        NA        NA        NA        NA        NA        NA         NA
    ## 16661        NA        NA        NA        NA        NA        NA         NA
    ## 16662        NA        NA        NA        NA        NA        NA         NA
    ## 16663        NA        NA        NA        NA        NA        NA         NA
    ## 16664        NA        NA        NA        NA        NA        NA         NA
    ##       h1008_167 h1008_168 h1008_169 h1008_170 h10_inc7_3 h10_inc7 h1008_aq9
    ## 16659        NA        NA        NA        NA          1        0         0
    ## 16660        NA        NA        NA        NA          2        0         0
    ## 16661        NA        NA        NA        NA          3        0         0
    ## 16662        NA        NA        NA        NA          4        0         0
    ## 16663        NA        NA        NA        NA          5        0         0
    ## 16664        NA        NA        NA        NA          6        0         0
    ##       h1008_aq10 h1008_aq11 h1008_aq12 h1008_aq13 h1008_aq14 h1008_aq15
    ## 16659          0          0          0          0          0          0
    ## 16660          0          0          0          0          0          0
    ## 16661          0          0          0          0          0          0
    ## 16662          0          0          0          0          0          0
    ## 16663          0          0          0          0          0          0
    ## 16664          0          0          0          0          0          0
    ##       h1008_6aq1 h1008_aq16 h1008_aq17 h1008_10aq1 h1008_aq19 h1008_aq20
    ## 16659          0          0          0           0          0          0
    ## 16660          0          0          0           0          0          0
    ## 16661          0          0          0           0          0          0
    ## 16662          0          0          0           0          0          0
    ## 16663          0          0          0           0          0          0
    ## 16664          0          0          0           0          0          0
    ##       h1008_aq21 h1008_5aq3 h1008_7aq1 h1008_aq22 h1008_7aq2 h1008_aq23
    ## 16659          0          0          0          0          0          0
    ## 16660          0          0          0          0          0          0
    ## 16661          0          0          0          0          0          0
    ## 16662          0          0          0          0          0          0
    ## 16663          0          0          0          0          0          0
    ## 16664          0          0          0          0          0          0
    ##       h1008_aq24 h1008_4aq116 h1008_4aq117 h1008_5aq1 h1008_7aq4 h1008_7aq5
    ## 16659          0            0            0          0          0          0
    ## 16660          0            0            0          0          0          0
    ## 16661          0            0            0          0          0          0
    ## 16662          0            0            0          0          0          0
    ## 16663          0            0            0          0          0          0
    ## 16664          0            0            0          0          0          0
    ##       h1008_7aq6 h1008_7aq7 h1008_7aq8 h1008_7aq9 h1008_aq25 h1008_7aq10
    ## 16659          0          0          0          0          0           0
    ## 16660          0          0          0          0          0           0
    ## 16661          0          0          0          0          0           0
    ## 16662          0          0          0          0          0           0
    ## 16663          0          0          0          0          0           0
    ## 16664          0          0          0          0          0           0
    ##       h1008_aq26 h1008_aq27 h1008_aq28 h1008_aq29 h1008_3aq5 h1008_4aq118
    ## 16659          0          0          0          0          0            0
    ## 16660          0          0          0          0          0            0
    ## 16661          0          0          0          0          0            0
    ## 16662          0          0          0          0          0            0
    ## 16663          0          0          0          0          0            0
    ## 16664          0          0          0          0          0            0
    ##       h1008_aq30 h1008_6aq3 h1008_3aq6 h1008_3aq7 nh1008_3aq1 h1008_aq32
    ## 16659          4          0          0          0          NA          0
    ## 16660          4          0          0          0          NA          0
    ## 16661          4          0          0          0          NA          0
    ## 16662          4          0          0          0          NA          0
    ## 16663          4          0          0          0          NA          0
    ## 16664          4          0          0          0          NA          0
    ##       h1008_aq33 h1008_aq34 h1008_3aq8 h1008_195 h1008_7aq11 h1009_aq1
    ## 16659          0        107          0         0           0      3000
    ## 16660          0        107          0         0           0      3000
    ## 16661          0        107          0         0           0      3000
    ## 16662          0        107          0         0           0      3000
    ## 16663          0        107          0         0           0      3000
    ## 16664          0        107          0         0           0      3000
    ##       h1009_aq2 h1009_aq3 h1009_aq4 h1009_aq5 h1009_aq6 h1009_aq7 h1009_aq8
    ## 16659         0         0      5000         0         0         0       300
    ## 16660         0         0      5000         0         0         0       300
    ## 16661         0         0      5000         0         0         0       300
    ## 16662         0         0      5000         0         0         0       300
    ## 16663         0         0      5000         0         0         0       300
    ## 16664         0         0      5000         0         0         0       300
    ##       h1010_aq1 h1010_aq2 h1010_aq3 h1010_aq4 h1010_aq5 h1010_aq6 h1010_aq7
    ## 16659     12000         0         0         0         0       500         0
    ## 16660     12000         0         0         0         0       500         0
    ## 16661     12000         0         0         0         0       500         0
    ## 16662     12000         0         0         0         0       500         0
    ## 16663     12000         0         0         0         0       500         0
    ## 16664     12000         0         0         0         0       500         0
    ##       h1010_aq8 h1010_aq9 h1010_aq10 h1010_aq11 h1010_aq12 h1010_aq13
    ## 16659         0         0          0          0          0          0
    ## 16660         0         0          0          0          0          0
    ## 16661         0         0          0          0          0          0
    ## 16662         0         0          0          0          0          0
    ## 16663         0         0          0          0          0          0
    ## 16664         0         0          0          0          0          0
    ##       h1010_aq14 h1010_aq15 h1010_aq16 h1010_aq17 h1010_aq18 h1010_aq19
    ## 16659          0          0          0          0          0          0
    ## 16660          0          0          0          0          0          0
    ## 16661          0          0          0          0          0          0
    ## 16662          0          0          0          0          0          0
    ## 16663          0          0          0          0          0          0
    ## 16664          0          0          0          0          0          0
    ##       h1010_aq20 h1010_26 h1010_27 h1010_aq23 h1010_aq24 h1010_aq25 h1010_aq26
    ## 16659          0        2      250          0       6000          0          0
    ## 16660          0        2      250          0       6000          0          0
    ## 16661          0        2      250          0       6000          0          0
    ## 16662          0        2      250          0       6000          0          0
    ## 16663          0        2      250          0       6000          0          0
    ## 16664          0        2      250          0       6000          0          0
    ##       h1011_2 h1011_3 h1011_4 h1011_5 h1011_6 h1011_7 h1011_8 h1011_3aq1
    ## 16659       2       2       2       2       2       2       2          2
    ## 16660       2       2       2       2       2       2       2          2
    ## 16661       2       2       2       2       2       2       2          2
    ## 16662       2       2       2       2       2       2       2          2
    ## 16663       2       2       2       2       2       2       2          2
    ## 16664       2       2       2       2       2       2       2          2
    ##       h1011_3aq2 h1011_3aq3 h1011_3aq4 h1011_3aq5 h1011_3aq6 h1011_3aq7 h1012_1
    ## 16659          3          3          2         NA          2          2       2
    ## 16660          3          3          2         NA          2          2       2
    ## 16661          3          3          2         NA          2          2       2
    ## 16662          3          3          2         NA          2          2       2
    ## 16663          3          3          2         NA          2          2       2
    ## 16664          3          3          2         NA          2          2       2
    ##       nh1012_1 h1012_2 h1012_3 h1012_4 h1012_5 h1012_6 h1012_7 nh1012_7
    ## 16659       NA      NA      NA      NA      NA      NA       1       NA
    ## 16660       NA      NA      NA      NA      NA      NA       1       NA
    ## 16661       NA      NA      NA      NA      NA      NA       1       NA
    ## 16662       NA      NA      NA      NA      NA      NA       1       NA
    ## 16663       NA      NA      NA      NA      NA      NA       1       NA
    ## 16664       NA      NA      NA      NA      NA      NA       1       NA
    ##       nh1012_8 h1012_9 h1012_10 h1012_11 h1012_12 h1012_13 h1012_14 h1012_15
    ## 16659       NA      NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA      NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA      NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA      NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA      NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA      NA       NA       NA       NA       NA       NA       NA
    ##       h1012_3aq1 h1012_16 h1012_17 h1012_18 h1012_19 h1012_1_4aq1 h1012_1_5aq1
    ## 16659         NA       NA       NA       NA       NA            2            2
    ## 16660         NA       NA       NA       NA       NA            2            2
    ## 16661         NA       NA       NA       NA       NA            2            2
    ## 16662         NA       NA       NA       NA       NA            2            2
    ## 16663         NA       NA       NA       NA       NA            2            2
    ## 16664         NA       NA       NA       NA       NA            2            2
    ##       h1012_1_5aq2 h1012_1_5aq3 h1012_1_5aq4 h1012_1_5aq5 h1012_1_4aq2
    ## 16659           NA           NA           NA           NA            1
    ## 16660           NA           NA           NA           NA            1
    ## 16661           NA           NA           NA           NA            1
    ## 16662           NA           NA           NA           NA            1
    ## 16663           NA           NA           NA           NA            1
    ## 16664           NA           NA           NA           NA            1
    ##       h1012_1_4aq3 h1013_2 h1013_6 h1013_10 h1013_14 h1013_18 h1013_22 h1013_26
    ## 16659            2       2       2        2        2        2        2        2
    ## 16660            2       2       2        2        2        2        2        2
    ## 16661            2       2       2        2        2        2        2        2
    ## 16662            2       2       2        2        2        2        2        2
    ## 16663            2       2       2        2        2        2        2        2
    ## 16664            2       2       2        2        2        2        2        2
    ##       h1013_8aq1 h1013_5aq1 h1013_8aq2 h1013_4aq1 h1013_4aq2 h1013_4aq4
    ## 16659          2          2          2          2         NA         NA
    ## 16660          2          2          2          2         NA         NA
    ## 16661          2          2          2          2         NA         NA
    ## 16662          2          2          2          2         NA         NA
    ## 16663          2          2          2          2         NA         NA
    ## 16664          2          2          2          2         NA         NA
    ##       h1013_4aq6 h1013_4aq8 h1013_4aq10 h1013_5aq4 h1013_5aq6 h1013_5aq8
    ## 16659         NA         NA          NA         NA         NA         NA
    ## 16660         NA         NA          NA         NA         NA         NA
    ## 16661         NA         NA          NA         NA         NA         NA
    ## 16662         NA         NA          NA         NA         NA         NA
    ## 16663         NA         NA          NA         NA         NA         NA
    ## 16664         NA         NA          NA         NA         NA         NA
    ##       h1013_6aq1 h1013_4aq14
    ## 16659         NA          NA
    ## 16660         NA          NA
    ## 16661         NA          NA
    ## 16662         NA          NA
    ## 16663         NA          NA
    ## 16664         NA          NA
    ##                                                                                                                                                                                                                                                           h1013_4aq15
    ## 16659 .                                                                                                                                                                                                                                                              
    ## 16660 .                                                                                                                                                                                                                                                              
    ## 16661 .                                                                                                                                                                                                                                                              
    ## 16662 .                                                                                                                                                                                                                                                              
    ## 16663 .                                                                                                                                                                                                                                                              
    ## 16664 .                                                                                                                                                                                                                                                              
    ##       h1013_4aq16 h1013_4aq17 h1013_4aq18 h1013_4aq20 h1013_4aq22 h1013_4aq24
    ## 16659           2          NA          NA          NA          NA          NA
    ## 16660           2          NA          NA          NA          NA          NA
    ## 16661           2          NA          NA          NA          NA          NA
    ## 16662           2          NA          NA          NA          NA          NA
    ## 16663           2          NA          NA          NA          NA          NA
    ## 16664           2          NA          NA          NA          NA          NA
    ##       h1013_4aq26 h1013_4aq28 h1013_4aq30 h1013_4aq32 h1014_4 h1014_8 h1014_12
    ## 16659          NA          NA          NA          NA      NA      NA       NA
    ## 16660          NA          NA          NA          NA      NA      NA       NA
    ## 16661          NA          NA          NA          NA      NA      NA       NA
    ## 16662          NA          NA          NA          NA      NA      NA       NA
    ## 16663          NA          NA          NA          NA      NA      NA       NA
    ## 16664          NA          NA          NA          NA      NA      NA       NA
    ##       h1014_16 h1014_20 h1014_24 h1014_28 h1014_32 h1014_36 h1014_3aq1
    ## 16659       NA       NA       NA       NA       NA       NA         NA
    ## 16660       NA       NA       NA       NA       NA       NA         NA
    ## 16661       NA       NA       NA       NA       NA       NA         NA
    ## 16662       NA       NA       NA       NA       NA       NA         NA
    ## 16663       NA       NA       NA       NA       NA       NA         NA
    ## 16664       NA       NA       NA       NA       NA       NA         NA
    ##       h1014_4aq1 h1015_4 h1015_8 h1015_12 h1015_20 h1015_25 h1015_29 h1015_33
    ## 16659         NA       2       2        2        2        1        1        2
    ## 16660         NA       2       2        2        2        1        1        2
    ## 16661         NA       2       2        2        2        1        1        2
    ## 16662         NA       2       2        2        2        1        1        2
    ## 16663         NA       2       2        2        2        1        1        2
    ## 16664         NA       2       2        2        2        1        1        2
    ##       h1015_37 h1015_4aq1 h1015_7aq1 h1015_aq1 h1015_40 h1015_41 h1015_42
    ## 16659        2          2          2         2       NA       NA       NA
    ## 16660        2          2          2         2       NA       NA       NA
    ## 16661        2          2          2         2       NA       NA       NA
    ## 16662        2          2          2         2       NA       NA       NA
    ## 16663        2          2          2         2       NA       NA       NA
    ## 16664        2          2          2         2       NA       NA       NA
    ##       h1015_43 h1015_44 h1015_45 h1015_46 h1015_47 h1015_48 h1015_49 h1015_50
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       h1015_51 h1015_52 h1015_53 h1015_54 h1015_55 h1015_56 h1015_57 h1015_60
    ## 16659       NA       NA       NA       NA       NA       NA       NA        5
    ## 16660       NA       NA       NA       NA       NA       NA       NA        5
    ## 16661       NA       NA       NA       NA       NA       NA       NA        5
    ## 16662       NA       NA       NA       NA       NA       NA       NA        5
    ## 16663       NA       NA       NA       NA       NA       NA       NA        5
    ## 16664       NA       NA       NA       NA       NA       NA       NA        5
    ##       h1015_aq2 h1015_61 h1015_62 h1015_63 h1015_66 h1015_67 h1015_68 h1015_aq3
    ## 16659         2       NA       NA       NA       NA       NA        6         2
    ## 16660         2       NA       NA       NA       NA       NA        6         2
    ## 16661         2       NA       NA       NA       NA       NA        6         2
    ## 16662         2       NA       NA       NA       NA       NA        6         2
    ## 16663         2       NA       NA       NA       NA       NA        6         2
    ## 16664         2       NA       NA       NA       NA       NA        6         2
    ##       h1015_69 h1015_70 h1015_71 h1015_74 h1015_75 h1015_76 h1015_aq4 h1015_77
    ## 16659       NA       NA       NA       NA       NA       NA        NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA        NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA        NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA        NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA        NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA        NA       NA
    ##       h1015_78 h1015_79 h1015_82 h1015_83 h1015_84 h1015_aq5 h1015_85 h1015_86
    ## 16659       NA       NA       NA       NA       NA        NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA        NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA        NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA        NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA        NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA        NA       NA       NA
    ##       h1015_87 h1015_90 h1015_91 h1015_92 h1015_aq6 h1015_93 h1015_94 h1015_95
    ## 16659       NA       NA       NA       NA        NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA        NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA        NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA        NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA        NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA        NA       NA       NA       NA
    ##       h1015_98 h1015_99 h1015_100 h1015_aq7 h1015_101 h1015_102 h1015_103
    ## 16659       NA       NA        NA        NA        NA        NA        NA
    ## 16660       NA       NA        NA        NA        NA        NA        NA
    ## 16661       NA       NA        NA        NA        NA        NA        NA
    ## 16662       NA       NA        NA        NA        NA        NA        NA
    ## 16663       NA       NA        NA        NA        NA        NA        NA
    ## 16664       NA       NA        NA        NA        NA        NA        NA
    ##       h1015_106 h1015_107 h1016_6aq1 h1016_6aq4 h1016_8 h1016_24 h1016_4aq1
    ## 16659        NA        NA         NA         NA      NA       NA         NA
    ## 16660        NA        NA         NA         NA      NA       NA         NA
    ## 16661        NA        NA         NA         NA      NA       NA         NA
    ## 16662        NA        NA         NA         NA      NA       NA         NA
    ## 16663        NA        NA         NA         NA      NA       NA         NA
    ## 16664        NA        NA         NA         NA      NA       NA         NA
    ##       h1016_4aq4 h1016_32 h1016_36 h1016_40 h1016_4aq7 h1016_56 h1016_60
    ## 16659         NA       NA       NA       NA         NA       NA       NA
    ## 16660         NA       NA       NA       NA         NA       NA       NA
    ## 16661         NA       NA       NA       NA         NA       NA       NA
    ## 16662         NA       NA       NA       NA         NA       NA       NA
    ## 16663         NA       NA       NA       NA         NA       NA       NA
    ## 16664         NA       NA       NA       NA         NA       NA       NA
    ##       h1016_64 h1017_1 h1017_2 h1017_3 h1017_4 h1017_5 h1017_6 h1017_7 p10_fnum
    ## 16659       NA       0      NA       2       1       4       1       1        1
    ## 16660       NA       0      NA       2       1       4       1       1        2
    ## 16661       NA       0      NA       2       1       4       1       1        3
    ## 16662       NA       0      NA       2       1       4       1       1        4
    ## 16663       NA       0      NA       2       1       4       1       1       NA
    ## 16664       NA       0      NA       2       1       4       1       1       NA
    ##       p10_tq p10_cp p1001_1 p1001_2 p1001_aq1 p1001_3 p1001_4 p1001_5 p1001_6
    ## 16659      3      1       2      NA        NA      NA      NA      NA      NA
    ## 16660      3      1       2      NA        NA      NA      NA      NA      NA
    ## 16661      3      1       2      NA        NA      NA      NA      NA      NA
    ## 16662      4      1       2      NA        NA      NA      NA      NA      NA
    ## 16663     NA     NA      NA      NA        NA      NA      NA      NA      NA
    ## 16664     NA     NA      NA      NA        NA      NA      NA      NA      NA
    ##       p1001_10 p1001_11 p1001_12 p1001_13 p1001_14 p1001_7 p1001_8 p1001_9
    ## 16659       NA       NA       NA       NA       NA      NA      NA      NA
    ## 16660       NA       NA       NA       NA       NA      NA      NA      NA
    ## 16661       NA       NA       NA       NA       NA      NA      NA      NA
    ## 16662       NA       NA       NA       NA       NA      NA      NA      NA
    ## 16663       NA       NA       NA       NA       NA      NA      NA      NA
    ## 16664       NA       NA       NA       NA       NA      NA      NA      NA
    ##       p1001_15 p1001_16 p1001_17 p1001_18 p1001_19 p1001_20 p1001_21 p1001_22
    ## 16659        2       NA       NA       NA       NA        2       NA       NA
    ## 16660        2       NA       NA       NA       NA        2       NA       NA
    ## 16661        2       NA       NA       NA       NA        2       NA       NA
    ## 16662        2       NA       NA       NA       NA        2       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       p1001_23 p1001_24 p1001_25 p1001_27 p1001_28 p1001_29 p1001_30 p1001_31
    ## 16659       NA       NA       NA        2       NA       NA       NA        2
    ## 16660       NA       NA       NA        2       NA       NA       NA        2
    ## 16661       NA       NA       NA        2       NA       NA       NA        2
    ## 16662       NA       NA       NA        2       NA       NA       NA        2
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       p1001_32 p1001_33 p1001_34 p1002_1 p1002_2 p1002_3 p1002_4 p1002_5
    ## 16659       NA       NA       NA       2       2      NA    2003       1
    ## 16660       NA       NA       NA       4      NA      NA      NA      NA
    ## 16661       NA       NA       NA       1       2      NA    2009      11
    ## 16662       NA       NA       NA       4      NA      NA      NA      NA
    ## 16663       NA       NA       NA      NA      NA      NA      NA      NA
    ## 16664       NA       NA       NA      NA      NA      NA      NA      NA
    ##       p1002_6 p1002_7 p1002_8 p1002_8aq1 p1002_9 p1002_8aq2 p1002_4aq1 p1002_10
    ## 16659      12      25      NA         NA      10         NA         NA       NA
    ## 16660      NA      NA      NA         NA      NA         NA         NA        2
    ## 16661      12      20      40      302.5      NA         NA          1       NA
    ## 16662      NA      NA      NA         NA      NA         NA         NA        2
    ## 16663      NA      NA      NA         NA      NA         NA         NA       NA
    ## 16664      NA      NA      NA         NA      NA         NA         NA       NA
    ##       p1002_11 p1002_12 p1002_8aq3 p1002_8aq4 p1002_31 p1002_4aq2 p1002_4aq3
    ## 16659       NA       NA         NA         NA       NA         NA         NA
    ## 16660       NA       NA         NA         NA       NA          2         NA
    ## 16661       NA       NA         NA         NA       NA         NA         NA
    ## 16662       NA       NA         NA         NA       NA          1          2
    ## 16663       NA       NA         NA         NA       NA         NA         NA
    ## 16664       NA       NA         NA         NA       NA         NA         NA
    ##       p1002_4aq4 p1002_8aq5 p1002_8aq6 p1002_8aq7 p1002_8aq8 p1002_8aq9
    ## 16659         NA          0          0          0          0          0
    ## 16660         NA          0          0          0          0          0
    ## 16661         NA          0          0          0          0          0
    ## 16662         NA          0          0          0          0          0
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       p1002_8aq10 p1002_8aq11 p1002_8aq12 p1002_8aq13 p1002_8aq14 p1002_8aq15
    ## 16659           0           0           0           0           0           0
    ## 16660           0           0           0           0           0           0
    ## 16661           0           0           0           0           0           0
    ## 16662           0           0           0           0           0           0
    ## 16663          NA          NA          NA          NA          NA          NA
    ## 16664          NA          NA          NA          NA          NA          NA
    ##       p1002_aq2 p1002_aq3 p1002_aq4 p1002_aq5 p1003_1 p1003_2 p1003_5 p1003_6
    ## 16659         0        NA        NA        NA       1       2       4       3
    ## 16660         0        NA        NA        NA       1       0       4       3
    ## 16661         0        NA        NA        NA       1       2       4       4
    ## 16662        NA        NA        NA        NA       1       2       4       3
    ## 16663        NA        NA        NA        NA      NA      NA      NA      NA
    ## 16664        NA        NA        NA        NA      NA      NA      NA      NA
    ##       p1003_7 p1003_8 p1003_9 p1003_10 p1003_11 p1003_12 p1004_1 p1004_2
    ## 16659       4       4       4        4        4        4       1       4
    ## 16660       4       4       4        4        4        4       1       4
    ## 16661       4       4       3        4        4        4       1       4
    ## 16662       4       4       2        4        4        4       1       4
    ## 16663      NA      NA      NA       NA       NA       NA      NA      NA
    ## 16664      NA      NA      NA       NA       NA       NA      NA      NA
    ##       p1004_3 p1004_4 p1004_5 p1004_6 p1004_7 p1004_8 p1004_9 p1004_10 p1004_11
    ## 16659       2       2      NA      NA       1      NA       1       NA        2
    ## 16660       2       2      NA      NA       1      NA      NA        4        7
    ## 16661       2       2      NA      NA       2      NA      NA       NA       NA
    ## 16662       2       2      NA      NA       2      NA      NA       NA       NA
    ## 16663      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16664      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ##       p1004_12 p1004_13 p1004_3aq1 p1004_3aq2 p1004_3aq3 p1004_3aq4 p1004_3aq5
    ## 16659       NA       NA          4          4          4          4          2
    ## 16660       NA       NA          4          4          3          3          3
    ## 16661       NA       NA          4          4          2          2          4
    ## 16662       NA       NA          4          4          2          2          4
    ## 16663       NA       NA         NA         NA         NA         NA         NA
    ## 16664       NA       NA         NA         NA         NA         NA         NA
    ##       p1004_3aq6 p1004_3aq7 p1004_3aq8 p1005_3aq1 p1005_3aq2 p1005_3aq3
    ## 16659          3          2          2         NA         NA         NA
    ## 16660          4          2          2         NA         NA         NA
    ## 16661          2          2          2         NA         NA         NA
    ## 16662          2          2          2          3         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       p1005_3aq4 p1005_3aq5 p1005_3aq6 p1005_3aq7 p1005_3aq8 p1005_3aq9
    ## 16659         NA          2         NA         NA         NA          1
    ## 16660         NA          2         NA         NA         NA          1
    ## 16661         NA          2         NA         NA         NA          1
    ## 16662         NA          2         NA         NA         NA          1
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       p1005_3aq10 p1005_2 p1005_3 p1005_4aq1 p1005_4aq2 p1005_4aq3 p1005_4aq4
    ## 16659          NA       3       5          4          1          1          1
    ## 16660          NA       2       2          1          1          1          1
    ## 16661          NA       2       4          3          1          1          1
    ## 16662          NA       2       4          3          1          1          1
    ## 16663          NA      NA      NA         NA         NA         NA         NA
    ## 16664          NA      NA      NA         NA         NA         NA         NA
    ##       p1005_4aq5 p1005_4aq6 p1005_4aq7 p1005_4aq8 p1005_5 p1005_6 p1005_7
    ## 16659          1          1          1          1       2       2       2
    ## 16660          1          1          1          1       2       2       2
    ## 16661          1          1          1          1       2       2       2
    ## 16662          1          1          1          1       2       2       2
    ## 16663         NA         NA         NA         NA      NA      NA      NA
    ## 16664         NA         NA         NA         NA      NA      NA      NA
    ##       p1005_8 p1005_3aq11 p1005_9 p1005_10 p1005_11 p1005_12 p1005_13 p1005_14
    ## 16659       2           0       1        4        1        1        1        1
    ## 16660       2           2       1        4        1        1        1        1
    ## 16661       2           2       1        4        1        1        1        1
    ## 16662       2           0       1        4        1        1        1        1
    ## 16663      NA          NA      NA       NA       NA       NA       NA       NA
    ## 16664      NA          NA      NA       NA       NA       NA       NA       NA
    ##       p1005_15 p1005_16 p1005_17 p1005_18 p1005_19 p1005_20 p1005_21 p1005_22
    ## 16659        4        1        1        1        1        3        3        1
    ## 16660        4        1        1        1        1        3        3        1
    ## 16661        4        1        1        1        1        3        3        1
    ## 16662        4        1        1        1        1        3        3        1
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       p1005_23 p1005_24 p1005_25 p1005_26 p1005_27 p1005_28 p1005_29 p1005_4aq9
    ## 16659        4        1        3        3        1        1        1          1
    ## 16660        4        1        4        4        1        1        1          1
    ## 16661        3        1        4        4        1        1        1          0
    ## 16662        3        1        3        3        1        1        1          0
    ## 16663       NA       NA       NA       NA       NA       NA       NA         NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA         NA
    ##       p1005_4aq10 p1005_4aq11 p1005_aq1 p1005_aq2 p1005_aq3 p1005_aq4
    ## 16659           1           1         6         6         6         6
    ## 16660           1           1         6         6         6         6
    ## 16661           0           0         6         0         0         0
    ## 16662           0           0         6         0         0         0
    ## 16663          NA          NA        NA        NA        NA        NA
    ## 16664          NA          NA        NA        NA        NA        NA
    ##       p1005_6aq1 p1005_6aq2 p1005_6aq3 p1005_6aq4 p1005_6aq5 p1005_6aq6
    ## 16659         NA         NA         NA         NA         NA         NA
    ## 16660         NA         NA         NA         NA         NA         NA
    ## 16661         NA         NA         NA         NA         NA         NA
    ## 16662          2         NA         NA          2         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       p1005_6aq7 p1005_6aq8 p1005_6aq9 p1005_7aq1 p1005_7aq2 p1005_7aq3
    ## 16659         NA         NA         NA          2          2          2
    ## 16660         NA         NA         NA          2          2          2
    ## 16661         NA         NA         NA          2          2          2
    ## 16662          2         NA         NA         NA         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       p1007_3aq1 p1007_3aq2 p1007_3aq3 p1007_3aq4 p1007_3aq5 p1007_3aq6
    ## 16659         NA         NA         NA         NA         NA         NA
    ## 16660         NA         NA         NA         NA         NA         NA
    ## 16661         NA         NA         NA         NA         NA         NA
    ## 16662          2         12         33      33310         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       p1007_3aq7 np1006_1 np1006_2 np1006_3 np1006_4 np1006_5 np1006_6 np1006_7
    ## 16659         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662         NA        2        3        1     2014     2014        1     2014
    ## 16663         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664         NA       NA       NA       NA       NA       NA       NA       NA
    ##       np1006_8 np1006_9 np1006_10 np1006_11 np1006_12 np1006_13 np1006_14
    ## 16659       NA       NA        NA        NA        NA        NA        NA
    ## 16660       NA       NA        NA        NA        NA        NA        NA
    ## 16661       NA       NA        NA        NA        NA        NA        NA
    ## 16662     2014        1        88        88        88        88        88
    ## 16663       NA       NA        NA        NA        NA        NA        NA
    ## 16664       NA       NA        NA        NA        NA        NA        NA
    ##       np1006_15 np1006_16 np1006_17 np1006_18 np1006_19 np1006_20 np1006_21
    ## 16659        NA        NA        NA        NA        NA        NA        NA
    ## 16660        NA        NA        NA        NA        NA        NA        NA
    ## 16661        NA        NA        NA        NA        NA        NA        NA
    ## 16662        88        88        88        88        88        88        88
    ## 16663        NA        NA        NA        NA        NA        NA        NA
    ## 16664        NA        NA        NA        NA        NA        NA        NA
    ##       np1006_22 np1006_23 np1006_24 np1006_25 np1006_26 np1006_27 np1006_28
    ## 16659        NA        NA        NA        NA        NA        NA        NA
    ## 16660        NA        NA        NA        NA        NA        NA        NA
    ## 16661        NA        NA        NA        NA        NA        NA        NA
    ## 16662        88        88        88         0        NA        NA        NA
    ## 16663        NA        NA        NA        NA        NA        NA        NA
    ## 16664        NA        NA        NA        NA        NA        NA        NA
    ##       np1006_29 np1006_30 np1006_31 np1006_32 np1006_33 np1006_34 np1006_35
    ## 16659        NA        NA        NA        NA        NA        NA        NA
    ## 16660        NA        NA        NA        NA        NA        NA        NA
    ## 16661        NA        NA        NA        NA        NA        NA        NA
    ## 16662        NA        NA         1        NA         1        NA         1
    ## 16663        NA        NA        NA        NA        NA        NA        NA
    ## 16664        NA        NA        NA        NA        NA        NA        NA
    ##       np1006_36 np1006_37 np1006_38 np1006_39 np1006_40 np1006_41 np1006_42
    ## 16659        NA        NA        NA        NA        NA        NA        NA
    ## 16660        NA        NA        NA        NA        NA        NA        NA
    ## 16661        NA        NA        NA        NA        NA        NA        NA
    ## 16662        NA         1        NA         5         5        10        13
    ## 16663        NA        NA        NA        NA        NA        NA        NA
    ## 16664        NA        NA        NA        NA        NA        NA        NA
    ##       np1006_43 np1006_44 p1006_3aq1 c10_fnum c10_cp c10_grade c1001_1 c1001_2
    ## 16659        NA        NA         NA       NA     NA        NA      NA      NA
    ## 16660        NA        NA         NA       NA     NA        NA      NA      NA
    ## 16661        NA        NA         NA       NA     NA        NA      NA      NA
    ## 16662         2        NA          2       NA     NA        NA      NA      NA
    ## 16663        NA        NA         NA       NA     NA        NA      NA      NA
    ## 16664        NA        NA         NA       NA     NA        NA      NA      NA
    ##       c1001_3 c1001_4 c1001_5 c1001_6 c1001_7 c1001_8 c1001_9 c1001_10 c1001_11
    ## 16659      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16660      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16661      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16662      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16663      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16664      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ##       c1001_12 c1001_13 c1001_7aq5 c1001_7aq6 c1001_7aq7 c1001_7aq8 c1001_4aq1
    ## 16659       NA       NA         NA         NA         NA         NA         NA
    ## 16660       NA       NA         NA         NA         NA         NA         NA
    ## 16661       NA       NA         NA         NA         NA         NA         NA
    ## 16662       NA       NA         NA         NA         NA         NA         NA
    ## 16663       NA       NA         NA         NA         NA         NA         NA
    ## 16664       NA       NA         NA         NA         NA         NA         NA
    ##       c1001_4aq2 c1001_4aq3 c1001_4aq4 c1001_4aq5 c1001_4aq6 c1002_1 c1002_2
    ## 16659         NA         NA         NA         NA         NA      NA      NA
    ## 16660         NA         NA         NA         NA         NA      NA      NA
    ## 16661         NA         NA         NA         NA         NA      NA      NA
    ## 16662         NA         NA         NA         NA         NA      NA      NA
    ## 16663         NA         NA         NA         NA         NA      NA      NA
    ## 16664         NA         NA         NA         NA         NA      NA      NA
    ##       c1002_3 c1002_4 c1002_5 c1002_6 c1002_7 c1002_8 c1002_9 c1002_10 c1002_11
    ## 16659      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16660      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16661      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16662      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16663      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ## 16664      NA      NA      NA      NA      NA      NA      NA       NA       NA
    ##       c1002_12 c1002_13 c1002_14 c1002_15 c1002_16 c1002_17 c1002_18 c1002_19
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_20 c1002_21 c1002_22 c1002_23 c1002_24 c1002_25 c1002_26 c1002_4aq1
    ## 16659       NA       NA       NA       NA       NA       NA       NA         NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA         NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA         NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA         NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA         NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA         NA
    ##       c1002_27 c1002_28 c1002_29 c1002_30 c1002_31 c1002_32 c1002_33 c1002_34
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_35 c1002_36 c1002_37 c1002_38 c1002_39 c1002_40 c1002_41 c1002_42
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_43 c1002_44 c1002_45 c1002_46 c1002_47 c1002_48 c1002_49 c1002_50
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_51 c1002_52 c1002_53 c1002_54 c1002_55 c1002_56 c1002_57 c1002_58
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_4aq2 c1002_59 c1002_60 c1002_61 c1002_62 c1002_63 c1002_64 c1002_65
    ## 16659         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663         NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664         NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_66 c1002_67 c1002_68 c1002_69 c1002_70 c1002_71 c1002_72 c1002_73
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_74 c1002_75 c1002_76 c1002_77 c1002_7aq1 c1002_7aq2 c1002_7aq3
    ## 16659       NA       NA       NA       NA         NA         NA         NA
    ## 16660       NA       NA       NA       NA         NA         NA         NA
    ## 16661       NA       NA       NA       NA         NA         NA         NA
    ## 16662       NA       NA       NA       NA         NA         NA         NA
    ## 16663       NA       NA       NA       NA         NA         NA         NA
    ## 16664       NA       NA       NA       NA         NA         NA         NA
    ##       c1002_7aq4 c1002_7aq5 c1002_7aq6 c1002_7aq7 c1002_7aq8 c1002_7aq9
    ## 16659         NA         NA         NA         NA         NA         NA
    ## 16660         NA         NA         NA         NA         NA         NA
    ## 16661         NA         NA         NA         NA         NA         NA
    ## 16662         NA         NA         NA         NA         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       c1002_7aq10 c1002_7aq11 c1002_7aq12 c1002_7aq13 c1002_7aq14 c1002_78
    ## 16659          NA          NA          NA          NA          NA       NA
    ## 16660          NA          NA          NA          NA          NA       NA
    ## 16661          NA          NA          NA          NA          NA       NA
    ## 16662          NA          NA          NA          NA          NA       NA
    ## 16663          NA          NA          NA          NA          NA       NA
    ## 16664          NA          NA          NA          NA          NA       NA
    ##       c1002_79 c1002_80 c1002_81 c1002_82 c1002_83 c1002_84 c1002_85 c1002_86
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_87 c1002_88 c1002_89 c1002_90 c1002_91 c1002_92 c1002_93 c1002_94
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1002_95 c1002_96 c1002_97 c1002_98 c1002_4aq3 c1002_4aq4 c1002_4aq5
    ## 16659       NA       NA       NA       NA         NA         NA         NA
    ## 16660       NA       NA       NA       NA         NA         NA         NA
    ## 16661       NA       NA       NA       NA         NA         NA         NA
    ## 16662       NA       NA       NA       NA         NA         NA         NA
    ## 16663       NA       NA       NA       NA         NA         NA         NA
    ## 16664       NA       NA       NA       NA         NA         NA         NA
    ##       c1002_4aq6 c1002_4aq7 c1002_4aq8 c1002_7aq15 c1003_4aq1 c1003_4aq2
    ## 16659         NA         NA         NA          NA         NA         NA
    ## 16660         NA         NA         NA          NA         NA         NA
    ## 16661         NA         NA         NA          NA         NA         NA
    ## 16662         NA         NA         NA          NA         NA         NA
    ## 16663         NA         NA         NA          NA         NA         NA
    ## 16664         NA         NA         NA          NA         NA         NA
    ##       c1003_4aq3 c1003_4aq4 c1003_4aq5 c1003_4aq6 c1003_4aq7 c1003_4aq8
    ## 16659         NA         NA         NA         NA         NA         NA
    ## 16660         NA         NA         NA         NA         NA         NA
    ## 16661         NA         NA         NA         NA         NA         NA
    ## 16662         NA         NA         NA         NA         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       c1003_4aq9 c1003_6 c1003_7 c1003_8 c1003_9 c1003_13 c1003_14 c1003_15
    ## 16659         NA      NA      NA      NA      NA       NA       NA       NA
    ## 16660         NA      NA      NA      NA      NA       NA       NA       NA
    ## 16661         NA      NA      NA      NA      NA       NA       NA       NA
    ## 16662         NA      NA      NA      NA      NA       NA       NA       NA
    ## 16663         NA      NA      NA      NA      NA       NA       NA       NA
    ## 16664         NA      NA      NA      NA      NA       NA       NA       NA
    ##       c1004_4aq1 c1004_4aq2 c1004_4aq3 c1004_4aq4 c1004_4aq5 c1004_4aq6 c1004_1
    ## 16659         NA         NA         NA         NA         NA         NA      NA
    ## 16660         NA         NA         NA         NA         NA         NA      NA
    ## 16661         NA         NA         NA         NA         NA         NA      NA
    ## 16662         NA         NA         NA         NA         NA         NA      NA
    ## 16663         NA         NA         NA         NA         NA         NA      NA
    ## 16664         NA         NA         NA         NA         NA         NA      NA
    ##       c1004_2 c1004_3 c1004_4 c1004_5 c1004_6 c1004_7 c1004_8 c1004_9 c1004_10
    ## 16659      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 16660      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 16661      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 16662      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 16663      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ## 16664      NA      NA      NA      NA      NA      NA      NA      NA       NA
    ##       c1004_4aq7 c1004_4aq8 c1005_12 c1005_13 c1005_14 c1005_15 c1005_16
    ## 16659         NA         NA       NA       NA       NA       NA       NA
    ## 16660         NA         NA       NA       NA       NA       NA       NA
    ## 16661         NA         NA       NA       NA       NA       NA       NA
    ## 16662         NA         NA       NA       NA       NA       NA       NA
    ## 16663         NA         NA       NA       NA       NA       NA       NA
    ## 16664         NA         NA       NA       NA       NA       NA       NA
    ##       c1005_17 c1005_4aq1 c1005_18 c1005_19 c1005_7aq1 c1005_7aq2 c1005_7aq3
    ## 16659       NA         NA                           NA         NA         NA
    ## 16660       NA         NA                           NA         NA         NA
    ## 16661       NA         NA                           NA         NA         NA
    ## 16662       NA         NA                           NA         NA         NA
    ## 16663       NA         NA                           NA         NA         NA
    ## 16664       NA         NA                           NA         NA         NA
    ##       c1005_7aq4 c1005_7aq5 c1005_7aq6 c1005_7aq7 c1005_7aq8 c1005_7aq9
    ## 16659         NA         NA         NA         NA         NA         NA
    ## 16660         NA         NA         NA         NA         NA         NA
    ## 16661         NA         NA         NA         NA         NA         NA
    ## 16662         NA         NA         NA         NA         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       c1005_7aq10 c1005_7aq11 c1005_7aq12 c1005_7aq13 c1005_7aq14 c1005_7aq15
    ## 16659          NA          NA          NA          NA          NA          NA
    ## 16660          NA          NA          NA          NA          NA          NA
    ## 16661          NA          NA          NA          NA          NA          NA
    ## 16662          NA          NA          NA          NA          NA          NA
    ## 16663          NA          NA          NA          NA          NA          NA
    ## 16664          NA          NA          NA          NA          NA          NA
    ##       c1005_7aq16 c1005_7aq17 c1005_7aq18 c1005_7aq19 c1005_7aq20 c1005_20
    ## 16659          NA          NA          NA          NA          NA       NA
    ## 16660          NA          NA          NA          NA          NA       NA
    ## 16661          NA          NA          NA          NA          NA       NA
    ## 16662          NA          NA          NA          NA          NA       NA
    ## 16663          NA          NA          NA          NA          NA       NA
    ## 16664          NA          NA          NA          NA          NA       NA
    ##       c1005_21 c1005_22 c1005_23 c1005_24 c1005_25 c1005_26 c1005_27 c1005_28
    ## 16659       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16660       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16661       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16662       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16663       NA       NA       NA       NA       NA       NA       NA       NA
    ## 16664       NA       NA       NA       NA       NA       NA       NA       NA
    ##       c1005_29 c1005_30 c1005_31 c1005_32 c1005_36 c1005_4aq2 c1005_38
    ## 16659       NA       NA       NA       NA       NA         NA       NA
    ## 16660       NA       NA       NA       NA       NA         NA       NA
    ## 16661       NA       NA       NA       NA       NA         NA       NA
    ## 16662       NA       NA       NA       NA       NA         NA       NA
    ## 16663       NA       NA       NA       NA       NA         NA       NA
    ## 16664       NA       NA       NA       NA       NA         NA       NA
    ##       c1005_4aq3 c1005_40 c1005_10aq1 c1005_42 c1005_4aq4 c1005_44 c1005_4aq5
    ## 16659         NA       NA          NA       NA         NA       NA         NA
    ## 16660         NA       NA          NA       NA         NA       NA         NA
    ## 16661         NA       NA          NA       NA         NA       NA         NA
    ## 16662         NA       NA          NA       NA         NA       NA         NA
    ## 16663         NA       NA          NA       NA         NA       NA         NA
    ## 16664         NA       NA          NA       NA         NA       NA         NA
    ##       c1005_46 c1005_4aq6 c1005_48 c1005_4aq7 c1005_4aq8 c1005_4aq9 c1005_4aq10
    ## 16659       NA         NA       NA         NA         NA         NA          NA
    ## 16660       NA         NA       NA         NA         NA         NA          NA
    ## 16661       NA         NA       NA         NA         NA         NA          NA
    ## 16662       NA         NA       NA         NA         NA         NA          NA
    ## 16663       NA         NA       NA         NA         NA         NA          NA
    ## 16664       NA         NA       NA         NA         NA         NA          NA
    ##       c1005_4aq11 c1005_4aq12 c1005_4aq13 c1005_4aq14 c1005_4aq15 c1007_4aq1
    ## 16659          NA          NA          NA          NA          NA         NA
    ## 16660          NA          NA          NA          NA          NA         NA
    ## 16661          NA          NA          NA          NA          NA         NA
    ## 16662          NA          NA          NA          NA          NA         NA
    ## 16663          NA          NA          NA          NA          NA         NA
    ## 16664          NA          NA          NA          NA          NA         NA
    ##       c1007_4aq2 c1007_7aq1 c1007_7aq2 c1007_7aq3 c1007_4aq3 c1007_4aq4
    ## 16659         NA         NA         NA         NA         NA         NA
    ## 16660         NA         NA         NA         NA         NA         NA
    ## 16661         NA         NA         NA         NA         NA         NA
    ## 16662         NA         NA         NA         NA         NA         NA
    ## 16663         NA         NA         NA         NA         NA         NA
    ## 16664         NA         NA         NA         NA         NA         NA
    ##       c1007_4aq5 c1007_4aq6 c1007_4aq7 c1007_4aq8 c1007_4aq9 c1007_4aq10
    ## 16659         NA         NA         NA         NA         NA          NA
    ## 16660         NA         NA         NA         NA         NA          NA
    ## 16661         NA         NA         NA         NA         NA          NA
    ## 16662         NA         NA         NA         NA         NA          NA
    ## 16663         NA         NA         NA         NA         NA          NA
    ## 16664         NA         NA         NA         NA         NA          NA
    ##       c1007_4aq11 c1007_4aq12 c1007_4aq13 c1007_4aq14 c1007_4aq15 c1007_4aq16
    ## 16659          NA          NA          NA          NA          NA          NA
    ## 16660          NA          NA          NA          NA          NA          NA
    ## 16661          NA          NA          NA          NA          NA          NA
    ## 16662          NA          NA          NA          NA          NA          NA
    ## 16663          NA          NA          NA          NA          NA          NA
    ## 16664          NA          NA          NA          NA          NA          NA
    ##       c1007_4aq17 c1007_4aq18 c1007_4aq19 c1007_4aq20 c1007_4aq21 c1007_4aq22
    ## 16659          NA          NA          NA          NA          NA          NA
    ## 16660          NA          NA          NA          NA          NA          NA
    ## 16661          NA          NA          NA          NA          NA          NA
    ## 16662          NA          NA          NA          NA          NA          NA
    ## 16663          NA          NA          NA          NA          NA          NA
    ## 16664          NA          NA          NA          NA          NA          NA
    ##       c1007_4aq23 h10_pers_income1 h10_pers_income2 h10_pers_income3
    ## 16659          NA               NA               NA             7163
    ## 16660          NA               NA               NA               NA
    ## 16661          NA             3630               NA               NA
    ## 16662          NA               NA              700               NA
    ## 16663          NA               NA               NA               NA
    ## 16664          NA               NA               NA               NA
    ##       h10_pers_income4 h10_pers_income5
    ## 16659                0               NA
    ## 16660                0               NA
    ## 16661                0               NA
    ## 16662                0               NA
    ## 16663                0               NA
    ## 16664                0               NA

#### 5\. 변수명 바꾸기

``` r
welfare <- rename(welfare,
                  sex = h10_g3,
                  birth = h10_g4,
                  marriage = h10_g10,
                  religion = h10_g11,
                  income = p1002_8aq1,
                  code_job = h10_eco9,
                  code_region = h10_reg7)
```

### 데이터 분석 절차

  - 1단계. 변수 검토 및 전처리 가장 먼저 분석에 사용할 변수들을 전처리 한다., 변수의 특성을 파악하고 이상치를 정제한
    다음 파생변수르르 만든다. 전처리는 분석에 활용할 변수 각각에 대해 실시

  - 2단계. 변수 간 관계 분석 전처리가 완료되면 본격적으로 변수 간 관계를 파악하는 분석을 함 데이터를 요약한 표를 만든후
    분석 결과를 쉽게 이해할 수 있는 그래프 만듬 ![](img/09_01.png)
