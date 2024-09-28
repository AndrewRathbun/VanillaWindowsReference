# File Path Hash Sets

These can be used with MFTECmd to filter out known file and folder paths native to a clean Windows install. Think of this as similar to applying a hash set against a forensic image in your forensic tool of choice, except this is purely based on file and folder names instead of a hashing algorithm. The intent is to reduce the noise in MFTECmd's CSV output to filter out files that Microsoft creates for it various Windows SKUs. 

Please note, these are generated from all CSVs that exist in https://github.com/AndrewRathbun/VanillaWindowsReference as of September 2024. Currently, the cutoff of versions supported in this dataset doesn't extend beyond Windows SKUs released after March 2023. See below for a high-level summary:  
  
* .\Windows8\W8.1_Pro_9600
* .\Windows8\W8_Pro_20130909_9200
* .\Windows10\20H2
* .\Windows10\21H1
* .\Windows10\21H2
* .\Windows10\22H2
* .\Windows10\1507
* .\Windows10\1511
* .\Windows10\1607
* .\Windows10\1703
* .\Windows10\1709
* .\Windows10\1803
* .\Windows10\1809
* .\Windows10\1903
* .\Windows10\1909
* .\Windows10\2004
* .\Windows11\21H2
* .\Windows11\22H2
* .\WindowsServer\2016
* .\WindowsServer\2019
* .\WindowsServer\2022

## Files Generated

### UniqueDirectoryAndNames.csv

![image](https://github.com/user-attachments/assets/64f3229e-4b36-4ff5-8697-12d1ce184d8f)

### UniqueDirectoryNames.csv

The same as above, but with Directory Name values only.

### UniqueNames.csv

The same as above, but with Name values only.
