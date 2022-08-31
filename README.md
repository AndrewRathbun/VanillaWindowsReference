# VanillaWindowsReference

## Introduction

VanillaWindowsReference is a repo that contains recursive directory listings of a vanilla (clean) install of nearly every Windows OS version to compare and see what's been added with each update. MD5 and SHA1 values are also provided for every file in every CSV. 

## CSV Disclaimer

Each CSV will have a `test.csv` and `PsExec_IgnoreThisFile_ResearchTool.exe` that are to be ignored. `test.csv` is the output of the CSV you're looking at which is later renamed to accurately describe which Windows OS the CSV was generated from. PsExec was used to escalate privileges to hash as many files as possible during the generation of the directory listing. 

## PowerShell Command

In order to generate the CSV and the SystemInfo output, the following command can be used:

```cmd
C:\PsExec_IgnoreThisFile_ResearchTool.exe -accepteula -i -s cmd.exe /c powershell.exe "Get-ChildItem -Recurse 'C:\' | Where-Object { ! $_.PSIsContainer } | Select-Object DirectoryName, Name, FullName, Length, @{ N = 'CreationTimeUtc'; E = { (Get-Date -Format 's' $_.CreationTimeUtc).Replace('T', ' ') } }, @{ N = 'LastAccessTimeUtc'; E = { (Get-Date -Format 's' $_.LastAccessTimeUtc).Replace('T', ' ') } }, @{ N = 'LastWriteTimeUtc'; E = { (Get-Date -Format 's' $_.LastWriteTimeUtc).Replace('T', ' ') } }, Attributes, @{ N = 'MD5'; E = { (Get-FileHash $_.FullName -Algorithm MD5).Hash } }, @{ N = 'SHA1'; E = { (Get-FileHash $_.FullName -Algorithm SHA1).Hash } }, @{ N = 'SHA256'; E = { (Get-FileHash $_.FullName -Algorithm SHA256).Hash } }, @{ N = 'Sddl'; E = { (Get-Acl $_.FullName).Sddl } } | Export-Csv C:\test.csv -NoTypeInformation; systeminfo > C:\SystemInfo_.txt"
```

This command was expertly crafted by [Nasreddine Bencherchali](https://twitter.com/nas_bench). Huge thanks to him!

## CSV Preview

![W11CSVTimelineExplorerPreview](https://github.com/AndrewRathbun/VanillaWindowsReference/blob/main/SupportingFiles/W11CSVTimelineExplorerPreview.gif?raw=true)

## Use Cases

Have you ever wanted to know the hash of a Windows-native file for a given version to compare with what you're seeing as possibly suspicious on a system you're analyzing? Have you ever wanted to know where Windows-native executables reside for a given version of Windows? Have you ever wanted to make your own hash set but didn't want to generate the dataset yourself? 

These are but a few use cases for this repo! If you have more use cases, let us know and we can add them here! Or, do a Pull Request with your use case to this README.

## ChangeLog

2022-08-31 - SHA1 hashes were added (https://github.com/AndrewRathbun/VanillaWindowsReference/issues/2)
