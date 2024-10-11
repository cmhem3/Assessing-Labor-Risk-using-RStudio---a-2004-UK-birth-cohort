# Investigating Potential Risks of Long Labor and Associations of Emergency Cesarean Sections
A masters of public health project identifying c-section risk using RStudio (2020-02-02)



### DATA SOURCE:
The data set for this analysis is the 2004 UK Birth cohort. This was a prospective observational study on 8,915 women who intended to deliver their babies while labouring in water (a waterbirth). The women were cared for by midwives employed by the National Health Service (NHS) in three different delivery settings. The women either attended a hospital, small midwifery-led unit, or stayed at home to deliver their baby. The original study examined the characteristics of the women and any subsequent interventions they had during labour.  In this assessment we want you to explore what the predictors are of two outcomes. 

#### GRADUATE SCHOOL CRITERIA FOR DATA ANALYSIS
    -undertake a re-analysis of this birth cohort dataset

    -submit a 550 word abstract summarising the study and what you found

In addition to the abstract, submit two data flow diagrams*, one for each of the two final models you fit

Finally, submit the single line of R code for each model

* The data flow diagram should depict how many women were in the dataset at the start, how many you then removed prior to analysis with the reason why you removed them, and how many women were included in your final analysis model. 

INSTRUCTIONS TO ANALYZE THE BIRTH COHORT DATASET: 
We would like you to undertake regression analysis to find out predictors of the outcomes:

    -Having an emergency Caesarean section - including any of the variables in the dataset of  your choosing
    -The time to birth of the baby (time taken from start of contractions to giving birth) - using the variables: gestational age (categorised), maternal age <30 (Y/N), parity (primip/multip), twins (Y/N), Breech (Y/N), membranes ruptured (Y/N), centre attended. 

Parity indicates the number of pregnancies reaching viable gestational age (including live births and stillbirths). The number of foetuses does not determine the parity, which means that a twin pregnancy carried to viable gestational age is counted as 1. If this is the woman’s first pregnancy, she will be what’s called a “primip”, short for “primiparous”; if not, she will be a “multip”, short for “multiparous”.

We would like to you analyse time to birth using survival analysis techniques. This means you will have to calculate a new variable for time to birth using the Lengthlabor variables in the dataset. You will also need to generate an extra censoring variable for the event of birth occurring (Yes=1, No=0). If there is missing data for time, you can consider including the time you at least know the women has been labouring for and then censor the observation if you do not have information for later stages. 

For this exercise there is no need to explore or include interaction terms. 

Note that this sample of women is a self-selecting group who had chosen to use a waterbirth pool during labour, which means they won’t be a representative sample of the population of all women in the UK giving birth.

In the dataset you will see there are maternal characteristics, interventions and birth and baby outcomes. Explore these variables to get familiar with the dataset. All missing data has been coded with the value ‘-9’. All impossible values in the dataset you should recode to missing.

#You will see the outcome variable:

·       EmergencyCS

This variable has the value 1 when the mother has had an emergency Caesarean section and 0 if they did not. A Caesarean section, or C-section or CS, is an operation to deliver the baby through the mother’s abdomen. An emergency C-section is undertaken when the mother or baby are assessed to be in dire stress and immediate delivery is the only option.  Note that this is a low risk population and the rate of Emergency CS is much lower than the UK national average of 20-25%.

In the dataset you will see the Lengthlabor variables where time has been recorded in minutes:

·       Lengthlabor1st: 1st stage of labour measured in minutes

·       Lengthlabor2nd: 2nd stage of labour measured in minutes

·       Lengthlabor3rd: 3rd stage of labour measured in minutes

Labour has three stages:

·       1st stage = labour contractions with cervical dilatation to 10 centimetres

·       2nd stage = from the onset of expulsive contractions and full cervical dilatation to the birth of the baby

·       3rd stage = from the birth of the baby to the delivery of placenta and membranes

Remember to undertake a thorough exploration of the variables prior to starting any analysis to help you decide what variables to include in the models and how best to include variables if there are missing values or unusual distributions.

Gestational age is recorded in weeks. We recommend that this is first turned into a categorical variable with the following categories: <38 weeks (shorter than desired), 38-42 weeks (normal pregnancy range), and >42 weeks (longer than desired).

