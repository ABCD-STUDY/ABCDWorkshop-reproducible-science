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

## Module 0 - Baseline data analysis

Key ABCD study components are:
- Longitudinal study design
- Nested structure with Site/Family/Scanner
- Enrollment model suitable for corrections towards American Community Survey targets
- Importance of effect sizes for large N studies

We will present these components and show how an analysis can utilize the inherent study structure to correctly model the degrees of freedoms using large N analysis pipelines in [R]({{ site.R }}).

## Module 1 - DEAP

The Data Exploration and Analysis Portal (DEAP) implements convenient and enriched access to publicly shared data from the [National Data Archive]({{ site.nda }}). The portal is currently run by the DAIC and available under [abcd-deap.ucsd.edu]({{ site.deap }}).

Access to this resource is provided under the [NDA Use Certification]({{ site.nda_data_use }}).

### Background

At a high level [DEAP]({{ site.deap }}) does the following:
- access NDA data spreadsheets and data dictionaries for the ABCD study
- cache this information and make it searchable by the user
- data subsetting based on data values
- run statistical analysis such as regression models

The functionality is provided on a web-site that should function on most of today's browsers. We have tested the application under MacOS, Linux and Windows with modern browsers such as Safari (++), Firefox (++), Chromium (+), Chrome (++) and Edge (.). The DEAP applications are demanding and require a modern laptop with sufficient main memory (8GB suggested). JavaScript is used to provide most interactive functionality and needs to be enabled.

### How to find variables?

- search
- look at the distribution/factor levels

### How to remove outliers?

Distributions should fit to the model used - check for Gaussianity!

- Subsetting
- Censoring
- Expert mode to winsorize

### Pre-registration workflow

- Two stage approach
 - 1) formulate hypothesis
 - file the hypothesis
 - 2) run analysis
- What tools are allowed?


[ see if we can use http://swcarpentry.github.io/shell-novice/ in another tutorial ]