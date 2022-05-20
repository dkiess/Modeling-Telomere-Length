# Modeling-Telomere-Length
Modeling telomere length compared to age using data from the 2001-2002 NHANES performed by the CDC.

This README file contains an overview of each component of this inquiry project for F block Molecular Genetics. 

Before running the program for yourself, you may need to install the python library 
I recieved help from Victor Hoppenot, a classmate an long time friend of mine, who helped me with some of the data preparation work I did to the data from the CDC. 

I obtained the data I used to produce my model from the CDC's National Health and Nutrition Examination Survey (NHANES). The NHANES collected telomere data from particpants from 1999 to 2002 and makes them available for everyone to download. I used the data from the 1999-2000 NHANES, so I downloaded TELO_A.XPT, the document containing the telomere data and the reference number of the participant. I also downloaded DEMO.XPT, which contains the age adn partcipant reference number data. 

Before runnig program, I used the xport python library to convert the dataset from a SAS Transport file (.xpt) to a Comma Sepparated Values file (.csv). 
I obtained this from the CDC's NHANES, which is free and downloadable by the public, however all of the files are in the .xpt format.
I needed to do this because python does not have native support for SAS Transport files. Pthon does support .csv files natively.
I performed this by running some simple command line prompts shown below:

C:\\Users\Dkiess> pip install xport                           # this installs the xport library to my computer so that I can use it
C:\\Users\Dkiess> cd Downloads                                # change directories to where my file is stored. I had mine in the downloads folder
C:\\Users\Dkiess\Downloads> dir                               # this lists all of the files in the current directory. I did this to confirm that my .xpt file was there and its name
C:\\Users\Dkiess\Downloads> python -m TELO_A.XPT > telo_a.csv # this uses the xport library in python that i installed earlier to convert my file from .xpt to .csv

This massive .csv file has the data that I will be using, however, it also has a lot of extra data that I will not be using, 
so I created a new Excel document and copied the columns I needed from the SSAFB file to it.
These columns were SEQN and TELOMEAN which are the reference number of the person and the T/S ratio of the person. 
The last step before this file has the telomere data is to convert from T/S ratio to base pairs. The NHANES documentation provides this. The formula is 3274 + 2413x where x is the T/S ratio.
Next, I need to get the age of the participant from the reference number. This is stored in the Demographics sectino of NHANES, so I downloaded and converted that file. 
Now I have 2 files, telo_a.csv, which has the telomere data, and demo.csv, which has the age data. Both of these use the SEQN list to show which participant the data is from.
I enlisted the help of Victor Hoppenot who graciously helped me merge the 2 .csv files into the 'telomeres.csv' file using the R programing language. 
After merging the files, I deleted all of the extra columns I wasn't using from telomeres.csv. This reduced the size from over 20 MB to under 115 kB in size. 
Now the data is finally ready to be used in the rest of the program which will be commented with in line comments.

The new .csv file has 4 columns, SEQN, RIDAGEYR, TELOMEAN, and TELOBASSE. 
    SEQN        Reference number of the participant
    RIDAGEYR    Age of the participant
    TELOMEAN    T/S ratio of the participant
    TELOBASE    Base pair length of the participant's telomeres
    
The program itself uses sklearn to form a linear regression on the data set itself as well as find the 90% prediction interval the data.
