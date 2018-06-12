---
title: "Reproducible Science and Hypothesis Testing"
teaching: 30
exercises: 0
questions:
- "What is reproducible science and why should I do it?"
- "What tools exist for reproducible science?"
- "How should I share my results?"
objectives:
- "Be familiar with scripting and git"
keypoints:
- "Use established tools to share analysis streams"
---

> ## You can skip this lessen if you can answer these questions: &#8680;
>
>  - What is the preferred shebang for portability in bash?
>  - How do I commit to the master branch on the command line?
>  - Name your three favorite text editors that work on Windows/MacOS/Linux
>  - Why should I fork?
{: .challenge}

# Module 0: Reproducible Science

## Access to the workshop shell environment

You may access the https://abcd-workshop.ucsd.edu. Hit enter twice for host and port, and enter one of the workshop user names (abcd01, abcd02, ... abcd20):
~~~
[Press Shift-F1 for help]

Host/IP or ssh:// URL [localhost]:
Port [22]:
User: abcd01
Connecting to ssh://abcd01@localhost:22

abcd01@localhost's password:
~~~
You should see a shell prompt that allows you to enter further commands.



## Scripting documents workflows



## Version control documents papers



# Module 1: Hypothesis Registration

In order to make hypothesis registration as easy as possible the DEAP platform supports the structured generation of hypothesis. This includes the definition of the hypothesis with variable selection and data transformations.

## Overview

The Adolescent Brain Cognitive Development (ABCD) Study is a long-term study of cognitive and brain development in children across the United States. A notable team of investigators, well known in the fields of adolescent development and neuroscience, has received funding from the National Institutes of Health to conduct this ambitious project. From 2016-2018, children between the ages of 9-11 have been invited to join the study from 21 sites around the nation, with the intent to enroll and follow approximately 11,500 healthy children longitudinally for 10 years into young adulthood.

Volkow, Nora D., et al. "The conception of the ABCD study: From substance use to a broad NIH collaboration." Developmental cognitive neuroscience (2017)

### Sampling Plan

An important motivation for the ABCD study is that its sample reﬂects the sociodemographic variation of the US population. With one important departure, the ABCD cohort recruitment emulates a multi-stage probability sample of eligible children: A nationally distributed set of 21 primary stage study sites, a probability sampling of schools within the deﬁned catchment areas for each site, and recruitment of eligible children in each sample school. The major departure from traditional probability sampling of U.S. children originates in how participating neuroimaging sites were chosen for the study. Although the 21 ABCD study sites are well-distributed nationally the selection of collaborating sites is not a true probability sample of primary sampling units but was constrained by the grant review selection process and the requirement that selected locations have both the research expertise and the neuroimaging equipment needed for the study protocol. As a consequence, neuroimaging research centers are more likely to be located in urban areas resulting in a potential under-representation of rural youth.

There is a substantial twin cohort in ABCD, consisting of monozygotic and dizygotic twin pairs, primarily collected at four of the 21 sites. There are also other sibling groups that will have been collected incidentally at all 21 sites. 

Garavan, H., et al. "Recruiting the ABCD sample: design considerations and procedures." Developmental cognitive neuroscience (2018).

### Design Plan

The ABCD study is a prospective longitudinal study with yearly assessments. The November 2017 public data release on the NIMH Data Archive (NDA) consists of baseline data from 4,524 children from 20 sites. Data collection efforts for ABCD are organized into several data categories. Overviews of neuroimaging data, neuropsychological measures, biospecimens, substance use assessments, and culture and environment are given in the citations below.

Lisdahl, Krista M., et al. "Adolescent brain cognitive development (ABCD) study: Overview of substance use assessment methods." Developmental cognitive neuroscience (2018).

Uban, Kristina A., et al. "Biospecimens and the ABCD Study: Rationale, Methods of Collection, Measurement and Early Data." Developmental cognitive neuroscience (2018).

Casey, B. J., Cannonier, T., Conley, M. I., Cohen, A. O., Barch, D. M., Heitzeg, M. M., ... & Orr, C. A. (2018). The Adolescent Brain Cognitive Development (ABCD) Study: Imaging Acquisition across 21 Sites. Developmental cognitive neuroscience.

Luciana, M., et al. "Adolescent neurocognitive development and impacts of substance use: Overview of the adolescent brain cognitive development (ABCD) baseline neurocognition battery." Developmental cognitive neuroscience (2018).

Zucker, Robert A., et al. "Assessment of culture and environment in the adolescent brain and cognitive development study: Rationale, description of measures, and early data." Developmental cognitive neuroscience (2018).

### Analysis Plan

Analyses should, if possible, reflect the study design of ABCD. In particular, there are a substantial number of siblings (including twins) in the study, and hence subjects should be nested within families. Moreover, we recommend including site as a random or fixed effect covariate in regression analyses. In DEAP, the Multilevel Module incorporates family and site as random effects, and defaults to including as fixed effect covariates the demographic categories that were used as metrics in recruitment to balance with the American Childhood Survey (ACS): age, sex at birth, race/ethnicity, household income, marital status of guardian, highest household education. 

Proper attention should be given to model assumptions. For example, normality of residuals or linearity of effects for regression models. If these assumptions are not met, consider data transformations or censoring of extreme outliers, or use of non-linear models.

### Analysis Scripts

We strongly recommend making analysis scripts publicly available along with any published results. DEAP allows for downloading and sharing of R scripts used in analyses. Scripts can be included as Supplementary Materials in published results. We also recommend uploading scripts to ABCD Github (https://github.com/ABCD-STUDY/).

