---
title: "Navigating Substance Use and Demographic Information"
author: "Hauke Bartsch"
teaching: 30
exercises: 0
questions:
- "How do I read the substance use and demographic surveys?"
- "How are these instruments administered?"
objectives:
- "Understanding of branching logic and the resulting pattern of missingness"
keypoints:
- "Missing value entries do not automatically indicate missing data"
---

## Substance Use Instrument [abcd_ysu01]

A search on abcd-deap.ucsd.edu (Ontology viewer) for `abcd-ysu01` results in 715 items that exist in this instrument. The average time an ABCD participant needs to fill out this instrument is 2.3 minutes. This is only possible because the participants do not use any substances yet. Instead of answering all 715 questions they only have to enter 'No' for questions asking "Have you ever heard about substance X?". If the answer 'No' is recorded no further questions about their use are presented to the participants. This is a feature of our data collection system (REDCap) called *branching logic*. Its important to understand the effects of this feature on the shared data to separate missingness by design from truly missing entries.

The very first question asked is stored in the variable `tlfb_alc`:
~~~
tlfb_alc in ABCD Youth Substance Use Interview / abcd_ysu01 [Substance Use]

Have you heard of alcohol, such as beer, wine or liquor?
0 = No; 1 = Yes
~~~

Let's check on NDA how the coding of this variable is done. The list of all ABCD data dictionaries can be found here: [https://ndar.nih.gov/data_dictionary.html?source=ABCD&submission=ALL]({{site.nda_abcd_dd}}). Search for the short name of the instrument `abcd_ysu01` and open that instruments data dictionary (click on the title). The row that describes the variable on NDA is shown here:

| ElementName| DataType| Size|	Required|	Condition|	ElementDescription|	ValueRange|	Notes|	Aliases
tlfb_alc|	Integer|	|	Recommended|	|	Have you heard of alcohol, such as beer, wine or liquor?|	0 ; 1|	0 = No; 1 = Yes|	

As you can see the information in the NDA data dictionary for this item is more extensive compared to what is displayed on DEAP. The Notes section contains the coding information with `0` for No and `1` for Yes.

On DEAP a mouse-click on the variable name in the Ontology viewer shows the summary information:
~~~
''	2
No	108
Yes	4,414
~~~
As you can see most participants have answered with Yes to this question (two participants with missing data).

The next substance use alcohol related questions are:
~~~
tlfb_alc_sip in ABCD Youth Substance Use Interview / abcd_ysu01 [Substance Use]

I want to start by asking if you have EVER TRIED any of the following drugs in your life. Have you ever tried at any time in your life? A sip of alcohol such as beer, wine or liquor (rum, vodka, gin, whiskey)
0 = No; 1 = Yes
~~~
and
~~~
tlfb_alc_use in ABCD Youth Substance Use Interview / abcd_ysu01 [Substance Use]

I want to start by asking if you have EVER TRIED any of the following drugs in your life. Have you ever tried at any time in your life? A full drink of beer, wine or liquor (rum, vodka, gin, whiskey)
0 = No; 1 = Yes
~~~

If we check the NDA data dictionary entries for these two questions we find:

| ElementName| DataType| Size|	Required|	Condition|	ElementDescription|	ValueRange|	Notes|	Aliases|
| tlfb_alc_sip|	Integer|		|Conditional|	 *tlfb_alc   == 1*|	I want to start by asking if you have EVER TRIED any of the following drugs in your life. Have you ever tried___________at any time in your life? A sip of alcohol such as beer, wine or liquor (rum, vodka, gin, whiskey)|	0 ; 1|	0 = No; 1 = Yes|
|tlfb_alc_use|	Integer|	|Conditional|	 *tlfb_alc   == 1 &&  tlfb_alc_sip   == 1*|	I want to start by asking if you have EVER TRIED any of the following drugs in your life. Have you ever tried___________at any time in your life? A full drink of beer, wine or liquor (rum, vodka, gin, whiskey)|	0 ; 1|	0 = No; 1 = Yes|

As you can see these questions have now an entry in the *Condition* column that encoded the branching logic for these items. They depend on the *tlfb_alc* item from above and only if that item is '1' the condition is true and the item was visible to the user.

It is easy now on DEAP to look up the summary information for these two variables:
~~~
tlfb_alc_sip

''	108
No	3,280
Yes	1,136

tlfb_alc_use

''	3,388
No	1,123
Yes	13
~~~

### Missingness is not missingness with branching logic

How do we interprete these numbers given that there is branching logic involved? For the sip question we have 108 missing entries. There are two possible reasons that those values could be missing. Either they are missing because the participant did not enter the values (true missingness) or, they are missing because the tlfb_alc question lists the answer 'No' for the participant (missingness by design).

A further complication is that missingness by design also affects the continuous variables that capture substance use quantities. The `tlfb_alc_max` item for example asks: "Great, now I'd like to know the largest amount of each drug that you have ever had in your life in one sitting. I am going to measure this in standard units, I will help explain that for each drug you have used.". The item statistical summary on DEAP lists 4,511 participants with missing data:
~~~
Min.	0.7
1st Qu.	1
Median	1
Mean	1.131
3rd Qu.	1
Max.	2
NA's	4,511
~~~

Due to the low number of participants with this type of substance use we cannot do a proper statistical analysis - other than to say that the ABCD cohort is still alcohol naive (8/9 year old participants). If alcohol use becomes more common in later years we would still expect a larger proportion of participants with missing (NA's) values. At that point it might be more appropriate to replace the NA values (not available) with the value '0' to indicate that no standard drinks where consumed by these participants.

This example highlights a major difference in handling data collection instruments compared to instruments usable for statistical analysis.

## Parent Demographic Survey [pdem01]

The demographic spreadsheets data dictionary is available on [NDA]({{site.nda_abcd_dd}}) (search for pdem01). This instrument is also using extensive branching logic - similar to the substance use instrument. This reduces the time required for the parent to fill out the form as questions are specific to the biological mother, father, adoptive parent and childs custodian. The relevant initial variable for this is `demo_prim`:

| ElementName| DataType| Size|	Required|	Condition|	ElementDescription|	ValueRange|	Notes|	Aliases|
|demo_prim|	Integer|	|	Recommended|	|	You are the: Usted es:|	1 ; 2 ; 3 ; 4 ; 5|	1 = Childs Biological Mother La madre biológica del niño / de la niña; 2 = Childs Biological Father El padre biológico del niño / de la niña; 3 = Adoptive Parent Padre o madre adoptivo(a); 4 = Childs Custodial Parent El padre o la madre que tiene la custodia del niño / de la niña; 5 = Other Otro	| |

