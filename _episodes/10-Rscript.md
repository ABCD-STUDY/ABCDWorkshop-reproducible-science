---
title: "Multi-level Model Explained"
teaching: 30
exercises: 0
questions:
- "If I know R, how do I read the downloaded R-script for GAMM4?"
objectives:
- "Familiarize participants with the structure of the statistical analysis script."
keypoints:
- "Key components in the analysis script are single liners"
---

In order to support the open science initiative the DAIC provides full access to the behind the scenes analysis scripts that make DEAP possible. This exposes the core model setup as well as code that is used to parse the user interface and code that generates the graphical output. This workshop will show the basic structure of the downloadable script and highlight ways in which the code can be adjusted.

### Export a multi-level regression model

Use the Hamburger menu on the top left of the Multi-level model page to open the session controls of the application. Create a model, run it and save the model under a new name. Now download the model. This will create a zip file, which contains the model specification, the data used by the model and all the model graphs.

This functionality is still in development, please report any bugs you might see with this functionality.


### Which sections of the R-script are important for the analysis?

The first part of the Multi-level model shows two Expert mode features, the conversion of a factor level variable into a continuous variable and the change of the reference category for a factor variable.

```{r}
# Change the coding from factor levels to numbers
#levels(data$physical_activity1_y) = c("", 0:7);
#data$physical_activity1_y = as.numeric(as.character(data$physical_activity1_y))

# Change the reference category of a factor level variable
# data$sex <- factor(data$sex, levels=c("M","F"))
```
The next part of the analysis script implements the transformations of the variable and interaction variables (adding main effects). Followed by implementing the subsetting feature:

```{r}
data = data[c("src_subject_id","rel_family_id","abcd_site",varList)]
##################
##  subset data ##
##################
if(length(subsetVar)>0){
    json_data = fromJSON(file = subsetVar);
    subset = data.frame(src_subject_id=unlist(lapply(json_data[[1]]$set,function(d){ d[1] })), eventname=unlist(lapply(json_data[[1]]$set,function(d){d[2]})));
    data = merge(subset, data, all.x = T, all.y = F)
}
```
and limiting of the data to rows without missing values:
```{r}
data = data[complete.cases(data),]
```
The code creates two model formulas with the covariates and interaction included and one without the covariates and the interaction variables.
```{r}
#################
##  run model  ##
#################

if(categorical.dependent){ #logistic regression
  model  = gamm4(as.formula(formulastr) , random = ~(1|abcd_site/rel_family_id), family = binomial , data = data)
  model2 = gamm4(as.formula(formulastr2) , random = ~(1|abcd_site/rel_family_id), family = binomial , data = data)
  
} else{ #linear regression
  model  = gamm4(as.formula(formulastr) , random = ~(1|abcd_site/rel_family_id), data = data)
  model2 = gamm4(as.formula(formulastr2) , random = ~(1|abcd_site/rel_family_id), data = data)
}
```
A binomial model family is selected of the dependent variable is a categorical variable. Otherwise the standard model is run using site and family as random effects. Whereas the covariates can be disabled by the user the random effects can only be changed in Expert Model by editing the above section of the R-script.

All other parts of the script create the model output tables and the predicted model with the model error. The following is a section of that code that calculates the AIC and BIC values for the full model and the restricted model:
```{r}
n = summary(model$gam)$n
aic = round(AIC(model$mer),2)
bic = round(BIC(model$mer),2)
r2   = round(summary(model$gam)$r.sq,5)
r2_delta = round(as.numeric(r.squaredLR(model$mer,model2$mer)),5)
aic2 = round(AIC(model2$mer),2)
bic2 = round(BIC(model2$mer),2)
```
As you can see relevant portions of the R-code are quite compact and can be easily changed on the DEAP page.
