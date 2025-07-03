# AIOP

This repo has the code that is currently being used for my portion of the AIOP project at Jefferson Lab. All files are works in progress, and may not be clearly commented or cleanly written at the moment. 

The following outlines the steps to use the code. A more comprehensive description exists in AIOP_CoherentEdgePosition.pdf.

1) The .txt files in myGetCommands are the commands I use to access the archived EPICS data for the Spring 2020, 2023, and 2025 run periods. These must be run on one of the gluon machines. gluons150-155 are for general use. 

2) cleanAndConvertData_txt_to_csv.py converts the resulting .txt files to csv files.

3) merge_all_csv.ipynb is used to merge the resulting csv files for all the variables. It also corrects the run numbers, and defines the nudge sequence variables. 

4) add_beam_up_time_combined.ipynb: Adds a variable for the time since the last beam drop. A beam drop is defined as any second when the beam current is below 30 nA. 


The remaining notebooks are used to visualize different classes of nudge events. They include 

a) nonudge-study_combined.ipynb: Used to look at correlations between electron beam and photon beam parameters for each run period.
b) singlenudge-study.ipynb: Used to visualize nudge sequences and estimate the energy change per nudge 
c) multinudge_combined.ipynb: Used to get a much better estimate of the energy change per nudge by fitting a plot of energy change vs number of nudges. Also visualize the nudge sequences as time series plots 
d) backlash_study_combined.ipynb: Used to investigate instances of backlash in the goniometer. 

