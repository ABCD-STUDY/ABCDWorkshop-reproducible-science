---
title: "Image data access in ABCD"
author: "Hauke Bartsch"
teaching: 30
exercises: 0
questions:
- "What are the first steps after downloading data from ABCD FastTrack and MiniProc?"
objectives:
- "Explain the directory structure of the downloaded TGZ files"
keypoints:
- "The resulting BIDS directory structure contains the information required for local data analysis"
---

## Fast-Track data

The Fast-Track data is available from NDA as S3 buckets with the submission numbers 14921, 13124, and 14895. As new data is uploaded to NDA these number might change in the future.

Lets create another analysis script that should un-pack the T1 weigthed image series in our `/data` directory. The script would be very similar to the one that we already used to extract a single DICOM file. Instead we will now extract all files into the local directory. This might consume a substantial amount of hard-drive memory. For the small example we provide this will work but Fast-Track consists of more than 100,000 files and unpacking all of them into a single directory might be prohibitive.

We should unpack our data in a separate directory of our project. Currently we have a `bin` and a `source` directory. Lets add a `mydata` directory using:
~~~
abcd03@abcd-workshop:~/workshop$ mkdir ~/workshop/mydata
~~~

Create a new script in our `bin` directory with `nano ~/workshop/bin/workshop02.sh`.
~~~
!/usr/bin/env bash
# used to unpack all data in /data

