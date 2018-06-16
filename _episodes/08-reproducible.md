---
title: "Reproducible Science and Hypothesis Testing"
author: "Hauke Bartsch"
teaching: 30
exercises: 0
questions:
- "What is reproducible science and why should I care?"
- "What tools exist for reproducible science?"
- "How should I share my results?"
objectives:
- "Be familiar with scripting and git"
keypoints:
- "Use established tools to share analysis streams. Use version control to store project history."
---

> ## You can skip this lesson if you can answer these questions: &#8680;
>
>  - What is the preferred shebang for portability in bash?
>  - How do I commit to the master branch on the command line?
>  - How do I create a github repository fork, implement a change suggest a merge?
>  - Name your three favorite text editors that work on Windows/MacOS/Linux
{: .challenge}

# Module 0: Reproducible Science

## Access to the command shell

For the duration of the workshop we are providing an interactive shell on a website: https://abcd-workshop.ucsd.edu. Select the SSH icon in the center of the page, hit enter twice to accept the default values for host and port, and enter one of the workshop user names (abcd01, abcd02, ... abcd20):
~~~
[Press Shift-F1 for help]

Host/IP or ssh:// URL [localhost]:
Port [22]:
User: abcd01
Connecting to ssh://abcd01@localhost:22

abcd01@localhost's password:
~~~
You should see a shell prompt that allows you to enter further commands.

## Introduction

The shell will wait for you to enter the first command. The easiest command is to press the Enter key on your keyboard which will show the shell prompt again:
~~~
abcd03@abcd-workshop:~$
abcd03@abcd-workshop:~$
abcd03@abcd-workshop:~$
~~~
It shows your user name (here abcd03), the name of the machine - abcd-workshop and your current directory '~' (shortcut for your home directory). You can start typing commands after the '$' sign.

### Directories

Your user's home directory should be empty. You can check this by first making sure you are in your home directory. Type in the command `pwd` which stands for _print working directory_. Many commands use this type of short name to save you from typing too much.
~~~
abcd03@abcd-workshop:~$ pwd
/home/abcd03
~~~

The command to list the content of the current directory is _ls_:
~~~
abcd03@abcd-workshop:~$ ls
abcd03@abcd-workshop:~$
~~~
It is also usual for a command to not produce any output if there is nothing to report.

In order to organize your project use a directory. You can always create a directory in your home folder:
~~~
abcd03@abcd-workshop:~$ pwd
/home/abcd03
abcd03@abcd-workshop:~$ mkdir workshop
abcd03@abcd-workshop:~$ ls
workshop
~~~

We use the `pwd` command to make sure we are in our home directory, `mkdir` - _make directory_ to create a new folder called `workshop`, and listed the home directory again with `ls`. This shows us our new directory. It is important to be aware of your current working directory. In other peoples directories you will not be able to create your folders.

In order to change our working directory to the new directory we can use the _change directory_ command `cd`. The `cd` command is also useful to get us back into our home directory if it is typed on its own:
~~~
abcd03@abcd-workshop:~$ cd
abcd03@abcd-workshop:~$
~~~
Add the name of the new directory to the command (separated by a space) to change to another directory:
~~~
abcd03@abcd-workshop:~$ cd workshop
abcd03@abcd-workshop:~/workshop$
~~~
This will change the prompt on the line, instead of just the tilde `~`-character now the prompt lists your home directory `~` followed by `workshop`. The slash (`/`) character is used to separate different folder names from each other.

List your current directory with `pwd` and use the `cd` command to go back to your home directory. Enter the workshop directory again using `cd workshop`.
~~~
abcd03@abcd-workshop:~/workshop$ pwd
/home/abcd03/workshop
abcd03@abcd-workshop:~/workshop$ cd
abcd03@abcd-workshop:~$ cd workshop
abcd03@abcd-workshop:~/workshop$
~~~
In order to move up relative to your current working directory use the special directory name which consists of two dots (`cd ..`).


Create two more directories called `source` and `bin` inside your `workshop` directory (use `mkdir`). Go back to your home directory and list your directory tree with the `tree` command. You should see this output:
~~~
abcd03@abcd-workshop:~$ tree
.
└── workshop
    ├── bin
    └── source

3 directories, 0 files
~~~

