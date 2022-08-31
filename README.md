# VanillaWindowsReference

## Introduction

VanillaWindowsReference is a repo that contains recursive directory listings of a vanilla (clean) install of nearly every Windows OS version to compare and see what's been added with each update. MD5 and SHA1 values are also provided for every file in every CSV. 

## CSV Disclaimer

Each CSV will have a `test.csv` and `PsExec_IgnoreThisFile_ResearchTool.exe` that are to be ignored. `test.csv` is the output of the CSV you're looking at which is later renamed to accurately describe which Windows OS the CSV was generated from. PsExec was used to escalate privileges to hash as many files as possible during the generation of the directory listing. 

## CSV Preview

![W11CSVTimelineExplorerPreview](https://github.com/AndrewRathbun/VanillaWindowsReference/blob/main/SupportingFiles/W11CSVTimelineExplorerPreview.gif?raw=true)

## Use Cases

Have you ever wanted to know the hash of a Windows-native file for a given version to compare with what you're seeing as possibly suspicious on a system you're analyzing? Have you ever wanted to know where Windows-native executables reside for a given version of Windows? Have you ever wanted to make your own hash set but didn't want to generate the dataset yourself? 

These are but a few use cases for this repo! If you have more use cases, let us know and we can add them here! Or, do a Pull Request with your use case to this README.