In your write-up of emergency CS model, we would like you to report on the relationship with the outcome for 1) the centre attended by women, 2) gestational age and 3) one other model covariate of your choosing. Include in the methods or results section what these estimates have been adjusted for, i.e., which other variables had been included in your model. 

In your write-up of the survival analysis, we would like you to report on how age (>30) and centre are associate with time to birth.  In this model you may detect evidence for non-proportional hazards between groups, but as the hazards do not swap and the model will provide an average over the whole time period, we are satisfied these model estimates provide a useful summary.  

**ANALYSIS USING RSTUDIO**

**BACKGROUND**:

Multiple factors may contribute to mortality during pregnancy.  Emergency Cesarean Section (ECS) is a technique done to preserve the life of both mom and baby.  Educating women on risk factors may help prevent unnecessary ECS.  Additionally, long labor time (LLT) is affected by numerous causes and that can jeopardize health outcomes of mom and baby.  This paper will analyze possible risks and associations of ECS and LLT.

**METHODS**:

An analysis using RStudio was performed on a relatively low-risk birth cohort to assess odds of ECS using Logistic Regression (Log-Reg).  Moreover, survival analysis via Cox Regression (Cox-Reg), was performed to analyze factors that affect LLT.  The analysis was done using two datasets, var_labels, which is a table listing the categorical status of all 8915 patients in the study.  The other is var_numbers, is a numeric table listing clinical status and outcomes of labor.  Both tables were free of major data errors, but flow-charts are included below to showcase where data may have been altered/removed to accurately run the regression models.  

**RESULTS**:

Log-Reg analyzed odds ratios (OR) of ECS using variables such as healthcare facility (i.e. “center_attended”, mid-wife-clinic vs hospital), gestational category (i.e. “Gestation_cat”, <38 weeks, 38-42 weeks, or >42 weeks).  Women with a previous c-section (i.e. “PreviousCS” with Gestation_cat, >42weeks) was found significant for an ECS (p=0.0003, 95% CI 1.90 - 6.84, OR 76.42) compared to Gestation_cat 38-42 weeks (p >0.05, OR = 3.20).  Unsurprisingly, PreviousCS, while significant (p<0.05, OR = 30.40), had a very small number of patients (n=18) so further research with larger sample size is warranted.  Midwife-clinic was found significant for ECS (p< 0.05), but the OR was low (0.074).  
Code for the Log-Reg model here: 

    -Log-RegModel <- glm(EmergencyCS ~ Gestation_cat + center_attended + PreviousCS, family = binomial(link = logit))
 
![image](https://github.com/user-attachments/assets/7ae092c3-418b-4e32-bf80-a54f2ce062ef)

Cox-Reg performed survival analysis to assess hazard ratios (HR) of LLT using the variables Gestation (defined above), Maternal Age (i.e. Mage, age under or over 30 years), Parity (number of previous pregnancies), Twins, Cephalic_Breech (position of baby), Membranes_ruptured, and Center_attended.  Essentially, HR’s <1 means less risk of LLT, and likely reduced risk of complications.  Variables associated with increasing LLT was Mage (HR 1.36, p<0.05), having Twins (HR 2.29, p<0.05), Membrane_not_ruptured (HR 1.73, p<0.05).  Other variables, like gestation, parity, and center (hospital) had p-values <0.05, but low HR’s.  As well, higher HR’s may contribute to longer LT and possible complications, like ECS.  
Code for Cox-Reg model:

    -Cox-RegModel <- coxph(Surv(labor, birth) ~ LL1$Gestation + LL1$Mage_cat2 + LL1$Parity + LL1$Twins + LL1$Cephalic_Breech + LL1$Membranes_rupture + LL1$center_attended) 

![image](https://github.com/user-attachments/assets/42dcd0e8-c68f-4a4c-be13-88244f171484)

**CONCLUSION**:

In summary, educating women on their risks for both ECS and LLT are essential to maternal-baby health.  Future studies may benefit investigating how previous c-sections may influence future ECS.  As well, literature reviews on maternal age, twins, parity, and center of delivery may provide additional context. Finally, there is a need to educate women on risk-factors to prevent harm to mom and baby.



**REFERENCES**:
1. Coursera. (n.d.). Statistical Analysis with R for Public Health. Retrieved February 2nd, 2020, from https://www.coursera.org/specializations/statistical-analysis-r-public-health?msockid=0a8de113eef36e56141cf5d6ef7f6f10


