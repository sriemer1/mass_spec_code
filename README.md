# Guide to Mass Spectrometer Code
## mass_spec_data.py
Code to analyze RGA data

###Specs
- Language used: Python
- Written and tested using Enthought Canopy software, a python compiler

###Overview
- The code takes two files from the working directory (presumably where the desirable files are). The first file is the gas scan (background) file, with the masses and intensities for the background scan. The second file is the plasma scan, with the masses and intensities for when the spectrometer is run when the plasma is on.
- Next, the code generates two graphs using a python library called matplotlib. The first graph is a plot of all of the differences between the plasma and background data. The second graph is a graph of all the intensities for both the plasma and background data. 
- The user then has the option to have the data analyzed and tables generated, or to end the program and just have the graphs. If the user chooses to have the data analyzed, they are asked for a certain error and three files are generated: one with the peaks and their corresponding masses, one with the differences that meet the minimum error, and one with the differences at the corresponding peak masses.
- At the end the user can choose to run the program again for the same set of files with different settings (error, zoom, etc.) or for a different set of files.

###Imports
- The first ten or so lines of the program import the python packages necessary for certain things (to be mentioned later).
- Import csv  allows the program to read in comma separated values files
- Import matplotlib imports the package that generates the graphs
- From pylab import * tells the program to import all functions from pylab, which I use to make the graphs have a logarithmic scale.
- From python website: ”NumPy is a general purpose array processing package designed to efficiently manipulate large multi-dimensional arrays of arbitrary records without sacrificing too much speed for small multi-dimensional arrays.” (I only use it on 1 out of ~230 lines of code)
- Math is a python module that has certain useful mathematical functions like sqrt()
- Sys allows me to use sys.exit, which exits the code prematurely
- Datetime and time are used to get the current date and time (used for naming files)
- os is used to look through current working directory for files

###Finding and reading in the background data
- After the spectrometer is run the files are saved as text (.txt) files into a folder. That folder should be the current working directory in Canopy, which you can change in the lower right hand corner of the Canopy screen.
- The background text files are saved in the format: ___ torr gas scan runNumber
- The user enters the torr run number of the gas (same torr for plasma), flow rate, and the date the file was made and the program reads in the file with those specifications
- If no run number is entered the program uses a  “default” background scan file
- * If no file is found the program will tell you and you will have to run the program again with a proper file name


