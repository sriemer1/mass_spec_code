# Guide to Mass Spectrometer Code
## mass_spec_data.py
Code to analyze RGA data

###Specs
- bullet Language used: Python
- bullet Written and tested using Enthought Canopy software, a python compiler

###Overview
- bullet The code takes two files from the working directory (presumably where the desirable files are). The first file is the gas scan (background) file, with the masses and intensities for the background scan. The second file is the plasma scan, with the masses and intensities for when the spectrometer is run when the plasma is on.
- bullet Next, the code generates two graphs using a python library called matplotlib. The first graph is a plot of all of the differences between the plasma and background data. The second graph is a graph of all the intensities for both the plasma and background data. 
- bullet The user then has the option to have the data analyzed and tables generated, or to end the program and just have the graphs. If the user chooses to have the data analyzed, they are asked for a certain error and three files are generated: one with the peaks and their corresponding masses, one with the differences that meet the minimum error, and one with the differences at the corresponding peak masses.
- bullet At the end the user can choose to run the program again for the same set of files with different settings (error, zoom, etc.) or for a different set of files.

###Imports
- bullet The first ten or so lines of the program import the python packages necessary for certain things (to be mentioned later).
- bullet Import csv  allows the program to read in comma separated values files
- bullet Import matplotlib imports the package that generates the graphs
-bullet From pylab import * tells the program to import all functions from pylab, which I use to make the graphs have a logarithmic scale.
- bullet From python website: ”NumPy is a general purpose array processing package designed to efficiently manipulate large multi-dimensional arrays of arbitrary records without sacrificing too much speed for small multi-dimensional arrays.” (I only use it on 1 out of ~230 lines of code)
- bullet Math is a python module that has certain useful mathematical functions like sqrt()
- bullet Sys allows me to use sys.exit, which exits the code prematurely
- bullet Datetime and time are used to get the current date and time (used for naming files)
- bullet os is used to look through current working directory for files

###Finding and reading in the background data
- bullet After the spectrometer is run the files are saved as text (.txt) files into a folder. That folder should be the current working directory in Canopy, which you can change in the lower right hand corner of the Canopy screen.
- bullet The background text files are saved in the format: ___ torr gas scan runNumber
- bullet The user enters the torr run number of the gas (same torr for plasma), flow rate, and the date the file was made and the program reads in the file with those specifications