cd ~/workshop/mydata/
find /data/* -name "*T1*.tgz" -print0 | while IFS= read -r -d $'\0' line; do
    tar xvf $line
done
~~~
As a first step we use `cd` to change to our `mydata` directory. The loop is the same as before, we use `tar` to unpack the files. Don't forget to make the script executable again with `chmod gou+x ~/workshop/bin/workshop02.sh`. Now start the script but use the `time` command to calculate the total running time of the operation: 
~~~
abcd03@abcd-workshop:~$ cd ~/workshop/bin
abcd03@abcd-workshop:~$ time ./workshop02.sh
~~~
The whole operation should take about 1.5sec. If you unpack all the files, instead of just the T1 weighted images the operation would take about 5min for the three example participants we are using in this tutorial. 

You can use tree to show the directory structure that has been created (use level -L 4 to not have all the DICOM files show up):
~~~
abcd03@abcd-workshop:~/workshop/mydata$ cd ~/workshop/mydata/
abcd03@abcd-workshop:~/workshop/mydata$ tree -L 4
.
├── sub-NDARINV28X60X09
│   └── ses-baselineYear1Arm1
│       └── anat
│           ├── ABCD-T1_run-20180312162234
│           └── ABCD-T1_run-20180312162234.json
├── sub-NDARINVXLYDYTLX
│   └── ses-baselineYear1Arm1
│       └── anat
│           ├── ABCD-T1_run-20180511164742
│           └── ABCD-T1_run-20180511164742.json
└── sub-NDARINVZ694ZZUM
    └── ses-baselineYear1Arm1
        └── anat
            ├── ABCD-T1-NORM_run-20180328183539
            ├── ABCD-T1-NORM_run-20180328183539.json
            ├── ABCD-T1_run-20180328183539
            └── ABCD-T1_run-20180328183539.json

13 directories, 4 files
~~~

The directory structure is similar to a Brain Imaging Data Structure (BIDS) directory structure. On the first level it lists the three participants. The directory underneath is specific to the imaging event. In our longitudinal ABCD study additional directories will be shared once the 2 year followup assessments start. The third directory level 'anat' indicates that these files belong to an anatomical scan (instead of DTI, fMRI, or resting state scan). Underneath are two files per image series. A `*.json` text file with meta information and a directory that contains the DICOM files for this series.

The BIDS directory structure encodes additional information in the file names of all data files. T1 images for example start with ABCD-T1, the number at the end is the data and time when this image series was aquired.

If we unpack additional TGZ files they will integrate into the already existing directory structures. The resulting directory structure would look like this:
~~~
abcd03@abcd-workshop:~/workshop/mydata$ tree -L 4
.
├── sub-NDARINV28X60X09
│   └── ses-baselineYear1Arm1
│       ├── anat
│       │   ├── ABCD-T1_run-20180312162234
│       │   ├── ABCD-T1_run-20180312162234.json
│       │   ├── ABCD-T2_run-20180312165839
│       │   └── ABCD-T2_run-20180312165839.json
│       ├── dwi
│       │   ├── ABCD-DTI_run-20180312165015
│       │   ├── ABCD-DTI_run-20180312165015.bval
│       │   ├── ABCD-DTI_run-20180312165015.bvec
│       │   └── ABCD-DTI_run-20180312165015.json
│       ├── fmap
│       │   ├── ABCD-Diffusion-FM_run-20180312164901
│       │   ├── ABCD-Diffusion-FM_run-20180312164901.json
│       │   ├── ABCD-fMRI-FM_run-20180312163301
│       │   ├── ABCD-fMRI-FM_run-20180312163301.json
│       │   ├── ABCD-fMRI-FM_run-20180312170605
│       │   ├── ABCD-fMRI-FM_run-20180312170605.json
│       │   ├── ABCD-fMRI-FM_run-20180312173045
│       │   ├── ABCD-fMRI-FM_run-20180312173045.json
│       │   ├── ABCD-fMRI-FM_run-20180312174510
│       │   ├── ABCD-fMRI-FM_run-20180312174510.json
│       │   ├── ABCD-fMRI-FM_run-20180312175849
│       │   └── ABCD-fMRI-FM_run-20180312175849.json
│       └── func
│           ├── ABCD-rsfMRI_run-20180312163501
│           ├── ABCD-rsfMRI_run-20180312163501.json
│           ├── ABCD-rsfMRI_run-20180312164141
│           ├── ABCD-rsfMRI_run-20180312164141.json
│           ├── ABCD-rsfMRI_run-20180312170707
│           ├── ABCD-rsfMRI_run-20180312170707.json
│           ├── ABCD-rsfMRI_run-20180312171337
│           └── ABCD-rsfMRI_run-20180312171337.json
├── sub-NDARINVXLYDYTLX
│   └── ses-baselineYear1Arm1
│       ├── anat
│       │   ├── ABCD-T1_run-20180511164742
│       │   ├── ABCD-T1_run-20180511164742.json
│       │   ├── ABCD-T2_run-20180511172216
│       │   └── ABCD-T2_run-20180511172216.json
│       ├── dwi
│       │   ├── ABCD-DTI_run-20180511171132
│       │   ├── ABCD-DTI_run-20180511171132.bval
│       │   ├── ABCD-DTI_run-20180511171132.bvec
│       │   ├── ABCD-DTI_run-20180511171132.json
│       │   ├── ABCD-DTI_run-20180511171655
│       │   ├── ABCD-DTI_run-20180511171655.bval
│       │   ├── ABCD-DTI_run-20180511171655.bvec
│       │   └── ABCD-DTI_run-20180511171655.json
│       ├── fmap
│       │   ├── ABCD-Diffusion-FM-AP_run-20180511171054
│       │   ├── ABCD-Diffusion-FM-AP_run-20180511171054.json
│       │   ├── ABCD-Diffusion-FM-PA_run-20180511170955
│       │   ├── ABCD-Diffusion-FM-PA_run-20180511170955.json
│       │   ├── ABCD-fMRI-FM-AP_run-20180511165601
│       │   ├── ABCD-fMRI-FM-AP_run-20180511165601.json
│       │   ├── ABCD-fMRI-FM-AP_run-20180511172750
│       │   ├── ABCD-fMRI-FM-AP_run-20180511172750.json
│       │   ├── ABCD-fMRI-FM-AP_run-20180511174328
│       │   ├── ABCD-fMRI-FM-AP_run-20180511174328.json
│       │   ├── ABCD-fMRI-FM-AP_run-20180511180857
│       │   ├── ABCD-fMRI-FM-AP_run-20180511180857.json
│       │   ├── ABCD-fMRI-FM-AP_run-20180511182437
│       │   ├── ABCD-fMRI-FM-AP_run-20180511182437.json
│       │   ├── ABCD-fMRI-FM-PA_run-20180511165516
│       │   ├── ABCD-fMRI-FM-PA_run-20180511165516.json
│       │   ├── ABCD-fMRI-FM-PA_run-20180511172701
│       │   ├── ABCD-fMRI-FM-PA_run-20180511172701.json
│       │   ├── ABCD-fMRI-FM-PA_run-20180511174220
│       │   ├── ABCD-fMRI-FM-PA_run-20180511174220.json
│       │   ├── ABCD-fMRI-FM-PA_run-20180511180812
│       │   ├── ABCD-fMRI-FM-PA_run-20180511180812.json
│       │   ├── ABCD-fMRI-FM-PA_run-20180511182334
│       │   └── ABCD-fMRI-FM-PA_run-20180511182334.json
│       └── func
│           ├── ABCD-rsfMRI_run-20180511165643
│           ├── ABCD-rsfMRI_run-20180511165643.json
│           ├── ABCD-rsfMRI_run-20180511170242
│           ├── ABCD-rsfMRI_run-20180511170242.json
│           ├── ABCD-rsfMRI_run-20180511172832
│           ├── ABCD-rsfMRI_run-20180511172832.json
│           ├── ABCD-rsfMRI_run-20180511173452
│           └── ABCD-rsfMRI_run-20180511173452.json
└── sub-NDARINVZ694ZZUM
    └── ses-baselineYear1Arm1
        ├── anat
        │   ├── ABCD-T1-NORM_run-20180328183539
        │   ├── ABCD-T1-NORM_run-20180328183539.json
        │   ├── ABCD-T1_run-20180328183539
        │   ├── ABCD-T1_run-20180328183539.json
        │   ├── ABCD-T2-NORM_run-20180328190543
        │   ├── ABCD-T2-NORM_run-20180328190543.json
        │   ├── ABCD-T2_run-20180328190543
        │   └── ABCD-T2_run-20180328190543.json
        ├── dwi
        │   ├── ABCD-DTI_run-20180328185159
        │   ├── ABCD-DTI_run-20180328185159.bval
        │   ├── ABCD-DTI_run-20180328185159.bvec
        │   └── ABCD-DTI_run-20180328185159.json
        ├── fmap
        │   ├── ABCD-Diffusion-FM-AP_run-20180328185055
        │   ├── ABCD-Diffusion-FM-AP_run-20180328185055.json
        │   ├── ABCD-Diffusion-FM-PA_run-20180328185018
        │   ├── ABCD-Diffusion-FM-PA_run-20180328185018.json
        │   ├── ABCD-fMRI-FM-AP_run-20180328183632
        │   ├── ABCD-fMRI-FM-AP_run-20180328183632.json
        │   ├── ABCD-fMRI-FM-AP_run-20180328190626
        │   ├── ABCD-fMRI-FM-AP_run-20180328190626.json
        │   ├── ABCD-fMRI-FM-AP_run-20180328191832
        │   ├── ABCD-fMRI-FM-AP_run-20180328191832.json
        │   ├── ABCD-fMRI-FM-AP_run-20180328193208
        │   ├── ABCD-fMRI-FM-AP_run-20180328193208.json
        │   ├── ABCD-fMRI-FM-AP_run-20180328194512
        │   ├── ABCD-fMRI-FM-AP_run-20180328194512.json
        │   ├── ABCD-fMRI-FM-PA_run-20180328183613
        │   ├── ABCD-fMRI-FM-PA_run-20180328183613.json
        │   ├── ABCD-fMRI-FM-PA_run-20180328190606
        │   ├── ABCD-fMRI-FM-PA_run-20180328190606.json
        │   ├── ABCD-fMRI-FM-PA_run-20180328191813
        │   ├── ABCD-fMRI-FM-PA_run-20180328191813.json
        │   ├── ABCD-fMRI-FM-PA_run-20180328193149
        │   ├── ABCD-fMRI-FM-PA_run-20180328193149.json
        │   ├── ABCD-fMRI-FM-PA_run-20180328194453
        │   └── ABCD-fMRI-FM-PA_run-20180328194453.json
        └── func
            ├── ABCD-rsfMRI_run-20180328183723
            ├── ABCD-rsfMRI_run-20180328183723.json
            ├── ABCD-rsfMRI_run-20180328184310
            ├── ABCD-rsfMRI_run-20180328184310.json
            ├── ABCD-rsfMRI_run-20180328190710
            ├── ABCD-rsfMRI_run-20180328190710.json
            ├── ABCD-rsfMRI_run-20180328191248
            └── ABCD-rsfMRI_run-20180328191248.json

72 directories, 62 files
~~~
You notice that for the diffusion series in the `dwi` directories additional `bval` and `bvec` text files have been shared.

### JSON files

The JavaScript Object Notation (JSON) files are frequently used file format in ABCD. They are machine readable text files, which are compact and easier to read than XML files. Our primary tools to work with them is the command line tool `jq` ([JQ]({{site.JQ}})). If we just want to list the content of a single JSON file we can change to the directory that contains the file (`cd`) and use `jq "." <filename>` to display the content:
~~~
abcd03@abcd-workshop:~$ cd ~/workshop/mydata/sub-NDARINVZ694ZZUM/ses-baselineYear1Arm1/anat
abcd03@abcd-workshop:~/workshop/mydata/sub-NDARINVZ694ZZUM/ses-baselineYear1Arm1/anat$ jq "." ABCD-T2_run-20180328190543.json
{
  "AccessionNumber": "",
  "AcquisitionLength": "TA 00.00",
  "AcquisitionMatrix": "[0, 256, 256, 0]",
  "ActiveCoils": "HEA;HEP",
  "Anonymized": "fast track v1.0, @DAIC April 2018",
  "ClassifyType": [
    "SIEMENS",
    "original",
    "ABCD-T2"
  ],
  "DateOfBirth": "20090315",
  "EchoTime": "565",
  "ImageType": "['ORIGINAL', 'PRIMARY', 'M', 'ND']",
  "IncomingConnection": "removed",
  "InstanceNumber": "1",
  "Manufacturer": "SIEMENS",
  "ManufacturerModelName": "Prisma_fit",
  "MeasID": "414",
  "Modality": "MR",
  "NumFiles": "removed",
  "NumSlices": "1",
  "OSLevel": "VE11B",
  "PatientID": "NDAR_INVZ694ZZUM",
  "PatientName": "NDAR_INVZ694ZZUM",
  "PatientSex": "F",
  "PatientsAge": "9.0",
  "PatientsSex": "",
  "PhaseEncodingDirectionPositive": "1",
  "RepetitionTime": "3200",
  "SOPClassUID": "MR Image Storage",
  "ScanningSequence": "SE",
  "SequenceName": "spc_200ns",
  "SequenceVariant": "['SK', 'SP']",
  "SeriesDescription": "SIEMENS, original, ABCD-T2",
  "SeriesInstanceUID": "1.3.12.2.1107.5.2.43.67084.2018032819054262586739723.0.0.0",
  "SeriesNumber": "17",
  "SeriesTime": "190543.077000",
  "SliceLocation": "-89.94140625",
  "SliceThickness": "1",
  "StudyDate": "20180328",
  "StudyDescription": "Adolescent Brain Cognitive Development Study",
  "StudyInstanceUID": "1.3.12.2.1107.5.2.43.67084.30000018032810040746400000034",
  "StudyTime": "182624.388000",
  "siemensDiffusionInformation": [],
  "siemensUUID": "Prisma_epi_moco_navigator_ABCD_tse_vfl.prot"
}
~~~
The content of this file has been created on the FIONA (Flash-based input/output network appliance) workstations at the data collection site using the DICOM meta data. FIONA's ClassifyType for example detected that this ABCD-T2 compatible image series is from a Siemens scanner.

Using our previous loop we can now display for example the series description for all unpacked image series.

Create another script in our `bin` directory:
~~~
abcd03@abcd-workshop:~$ cd
abcd03@abcd-workshop:~$ nano workshop/bin/workshop03.sh
~~~
Add this script:
~~~
!/usr/bin/env bash
# get the SeriesDescription for each JSON file in mydata

find ~/workshop/mydata -name "*.json" -print0 | while IFS= read -r -d $'\0' line; do
   jq -r ".SeriesDescription" $line
done
~~~
Compared to our previous scripts we are now looking for JSON files only. The option `-r` for `jq` removes some double-quotes which make the display harder to read.

Make the script executable and run it:
~~~
abcd03@abcd-workshop:~$ chmod gou+x workshop/bin/workshop03.sh
abcd03@abcd-workshop:~$ ./workshop/bin/workshop03.sh
SIEMENS, mosaic, original, ABCD-DTI
SIEMENS, original, ABCD-Diffusion-FM-AP
SIEMENS, original, ABCD-fMRI-FM-AP
SIEMENS, original, ABCD-fMRI-FM-AP
SIEMENS, original, ABCD-fMRI-FM-PA
SIEMENS, original, ABCD-fMRI-FM-PA
SIEMENS, original, ABCD-fMRI-FM-AP
SIEMENS, original, ABCD-fMRI-FM-AP
SIEMENS, original, ABCD-Diffusion-FM-PA
SIEMENS, original, ABCD-fMRI-FM-PA
SIEMENS, original, ABCD-fMRI-FM-AP
SIEMENS, original, ABCD-fMRI-FM-PA
SIEMENS, original, ABCD-fMRI-FM-PA
SIEMENS, mosaic, original, ABCD-rsfMRI
SIEMENS, mosaic, original, ABCD-rsfMRI
SIEMENS, mosaic, original, ABCD-rsfMRI
SIEMENS, mosaic, original, ABCD-rsfMRI
SIEMENS, original, ABCD-T1
SIEMENS, original, ABCD-T1-NORM
SIEMENS, original, ABCD-T2
SIEMENS, original, ABCD-T2-NORM
PHILIPS, original, ABCD-DTI
PHILIPS, original, ABCD-DTI
PHILIPS, original, ABCD-fMRI-FM-PA
PHILIPS, original, ABCD-fMRI-FM-PA
PHILIPS, original, ABCD-fMRI-FM-PA
PHILIPS, original, ABCD-fMRI-FM-PA
PHILIPS, original, ABCD-fMRI-FM-AP
PHILIPS, original, ABCD-fMRI-FM-AP
PHILIPS, original, ABCD-fMRI-FM-AP
PHILIPS, original, ABCD-fMRI-FM-AP
PHILIPS, original, ABCD-Diffusion-FM-AP
PHILIPS, original, ABCD-Diffusion-FM-PA
PHILIPS, original, ABCD-fMRI-FM-PA
PHILIPS, original, ABCD-fMRI-FM-AP
PHILIPS, original, ABCD-rsfMRI
PHILIPS, original, ABCD-rsfMRI
PHILIPS, original, ABCD-rsfMRI
PHILIPS, original, ABCD-rsfMRI
PHILIPS, original, ABCD-T2
PHILIPS, original, ABCD-T1
GE, original, ABCD-DTI
GE, original, ABCD-fMRI-FM
GE, original, ABCD-fMRI-FM
GE, original, ABCD-fMRI-FM
GE, original, ABCD-Diffusion-FM
GE, original, ABCD-fMRI-FM
GE, original, ABCD-fMRI-FM
GE, original, ABCD-rsfMRI
GE, original, ABCD-rsfMRI
GE, original, ABCD-rsfMRI
GE, original, ABCD-rsfMRI
GE, original, ABCD-T1
GE, original, ABCD-T2
~~~

Can you run the workshop03.sh script again and remove the duplicates?

## MiniProc data

The minimally processed data also fits into the Fast-Track directory structure. Instead of the DICOM files we share volume files in the Nifti file format (BIDS). Both Fast-Track and MiniProc files can be unpacked in the same directory structure.

## Running MMPS using docker

Docker is available as a service for the abcd user account available for the workshop. Start by listing all the images that you have access to:
~~~
abcd03@abcd-workshop:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
ubuntu              latest              113a43faa138        10 days ago         81.2MB
~~~
This shows that docker has an image called ubuntu. We can run this image simply by calling it with `docker run` using an interactive terminal (-it):
~~~
abcd03@abcd-workshop:~$ docker run -it ubuntu date
Sat Jun 16 06:51:13 UTC 2018
~~~
In this example we have started the `date` command inside the docker container. It ran, produced the current date and the docker container closed again. This behavior makes docker container behave like programs. They are very fast to start.

Actually the docker contain left a trace on our system that is not visible in using the `docker image` command. It did create an object which is visible using
~~~
abcd03@abcd-workshop:~$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
7828d0a38bfb        ubuntu              "date"              2 minutes ago       Exited (0) 2 minutes ago                       clever_banach
~~~

These objects stay after the program finished running and can use up disk space. Especially if we are running many jobs during batch processing these objects can slow down our system and fill up the drive. We can automatically clean up these containers by adding the command line flag `--rm`:
~~~
abcd03@abcd-workshop:~$ docker run -it --rm ubuntu date
Sat Jun 16 06:58:08 UTC 2018
~~~
In this case the container will be removed after the command is finished.

Running larger programs in a docker container like MMPS require that the docker container has access to the file system outside the container. Without this access all data that is created while the docker container is running would disappear again after the docker container is closed.

A simple way to keep the work done in a docker container is to use a second shell to `commit` a currently running docker container. A committed container becomes a new images that can be started again at a later point - with the data from the moment the `commit` command was run.

We can test this behavior if we run a bash shell inside our container.
~~~
abcd03@abcd-workshop:~$ docker run -it --rm ubuntu /bin/bash
root@a5f43effee5f:/#
~~~
As you can see starting `/bin/bash` as the program inside the container presents us with a new prompt. Anything we do inside this container will disappear once we enter `exit`.

