<img src="https://github.com/geoportti/Logos/blob/master/geoportti_logo_300px.png">

# Use of Taito

This article gives the general overview of how to use the CSC's Taito supercomputer. For more detailed information visit [CSC's Taito user guide](https://research.csc.fi/taito-user-guide).

## Getting credentials

First thing you need is to have credentials to the Taito and have an active computing project from CSC. Information on obtaining those can be found at [here](https://research.csc.fi/csc-guide-projects-and-resource-allocation).

## Connecting to Taito

Taito does not offer a graphical user interface, and the user must connect to it using SSH. On Windows the de facto program is [PuTTY](https://www.putty.org). On Unix-like systems any terminal will do, simple give the command
```
ssh username@taito.csc.fi
```
More information on connecting to Taito [here](https://research.csc.fi/taito-connecting).

Once you have successfully connected to the Taito, you are presented with a welcome screen with various information.

## Software

Taito has numerous programs and libraries available for everyone. Many of them are conflicting each other and cannot be used at the same time. Therefore users must load the programs and libraries that they need. This is done by loading the appropriate **modules**. The available modules and information about them can be queried with the command
```
module spider
```
More info about the modules [here](https://research.csc.fi/taito-module-system).

## Queue

The main difference between using Taito and a basic desktop machine is that Taito consists of several nodes. When you connect to Taito you actually connect to one of the special login nodes. The actual computations are performed on computing nodes. This means that you must explicitly send your job to the computing nodes.

If every user could send their jobs directly into computation, the system would slow down or crash as soon as the submitted jobs together acquire more resources than the system has. This problem is solved by using a job queue system. The users write a specific batch job files that tell, among other things, how much memory and cores their calculation requires, and how long it will need to run. This information, in addition to the actual command that performed the calculation, is written to a batch job file. This file is then submitted to the queue, and the queue system decides when there are enough resources available for the job to run. Instruction for writing batch job files can be found [here](https://research.csc.fi/taito-batch-jobs).

Compared to working on a local desktop machine, the users to Taito cannot know exactly when their jobs will start to run. The queue system is configured to be as fair as possible, but sometimes a user must wait for a long time before obtaining results.

## Input files

Let's assume that you have a Python script *script.py* that you would like to run on Taito. The required data is already in Taito, and the required libraries are found in the module **geoconda**.

The different file paths in the Python script must be adapted to the Taito. The output directory of the script should be changed to your working directory. To find this you can give the following command in the terminal:
```
echo $WRKDIR
```
Make sure that the script writes everything under this directory.

An example batch job file can be for example *submit.sh* that contains the following lines:
```
#!/bin/bash -l 
#SBATCH -J test_script
#SBATCH -o output.txt
#SBATCH -e errors.txt
#SBATCH -t 00:30:00
#SBATCH -p serial
#
module load geoconda

python script.py
```
Submit the *submit.sh* to the queue with the following command:
```
sbatch submit.sh
```
This puts the job into the queue, and the queue system will eventually send to job to the calculation nodes.

## Workflow

The basic workflow in the Taito environment is
- Connect to Taito
- Copy the scripts and necessary data into Taito
- Modify the script for Taito environment
  - Change paths etc.
- Write a batch job file for the script
  - Estimate the resource requirements of the job
  - Load the required modules
- Submit the job to the queue
- Wait for the results

## More information

You can find more detailed information on using Taito from [CSC's Taito user guide](https://research.csc.fi/taito-user-guide).

## Usage and Citing

When used, the following citing should be mentioned: "We made use of geospatial
data/instructions/computing resources provided by the Open Geospatial
Information Infrastructure for Research (oGIIR,
urn:nbn:fi:research-infras-2016072513) funded by the Academy of Finland."
