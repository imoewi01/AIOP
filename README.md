# AIOP

This repo has the code that is currently being used for my portion of the AIOP project at Jefferson Lab. 

The following outlines the steps to use the code. 

1) The .txt files in myGetCommands are the commands I use to access the archived EPICS data for the Spring 2020 run period. These must be run
on one of the gluon machines. gluons150-155 are for general use. 

2) cleanAndConvertData_txt_to_csv.py converts the resulting .txt files to csv files, while also handling any "<< Network disconnection >>" messages.
It creates a .csv file with two variables, the new setpoint of the variable of interest, as well as the Date-Time when the new value was set. The
Date-Time is put into a datetime friendly format, so they can be used for time differences later.

3) merge_csv.ipynb is used to merge the resulting csv files for all the control setpoint values. Every time a variable is changed, the archived data includes the 
new set point as well as a time stamp for when it changed. We want to keep all new setpoint values, so we have to do an outer join on all of these datasets.
This outerjoin is done on the Date-Time column. This results in a large number of NaN's, since most of the variable set points are changed at different times.
Since these are all control setpoint values, we know we can forward fill existing setpoint values to future times, since they are recorded every time they change. 

