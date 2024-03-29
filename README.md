# Survival_Analysis-in-R


 ![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQHP1FCTGYwe3XnwH0RVqurzycU50EsVaf3i79asb_0tg&s)



*Survival analysis* focuses on describing for a given individual or group of individuals, a defined point of event called ***the failure*** ( such as occurrence of a disease, cure from a disease, death, relapse after response to treatment...) that occurs after a period of time called ***failure time*** during which individuals are observed. It is usually expressed through the ***survival probability*** which is the probability that the event of interest has not occurred by a duration, t.

In short, survival data can be described as having the following three characteristics:

1.  the dependent variable or response is the waiting time until the occurrence of a well-defined event,

2.  observations can be censored, in the sense that for some units the event of interest has not occurred at the time the data are analyzed, and

3.  there are predictors or explanatory variables whose effect on the waiting time we wish to assess or control.

### **About the project**

The data set is of adjuvant chemotherapy for 594 colon cancer patients. We will focus on the study effect of treatment types on time to death. We shall visualize the data with kaplan-meier estimate of the survival function and the corresponding 95% confidence intervals. We shall fit a cox hazards proportional model, compare survival experiences and test their significance using the log rank test.You can find [here](https://www.kaggle.com/datasets/lakshmi25npathi/colon-cancer) the link to the data set.

