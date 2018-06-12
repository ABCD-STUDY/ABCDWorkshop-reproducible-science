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

- Participant ID
- Family
- Site
- Age
- Sex/Gender
- Scanner
- ...

### Pre-registration workflow

- Two stage approach
  - 1) formulate hypothesis
  - file the hypothesis
  - 2) run analysis
- What DEAP tools are allowed in the pre-registration phase?

### How to find your variables of interest?

- Search in the Ontology viewer
- Show distributions and factor levels

### How to work with outliers?

Distributions should fit to the model used - check for Gaussianity!

- Subsetting
- Censoring
- Expert mode to winsorize



[ see if we can use http://swcarpentry.github.io/shell-novice/ in another tutorial ]
