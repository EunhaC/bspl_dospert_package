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
# Read the raw file downloaded from Qualtrics
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
# Clean the data 
df <- df[-c(1,2),] # exclude first two rows
score_num <- as.data.frame(apply((df[,c(18:47)]), 2, as.numeric)) # change character to numeric
df <- cbind(df$ResponseId, score_num)
names(df)[names(df) == "df$ResponseId"] <- "ID"

```

```
##                  ID eth_1 eth_2 eth_3 eth_4
## 1 R_esd1kyBKHNuVgOi     4     3     5     3
## 2 R_0qS2WIn2azOzoYm     1     3     5     4
## 3 R_eDnu4YBcBk32BIa     7     7     7     3
## 4 R_9ysdW0tzLki3NeS     4     3     7     4
## 5 R_cBxSpN13lHnVM5E     6     3     5     2
## 6 R_4UistUDzw71os7k     7     4     5     6
```

# Calculate domain-general and domain-specific risk attitude
Risk-taking attitude can be calculated by sum of all domains(domain-general risk taking, "sub_tot" column) and sum of each domain(domain-specific risk taking, "sub_xxx" column).

```r
# Calculate the scores and extract the data
df1$sum_eth = rowSums(df1[,c("eth_1", "eth_2", "eth_3", "eth_4", "eth_5", "eth_6")])
df1$sum_fin = rowSums(df1[,c("fin_1", "fin_2", "fin_3", "fin_4", "fin_5", "fin_6")])
df1$sum_hea = rowSums(df1[,c("hea_1", "hea_2", "hea_3", "hea_4", "hea_5", "hea_6")])
df1$sum_rec = rowSums(df1[,c("rec_1", "rec_2", "rec_3", "rec_4", "rec_5", "rec_6")])
df1$sum_soc = rowSums(df1[,c("soc_1", "soc_2", "soc_3", "soc_4", "soc_5", "soc_6")])
df1$sum_tot = rowSums(df1[,c("sum_eth", "sum_fin", "sum_hea", "sum_rec", "sum_soc")])
```


```
##                   ID sum_eth sum_fin sum_hea sum_rec sum_soc sum_tot
## 1  R_esd1kyBKHNuVgOi      24      22      24      20      27     117
## 2  R_0qS2WIn2azOzoYm      19      24      30      28      19     120
## 3  R_eDnu4YBcBk32BIa      29      24      27      22      19     121
## 4  R_9ysdW0tzLki3NeS      27      26      28      24      21     126
## 5  R_cBxSpN13lHnVM5E      24      18      35      31      30     138
## 6  R_4UistUDzw71os7k      29      20      27      27      20     123
## 7  R_ebyOHIQst9lwt9k      19      13      31      18      25     106
## 8  R_7UtywWeSjur8Xbw      23      22      24      21      23     113
## 9  R_6ShyWDxbRqVeMjI      28      24      23      28      25     128
## 10 R_4Z2wSXIWAmLcfcO      24      27      23      26      20     120
```

