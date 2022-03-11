# DOSPERT scale overview

DOSPERT has five risk domains which are used for variable names. 

Note that new DOSPERT(2020) only uses risk taking(RT) scale, excluding risk benefit(RB) and risk perception(RP). 

- Risk domains are abbreviated as follows:
    + financial: *fin*
    + health: *hea* 
    + recreational: *rec*
    + ethical: *eth*
    + social: *soc*

# Data cleaning
Data for analysis is raw data downloaded from Qualtrics. There is no need to manipulate the variable names before using any of the functions. However, for the responses of the dospert questions, variable names should be in "(domain)_Question number" (e. g. fin_1) format. 




```r
df <- read.csv("./DOSPERT(2020)_test.csv", header = TRUE, stringsAsFactors=FALSE) 
```

```
##                                              StartDate                                            EndDate                Status
## 3                                  2022-03-10 15:41:44                                2022-03-10 15:41:44                     2
## 4                                  2022-03-10 15:41:44                                2022-03-10 15:41:44                     2
## 5                                  2022-03-10 15:41:44                                2022-03-10 15:41:44                     2
## 6                                  2022-03-10 15:41:44                                2022-03-10 15:41:44                     2
##                 IPAddress                Progress   Duration..in.seconds.                Finished
## 1               IP Address                Progress   Duration (in seconds)                Finished
## 2 {"ImportId":"ipAddress"} {"ImportId":"progress"} {"ImportId":"duration"} {"ImportId":"finished"}
## 3                                              100                       0                       1
## 4                                              100                       0                       1
## 5                                              100                       0                       1
## 6                                              100                       0                       1
```

```r
df <- df[-c(1,2),] # exclude first two rows
score_num <- as.data.frame(apply((df[,c(18:47)]), 2, as.numeric))
df <- cbind(df$ResponseId, score_num)
```

```
          ResponseId eth_1 eth_2 eth_3 eth_4 eth_5 eth_6 fin_1 fin_2 fin_3 fin_4 fin_5 fin_6 hea_1 hea_2 hea_3 hea_4 hea_5 hea_6 rec_1
1  R_esd1kyBKHNuVgOi     4     3     5     3     3     6     1     2     7     5     2     5     5     1     6     3     6     3     1
2  R_0qS2WIn2azOzoYm     1     3     5     4     2     4     6     6     3     3     3     3     7     3     4     5     6     5     4
3  R_eDnu4YBcBk32BIa     7     7     7     3     2     3     7     6     5     4     1     1     5     2     7     7     1     5     2
4  R_9ysdW0tzLki3NeS     4     3     7     4     3     6     5     7     1     5     6     2     5     5     3     6     6     3     6
5  R_cBxSpN13lHnVM5E     6     3     5     2     7     1     1     6     1     2     7     1     7     7     6     5     6     4     6
6  R_4UistUDzw71os7k     7     4     5     6     1     6     7     3     4     2     3     1     2     7     4     2     5     7     4
7  R_ebyOHIQst9lwt9k     6     3     2     2     1     5     1     1     5     2     3     1     6     5     5     3     5     7     7
8  R_7UtywWeSjur8Xbw     4     7     5     2     2     3     6     7     1     5     1     2     6     1     7     7     1     2     5
9  R_6ShyWDxbRqVeMjI     7     6     2     7     5     1     6     3     5     5     1     4     3     3     2     7     4     4     7
10 R_4Z2wSXIWAmLcfcO     4     3     6     5     5     1     4     2     4     7     6     4     3     7     7     2     3     1     7
   rec_2 rec_3 rec_4 rec_5 rec_6 soc_1 soc_2 soc_3 soc_4 soc_5 soc_6
1      5     7     4     2     1     5     5     3     2     6     6
2      2     7     7     7     1     7     2     2     3     4     1
3      3     2     7     4     4     2     6     6     2     1     2
4      1     1     7     4     5     2     1     5     3     7     3
5      5     4     5     4     7     3     5     7     3     6     6
6      2     5     4     5     7     4     3     1     5     6     1
7      3     4     1     2     1     5     5     4     6     4     1
8      3     1     5     5     2     4     2     2     7     3     5
9      1     7     3     5     5     5     1     5     3     6     5
10     2     7     1     5     4     1     4     1     6     4     4
```

# Calculate domain-general and domain-specific risk preference
Risk-taking preference can be calculated by sum of all domains(domain-general risk taking) and sum of each domain(domain-specific risk taking).

```r
df1$sum_eth = rowSums(df1[,c("eth_1", "eth_2", "eth_3", "eth_4", "eth_5", "eth_6")])
df1$sum_fin = rowSums(df1[,c("fin_1", "fin_2", "fin_3", "fin_4", "fin_5", "fin_6")])
df1$sum_hea = rowSums(df1[,c("hea_1", "hea_2", "hea_3", "hea_4", "hea_5", "hea_6")])
df1$sum_rec = rowSums(df1[,c("rec_1", "rec_2", "rec_3", "rec_4", "rec_5", "rec_6")])
df1$sum_soc = rowSums(df1[,c("soc_1", "soc_2", "soc_3", "soc_4", "soc_5", "soc_6")])
df1$sum_tot = rowSums(df1[,c("sum_eth", "sum_fin", "sum_hea", "sum_rec", "sum_soc")])

df_score <- cbind(df$ResponseId, temp) #extract only responseId and scores
```


