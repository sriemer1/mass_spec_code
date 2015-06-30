# Guide to Mass Spectrometer Code
## mass_spec_data.py
Code to analyze RGA data

*** Scroll to the bottom to see a sample run.

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
- The program skips the header in the gas file and creates two lists, one for the masses (mass) and another for the intensities (intensityB)
- Reading the file as a csv file, the program stores the masses in the first column into the list called mass, and the intensities in the second column into a list called intensityB
- These lists will be used later to analyze the data and make new files 

###Finding and reading in the plasma data
- The same process that was used for the background data is used for the plasma data except the plasma data doesn’t have a run number, so when looking for the plasma files only the specified torr and date is searched for. 
- Furthermore, a new list for the masses need not be created since the masses are the same in both the background and plasma data.
- The list of intensities for the plasma is called intensityP
- Two new lists called intensityP_ and massP are created. These lists store the masses and the absolute value for intensities which have mass values that end in .90, .00, .10, or .20

###Plotting the data
- As mentioned in earlier slides, a tool called matplotlib is used to generate the graphs.
- Since two graphs are being made, one for the differences and another for the intensities, two figures must be defined in the code (fig1-differences and fig2-intensities).
- Although once the graph is generated you can zoom in or out as much as you’d like, the program gives the user the option to generate an intensities graph within certain x and y limits of their choice. (This feature can be removed or added for the differences as well if we want).
- The user can choose to only change some of the x and y limits, and use the default value for others.
- X and Y axis labels can be changed 
- Color of the data can be changed
- allDiffs is a list that contains the differences
- Mass is on the x axis, intensity or diff. on y
- A log scale is used: (log = True) and pyplot.yscale(‘log’)
- The graph of the differences is a bar graph, but you can uncomment the last line of that block to also be able to see the curve.

###Getting data for the file of differences
- If the user just wanted to generate graphs they have the option to stop the program after the graphs have been made. If not three files are made. The first one is the file with the differences.
- The user is asked to give an “error”. This is the minimum percent difference between the background and plasma data that we want to see data for. The differences file created will only contain the masses and their corresponding differences that meet this minimum error (errors need to be entered as decimals!)
- The differences are calculated by subtracting the background data from the plasma data.
    Example: In plasma data the intensity at 1 amu is 8.11E-9, in background data the                         intensity at 1 amu     is 7.57E-9, so the difference at 1 amu is 5.4E-10
- Since the data we care most about is at the peaks, and the peaks usually hover around integers and x.9, x.1,x.2, there is a loop that only calculates the differences at the mass values.
- Then it only adds a difference to the list if it reaches the minimum percent difference

###Getting data for the file of peak intensities
- Since the peaks are typically found only +.2, -.1 away from an integer mass, the program uses the list intensityP_ to find the largest intensities. 
- There is a loop that looks through intensityP_ in groups of four (.9,.0,.1,.2) and pulls out the max intensity and its corresponding mass and puts them into a list called peaks. There is a counter that keeps track of the index of the max intensity in intensityP_, so that we can use this counter to get the corresponding mass for that intensity. These masses are also added to the list peaks.
- Since for the integer masses 1 and 100 there are only 3 and 2 possible intensities, respectively, in the +.2 -.1 range, these values are analyzed separately. That is also why the loop that looks through the bulk of the data starts at index 3 and ends before 99.9 and 100 (goes to the length intensityP_ - 2). These intensities and corresponding masses are also added to peaks.

###Getting the differences at the peaks
- In order to find the difference between the background and plasma data at the peaks, the program looks at all the differences in differences, and all of the peaks in peaks. All of the masses for the peaks can be found in differences, but not all of the masses for the differences can be found in peaks. 
- In order to compare the masses to see which ones are contained in both lists, the two files created earlier are read into the program and two lists are created from these files, one called massDiffs, which contains the masses from the differences file, and massPeaks.
- The differences are also read in and stored in a list called diffs, since the new file will need some of these differences.
- Then, a loop looks through both of the lists (massPeaks and massDifferences), and if any value in massDifferences is also in massPeaks, it is added to a list called peakMassTemp along with its corresponding difference.
- peakMassTemp is a temporary list containing duplicates for each mass and difference which comes from using the for loop. Therefore a new list called peakMass is created outside the loop which only takes the unique values (this is where the program uses the numpy “unique” function).
- The values in peakMass, which are the masses and the differences at the peaks corresponding to those masses, are written to a new file with the same naming convention as the other two, except it ends in peak_differences

###Making the files
- Naming convention for all the files created in this program: date time torr error (differences_data, peak_data), run number
- Every value in differences (which contains the difference and the corresponding mass) is written to a new file, and every value in peaks (which contains the peak intensity and corresponding mass) is written to another new file
- The new files are saved in a new folder on the desktop. In that new folder another new folder is created with the name “Data tables flow rate, torr, run numer (if applicable)” and then the three files generated from the program are put into this folder.

###Fourth File
- The final file created is a file with various peak differences for run numbers of your choosing. 
- At the beginning the code asks Enter the run numbers you would like to see peak data for: 
- The code then creates a new file with the peak differences at these run numbers to allow for preliminary comparisons before quantitatively analyzing the data.

###What the program does
- Total data files read in: 2
- Total graphs created: 2
- Total files created: 4

##***Sample Run
- Enter the date of the scan (mm-dd-yyyy): Enter the date that the plasma scan was run
- Enter torr: Enter the torr used for the plasma scan
- Enter flow: Enter the flow used for the plasma scan
- Do you want to use the default background scan (y/n)? If you type 'y', the program uses the chosen default scan file,            06-02-2015 1-100 20sccm 3 torr gas scan.txt. If you type 'n' there are further prompts:
    - Enter run number for background: Enter the run number of the background. If there is no run number, just hit enter
    - Enter date for background: Enter the date the background scan was run
- Enter run number for plasma: Enter the run number of the plasma scan. If there is no run number, hit enter
- Enter the run numbers you would like to see peak data for: Enter the run numbers. If you do not want to generate this file for a particular program run, hit enter
- Would you like to set any parameters for the min/max x and y values (y/n)? Enter 'y' if you want to have the generated graph be zoomed in to a certain area. Enter 'n' if you just want the default plot size.
- Would you like to generate data tables (y/n)? Enter 'y' if you want to see the data analysis (differences, peaks, diffs. at peaks). Enter 'n' if you just want to see the plots for a particular run. Entering 'n' will end the program.
- Enter error as a decimal: Enter the error you want to see differences data for
- Data generated, would you like to run another set of data (y/n)? Enter 'y' if you want to run the program again, 'n' if not. Entering 'n' will end the program.



















