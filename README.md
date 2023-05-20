# Survival_Analysis-in-R



![]("C:/Users/steven\Desktop\survival.png")

WHAT IS SURVIVAL ANALYSIS ?

*Survival analysis* focuses on describing for a given individual or group of individuals, a defined point of event called ***the failure*** ( such as occurrence of a disease, cure from a disease, death, relapse after response to treatment...) that occurs after a period of time called ***failure time*** during which individuals are observed. It is usually expressed through the ***survival probability*** which is the probability that the event of interest has not occurred by a duration, t.

In short, survival data can be described as having the following three characteristics:

1.  the dependent variable or response is the waiting time until the occurrence of a well-defined event,

2.  observations can be censored, in the sense that for some units the event of interest has not occurred at the time the data are analyzed, and

3.  there are predictors or explanatory variables whose effect on the waiting time we wish to assess or control.

### **About the project**

The data set is of adjuvant chemotherapy for 888 colon cancer patients. We will focus on the study effect of treatment types on time to death. We shall visualize the data with kaplan-meier estimate of the survival function and the corresponding 95% confidence intervals. We shall fit a cox hazards proportional model, compare survival experiences and test their significance using the log rank test.You can find [here](https://www.kaggle.com/datasets/lakshmi25npathi/colon-cancer) the link to the data set.


---
title: "Survival analysis report"
output: 
  flexdashboard::flex_dashboard:
    theme: sandstone
    vertical_layout: fill
    orientation: columns
---

```{r setup, include=FALSE}
library(flexdashboard)
library(gt)
library(ggplot2)
library(dplyr)
library(here)
library(magrittr)
library(flextable)
library(survival)
library(janitor)
library(plotrix)

```

## Column {data-width=300}

### GLIMPSE OF THE DATASET

```{r}
DF <- rio::import(here("C:/Users/steven/Desktop/PROJECTS/colon.csv"))
  DF1 <- subset(DF,rx >= 2)

    colon <- DF1 %>% 
     select(
         time,
         status,
         rx,
         age,
         sex) %>% 
    mutate(
          follow_up=time/365,  
          event = ifelse(status==1, "death", "censored"),
          trt_ype  = ifelse(rx==2, "amisole", "amisole+5-FU"),
          gender = ifelse(sex == 1, "male", "female"),
          age_cat = case_when(
          age < 35 ~ "0 - 34",
          age >= 35 & age < 65 ~ "35 - 64",
          age >= 65 ~ "65+")
          )
colon %>% 
  select(age, gender, trt_ype, event) %>% 
   head(10)     


 
```

### BARPLOT OF EVENT STATUS BY TREATMENT TYPE

```{r}
table2 <- table(colon$event, colon$trt_ype)          
barplot(table2,beside = T,
        main ="Event status by Treatment type",
        ylab = "status number",
        xlab = "Treatment type",
        col.main = "blue",
        col.lab = "darkblue",
        col = c("lightgreen","red") )
legend("top",
       legend =c("censored","deaths"),
       bty ="n",
       fill = c("lightgreen","red"))

```

## Column {data-width=350}

### NUMBER OF EVENT STATUS BY GENDER

```{r}
colon3 <- colon %>% 
  select(gender, event) %>% 
  filter(event == "death") 

table3 <- table(colon3)
piepercent <-paste0(round(100 * table3/sum(table3), 1), "%")

pie3D(table3,radius = 1.5,
                explode = 0.3,
                labels = piepercent,
                col = c("purple","orange"),
                main = "Deaths by gender ",
                col.main="blue")
legend("topright",
       c("female","males"), 
       cex = 0.8,
       fill =c("purple","orange") )


```

### KAPLAN MEIER CURVES BY GENDER

```{r}
colo <- c("blue", "darkgreen")

colon_gender <- survfit(Surv
                        (follow_up,status) ~sex,
                        data =colon)
 
 #Survival curves for male and female 
 plot(
   colon_gender,
   col = colo,
   xlab = "Time(in years) follow up",
   ylab = "Survival probability",
   main = "Survival curves by gender",
   col.main = "blue"
   )
  legend(
    "topright",
  legend = c("female", "male"),
  col = colo,
   lty = 1,
   cex = .9,
   bty = "n"
   )

```

## Column {data-width=350}

### NUMBER OF EVENTS STATUS BY TREATMENT TYPE

```{r}
colon %>% 
  tabyl(trt_ype, event) %>% 
   adorn_totals(where = "both") %>%
  adorn_percentages() %>% 
  adorn_pct_formatting() %>% 
  adorn_ns(position = "front") %>% 
  gt()
```

### KAPLAN MEIER CURVES BY TREATMENT TYPES

```{r}
colon_trt <- survfit(
                  Surv(follow_up,status) ~
                    trt_ype,data =colon)

plot(
  colon_trt,
  col = colo,
  xlab = "Time(in years) follow up",
  ylab = "Survival probability",
  main = "Survival curves by Treatment type",
 col.main = "blue" )
legend(
  "topright",
  legend = c("lev(amisole)","lev(amisole)+5-FU"),
  col = colo,
  lty = 1,
  cex = .9,
  bty = "n"
  )
```
























