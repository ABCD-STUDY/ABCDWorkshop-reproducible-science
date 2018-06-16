---
title: "Baseline Data Analyses & DEAP"
teaching: 60
exercises: 0
questions:
- "How to structure an analysis that implements best-practices for ABCD?"
- "How are the baseline data analyses done?"
- "What is DEAP and how can it help researchers implement analyses?"
objectives:
- "To provide an introduction into the set of variables suggested as a framework for ABCD data analysis."
keypoints:
- We want the user to be familiar with variables suggested for baseline and the processing capabilities of DEAP.
---

During the workshop you may access the internal [DEAP]({{site.deap}}) using the user name `workshop` (password: `workshop`). Access to the program is provided under the [NDA access agreement]({{site.nda_data_use}}).

## Module 0 - Baseline data analysis

Key ABCD study components are:
- Longitudinal study design
- Nested structure with Site/Family/Scanner
- Enrollment model suitable for corrections towards American Community Survey targets
- Importance of effect sizes for large N studies

We will present these components and show how an analysis can utilize the inherent study structure to correctly model the degrees of freedoms using large N analysis pipelines in [R]({{ site.R }}).

## Module 1 - DEAP (RRID: SCR_016158)

The Data Exploration and Analysis Portal ([DEAP]({{https://scicrunch.org/scicrunch/Resources/record/nlx_144509-1/SCR_016158/resolver}})) implements convenient and enriched access to publicly shared data from the [National Data Archive]({{ site.nda }}). The portal is currently run by the DAIC and available under [abcd-deap.ucsd.edu]({{ site.deap }}).

Access to this resource is provided under the [NDA Use Certification]({{ site.nda_data_use }}).

### Background

At a high level [DEAP]({{ site.deap }}) does the following:
- access NDA data spreadsheets and data dictionaries for the ABCD study
- cache this information and make it searchable by the user
- data subsetting based on data values
- run statistical analysis such as regression models

The functionality is provided on a web-site that should function on most of today's browsers. We have tested the application under MacOS, Linux and Windows with modern browsers such as Safari (++), Firefox (++), Chromium (+), Chrome (++) and Edge (.). The DEAP applications are demanding and require a modern laptop with sufficient main memory (8GB suggested). JavaScript is used to provide most interactive functionality and needs to be enabled.

Information about DEAP project can be found under the [RRID: SCR_016158]({{https://scicrunch.org/scicrunch/Resources/record/nlx_144509-1/SCR_016158/resolver}}).

### Variables of interest to everyone

Some core variables are used on DEAP:

- Participant ID (src_subject_id)
- Family (rel_family_id, rel_group_id, rel_relationship)
- Site (abcd_site)
- Age (interview_age)
- Sex (sex, female, demo_sex_v2)
- Scanner (deviceserialnumber)
- Marital status (married, demo_prnt_marital_v2)
- SES (household.income, high.educ)
- Race/Ethnicity (race.ethnicity)
- Scanner identification (dti_manufacturer, dti_manufacturer_modelname, Fast-Track's DeviceSerialNumber)

### Pre-registration workflow

- Two stage approach supported by DEAP
  - 1) formulate hypothesis
  - [File your hypothesis on OSF (example).]
  - 2) execute your analysis
- What DEAP tools are allowed in the pre-registration phase?
  - identification of variables of interest
  - marginal distributions for variables of interest
  - explore variable transformations (log/censoring)
- What DEAP tools are not allowed in the pre-registration phase?
  - scatter plots between variables in the model
  - access to individual data points with participant identifiers
  - running a hypothesis

### How to find variables of interest?

- Search in the Ontology viewer (by domain, by search terms)
- Show distributions and factor levels for the ABCD core demographic variables.

### How to (not) work with outliers?

Distributions should fit to the model used - check for Gaussianity!

- Subsetting (selects participants)
- Censoring
  - Find the variable that contains the values for the mean diffusivity of all fibers and select that variable as the dependent variable
  - Look at the distribution for this variable. What can we do to improve the model? 
- Expert mode

### How to compare different models with each other?

In order to find out what the best set of covariates is for a given model use the displayed values for the Akaiken Information Criterion. Enable the *Tutorial mode* in the Multi-level Model application by clicking on the menu header *Normal*. Read the section about model comparison. Use the standard model to explore different combinations of covariates - write down the AIC for each model.

### Advanced applications

Try out the multi-dimensional embedding application. Open the application from the DEAP home page and create an embedding for the 25 FreeSurfer variables.