There are many more commands availble. You should get help for each one of them if you use the command `man` - _manual page` followed by the command you want to know more about. Find out what the command `banner` is doing from the help page and use it on your screen.

Find out about the `nudoku` command.

Find out about the `moon-buggy` command.

### Create a script

A shell script is a text document that lists single commands. By storing them in a text file we can run them later in the same order. We can share them with other people as well - one of the goals of reproducible science. We will store our script file in the `~/workshop/bin` directory. Change to this directory and open the text editor `nano`.
~~~
abcd03@abcd-workshop:~/workshop/bin$ nano
~~~

The nano text editor is very simple to use. You can type some text and save it under a new file name using the Control-o (^O) keyboard short-cut. Enter the filename `workshop01.sh` and leave the editor again (^x). The new script is now in our directory tree (use `cd` and `tree` to list the file):
~~~
abcd03@abcd-workshop:~/workshop/bin$ cd
abcd03@abcd-workshop:~$ tree
.
└── workshop
    ├── bin
    │   └── workshop01.sh
    └── source

3 directories, 1 file
~~~

To indicate that this file should be executable and not just a text file change the permissions of the file using the `chmod` - _change permission_ command:
~~~
abcd03@abcd-workshop:~$ cd workshop/bin/
abcd03@abcd-workshop:~/workshop/bin$ chmod gou+x workshop01.sh
abcd03@abcd-workshop:~/workshop/bin$
~~~
Now we can execute our script by tying its name (prepend `./` to indicate the script in the current directory).
~~~
abcd03@abcd-workshop:~/workshop/bin$ ./workshop01.sh
./workshop01.sh: line 1: some: command not found
abcd03@abcd-workshop:~/workshop/bin$
~~~
As you can see we got an error message in the first line of our new script. The first line I entered was `some text` and it tried to find the command `some` - which does not exist. The commands we enter inside a script have to be valid commands on our system.

Use `nano` to change the text in your `workshop01.sh` file to contain the following two lines:
~~~
#!/usr/bin/env bash

echo "some text"
~~~
Close the editor with ^o and ^x and run the script again:
~~~
abcd03@abcd-workshop:~/workshop/bin$ nano workshop01.sh
abcd03@abcd-workshop:~/workshop/bin$ ./workshop01.sh
some text
abcd03@abcd-workshop:~/workshop/bin$
~~~
Now the script correctly echo's "some text". Open the script again and change the text, add some more text or use `banner` instead of echo.

### Text editors you should know how to get out of

The default text editor on most unix based systems is called `vi`. This editor is used as the default editor by the Unix system if a manual change to a text file is required. In order to quit the vi editor again you should remember the following sequence of keys: `ESC :q!`.


## Test data

We have some example Fast-Track data archives stored on our system in the `/data/` directory. These files are shared on NDA and usually you would work with about 100,000 of these files instead of the small sample we use in this workshop. We can list the first file in the directory using a combination of commands separated by the pipe `|` character which will take the output of the first command and send it to the second command. What we will see as the result is the output of the second command. Here the first command lists all the files in that directory and the second command returns only the first file, all other lines from ls are removed:
~~~
abcd03@abcd-workshop:~/workshop/bin$ ls /data/ | head -1
NDARINV28X60X09_baselineYear1Arm1_ABCD-Diffusion-FM_20180312164901.tgz
~~~
Combining commands in this way is an effective way to process large numbers of files consistently. Here is a chain of commands that lists all the participant IDs in the /data directory
~~~
abcd03@abcd-workshop:~/workshop/bin$ ls /data/ | cut -d'_' -f1 | sort | uniq
NDARINV28X60X09
NDARINVXLYDYTLX
NDARINVZ694ZZUM
~~~
Use `man` to learn about the new commands `cut`, `sort`, and `uniq`.

If we now add the command `wc -l` to the above chain we can also count the number of participants (3) we have available:
~~~
abcd03@abcd-workshop:~/workshop/bin$ ls /data/ | cut -d'_' -f1 | sort | uniq | wc -l
3
~~~

It is difficult to give a complete list of commands that are useful to know. Here is just the list of my favorites: `pwd, ls, cd, mkdir, cut, sort, uniq, wc, find, bc, tar, cat, rm, echo`.

## Scripting a workflow

One of the most useful scripting functions is to be able to access a subset of files from our `/data` directory one after another and to perform an operation on each. In the simplest case we want to filter the list and list all the T1 weighted image series. Open the `workshop01.sh` script in `nano` and replace the content with the following loop. The line is more complex because it is able to work with spaces in the directory or file names. If you are using a loop use this one as it will usually work:
~~~
#!/usr/bin/env bash

find /data/* -name "*T1*.tgz" -print0 | while IFS= read -r -d $'\0' line; do
    echo $line
done
~~~
Run this script using its name `./workshop01.sh` and we should get a list of all the files that contain T1 weighted image series. Instead of `echo $line` (prints out the current filename) we can now use the `tar` command to extract a single DICOM file from this archive. All the DICOM files contain information about the whole image series so we can extract for example the DeviceSerialNumber tag from that DICOM file and remove the file afterwards again to keep our directory clean:
~~~
#!/usr/bin/env bash

find /data/* -name "*T1*.tgz" -print0 | while IFS= read -r -d $'\0' line; do
   tar --wildcards -xf $line "*00001.dcm" --strip-components 4
   dcmdump +P DeviceSerialNumber *.dcm | cut -d'[' -f2 | cut -d']' -f1
   rm *.dcm
done
~~~
The `dcmdump` command is provided by the marvelous dcmtk tools (dicom.office.de).


Running this script should produce the following list of (anonymized) scanner device serial numbers from each T1 series:
~~~
abcd03@abcd-workshop:~/workshop/bin$ ./workshop01.sh
anon3255
anon1364
anonbe03
anonbe03
~~~

Change the script to show the DeviceSerialNumber tag for the DTI series.


## Version control

Keep track of your changes using a version control system. Instead of adding dates to filenames and copying files called `_backup42` you need to use a version control software which will help to document your changes over time. GIT is such a modern version control system that makes it easy to document your work.

We can use our `workshop` directory to start keeping track of our changes using git. Start by creating an empty git repository in that directory:
~~~
abcd03@abcd-workshop:~/workshop/bin$ cd ~/workshop
abcd03@abcd-workshop:~/workshop$ git init
Initialized empty Git repository in /home/abcd03/workshop/.git/
~~~
If you list now the content of the workshop directory it will not show any changes - version control tries to keep your directory clean by storing all the information in the invisible .git directory. The `ls` command has an option to show these invisible directories and files as well. Any file you want to keep track of needs to be added to git. From that point forward the file is under version control. We have a single file `workshop01.sh`, lets add it to git.
~~~
abcd03@abcd-workshop:~/workshop$ git add bin/workshop01.sh
abcd03@abcd-workshop:~/workshop$
~~~
There is no output - a sign that the command was successful. In order to save a particular version of the file we need to `commit` the file. The first time we try to commit a file git will complain with a message that it does not know who we are:
~~~
abcd03@abcd-workshop:~/workshop$ git commit -m "initial checkin message"

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <abcd03@abcd-workshop.ucsd.edu>) not allowed
~~~
Run the two lines (copy & paste), fill in your email and your full name. Now try the `commit` line again.

If this worked try to change the file using `nano`. Add a comment as a second row, start the line with a hash-sign (# some comment). Add the file to git again and commit using a new message. Your changes have been saved in the .git directory and the git commands 'git log' for example can show you all the different versions.

One nice feature of git is that you cannot loose your work. If by chance you deleted the workshop01.sh file you can get it back using:
~~~
git checkout workshop01.sh
~~~
Try this out by:
~~~
abcd03@abcd-workshop:~/workshop$ cd ~/workshop/bin
abcd03@abcd-workshop:~/workshop$ rm workshop.sh
abcd03@abcd-workshop:~/workshop$ ls
abcd03@abcd-workshop:~/workshop$ git checkout workshop.sh
abcd03@abcd-workshop:~/workshop$ ls
~~~

Learn more about `git` (`git log`, `git blame`) and how you can check-out a specific version of the files stored in your local git repository.


# Module 1: Hypothesis Registration

In order to make hypothesis registration as easy as possible the DEAP platform supports the structured generation of hypothesis. This includes the definition of the hypothesis with variable selection and data transformations. Here are example text that could be added in the process of registering an hypothesis.

~~~
## Overview

The Adolescent Brain Cognitive Development (ABCD) Study is a long-term study of cognitive and brain development in children across the United States. A notable team of investigators, well known in the fields of adolescent development and neuroscience, has received funding from the National Institutes of Health to conduct this ambitious project. From 2016-2018, children between the ages of 9-11 have been invited to join the study from 21 sites around the nation, with the intent to enroll and follow approximately 11,500 healthy children longitudinally for 10 years into young adulthood.

Volkow, Nora D., et al. "The conception of the ABCD study: From substance use to a broad NIH collaboration." Developmental cognitive neuroscience (2017)
~~~

~~~
### Sampling Plan

An important motivation for the ABCD study is that its sample reﬂects the sociodemographic variation of the US population. With one important departure, the ABCD cohort recruitment emulates a multi-stage probability sample of eligible children: A nationally distributed set of 21 primary stage study sites, a probability sampling of schools within the deﬁned catchment areas for each site, and recruitment of eligible children in each sample school. The major departure from traditional probability sampling of U.S. children originates in how participating neuroimaging sites were chosen for the study. Although the 21 ABCD study sites are well-distributed nationally the selection of collaborating sites is not a true probability sample of primary sampling units but was constrained by the grant review selection process and the requirement that selected locations have both the research expertise and the neuroimaging equipment needed for the study protocol. As a consequence, neuroimaging research centers are more likely to be located in urban areas resulting in a potential under-representation of rural youth.

There is a substantial twin cohort in ABCD, consisting of monozygotic and dizygotic twin pairs, primarily collected at four of the 21 sites. There are also other sibling groups that will have been collected incidentally at all 21 sites. 

Garavan, H., et al. "Recruiting the ABCD sample: design considerations and procedures." Developmental cognitive neuroscience (2018).
~~~

~~~
### Design Plan

The ABCD study is a prospective longitudinal study with yearly assessments. The November 2017 public data release on the NIMH Data Archive (NDA) consists of baseline data from 4,524 children from 20 sites. Data collection efforts for ABCD are organized into several data categories. Overviews of neuroimaging data, neuropsychological measures, biospecimens, substance use assessments, and culture and environment are given in the citations below.

Lisdahl, Krista M., et al. "Adolescent brain cognitive development (ABCD) study: Overview of substance use assessment methods." Developmental cognitive neuroscience (2018).

Uban, Kristina A., et al. "Biospecimens and the ABCD Study: Rationale, Methods of Collection, Measurement and Early Data." Developmental cognitive neuroscience (2018).

Casey, B. J., Cannonier, T., Conley, M. I., Cohen, A. O., Barch, D. M., Heitzeg, M. M., ... & Orr, C. A. (2018). The Adolescent Brain Cognitive Development (ABCD) Study: Imaging Acquisition across 21 Sites. Developmental cognitive neuroscience.

Luciana, M., et al. "Adolescent neurocognitive development and impacts of substance use: Overview of the adolescent brain cognitive development (ABCD) baseline neurocognition battery." Developmental cognitive neuroscience (2018).

Zucker, Robert A., et al. "Assessment of culture and environment in the adolescent brain and cognitive development study: Rationale, description of measures, and early data." Developmental cognitive neuroscience (2018).
~~~

~~~
### Analysis Plan

Analyses should, if possible, reflect the study design of ABCD. In particular, there are a substantial number of siblings (including twins) in the study, and hence subjects should be nested within families. Moreover, we recommend including site as a random or fixed effect covariate in regression analyses. In DEAP, the Multilevel Module incorporates family and site as random effects, and defaults to including as fixed effect covariates the demographic categories that were used as metrics in recruitment to balance with the American Childhood Survey (ACS): age, sex at birth, race/ethnicity, household income, marital status of guardian, highest household education. 

Proper attention should be given to model assumptions. For example, normality of residuals or linearity of effects for regression models. If these assumptions are not met, consider data transformations or censoring of extreme outliers, or use of non-linear models.
~~~

~~~
### Analysis Scripts

We strongly recommend making analysis scripts publicly available along with any published results. DEAP allows for downloading and sharing of R scripts used in analyses. Scripts can be included as Supplementary Materials in published results. We also recommend uploading scripts to ABCD Github (https://github.com/ABCD-STUDY/).
~~~
