# AIOP

This repo has the code that is currently being used for my portion of the AIOP project at Jefferson Lab. 

The following outlines the steps to use the code. 

1) The .txt files in myGetCommands are the commands I use to access the archived EPICS data for the Spring 2020 run period. These must be run on one of the gluon machines. gluons150-155 are for general use. 

2) cleanAndConvertData_txt_to_csv.py converts the resulting .txt files to csv files, while also handling any "<< Network disconnection >>" messages. It creates a .csv file with two variables, the new setpoint of the variable of interest, as well as the Date-Time when the new value was set. The Date-Time is put into a datetime friendly format, so they can be used for time differences later. Some variables are readout more than once per second, but Date-Time only comes in second increments. In these cases, we take the first measured value for each second. We do this because there is no guarrantee the measured values happen at even intervals, so this reduces our data footprint while also ensuring we don't cause an issue by interpolating times. 

3) merge_all_csv.ipynb is used to merge the resulting csv files for all the variables. Every time a variable is changed, the archived data includes the new value as well as a time stamp for when it changed. We want to keep all values regardless of the time they occurred, so we have to do an outer join on Date-Time for all of these datasets. This results in a large number of NaN's, since most of the variable values change at different times. The majority of variables are set points for the controls, which we know can be forward filled since the NaN's will continue until a new set point was given. For readback (RBV) values, forward filling also gives a "reasonable" guess at the value until a new value is read in. 

This notebook also has to fix the run number. By default, a new run number is assigned during the data acquisition (DAQ) prestarting phase (DAQ:STATUS 1). We want each run to capture every reading since the previous run ended instead, so that we capture all the relevant configuration changes. 

For each run, we also mark whether it is a production run, ie whether it is in the standard RCDB query for the Spring 2020 dataset. 

Finally, we look into the sequences of nudges that occur before/during the run. Every time a new pitch/yaw setpoint is given, we increment the nudge_count. We also track whether nudges occurred while the DAQ was running (which they never should). 


4) data_analysis.ipynb is the notebook where we perform the main data analysis tasks. We filter out only the good data-taking runs, and separate them into runs that involved a "nudge" vs. those that do not. The runs that do not include a nudge are used to investigate how the electron and photon beam x,y positions are correlated with the ultimate photon beam energy that comes out. The runs with nudges will ultimately be used to train our Gaussian-process ML model on how nudge sequences can give us the desired photon beam energy.  