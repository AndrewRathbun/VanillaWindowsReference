# Windows Server 2019 (1809) Build 17763.1282 Directory Listing

Release date: 20200609

Official documentation: https://support.microsoft.com/en-us/topic/june-9-2020-kb4561608-os-build-17763-1282-437af506-e3ef-a8a1-09e7-26cc94e509c7

## Actions Taken Prior to Generating Directory Listing

1. Copied `PsExec_IgnoreThisFile_ResearchTool.exe` to the root of C:\
2. Opened `CMD` or `PowerShell` (Admin) and typed the commands listed below

## Commands Ran

```
cd C:\
PsExec_IgnoreThisFile_ResearchTool.exe -s -i PowerShell.exe
```

The above command will execute PsExec (renamed `PsExec_IgnoreThisFile_ResearchTool.exe`) and escalate privileges from `HOSTNAME\user` to `NT AUTHORITY\SYSTEM`. This allows for more system files to be hashed properly in the next command.

```PowerShell
Get-ChildItem -Recurse "C:\" | Where-Object { ! $_.PSIsContainer } | Select-Object DirectoryName,Name,FullName,Length,@{N='CreationTimeUtc';E={(Get-Date -Format "s" $_.CreationTimeUtc).Replace("T", " ")}},@{N='LastAccessTimeUtc';E={(Get-Date -Format "s" $_.LastAccessTimeUtc).Replace("T", " ")}},@{N='LastWriteTimeUtc';E={(Get-Date -Format "s" $_.LastWriteTimeUtc).Replace("T", " ")}},Attributes,@{N='MD5';E={(Get-FileHash $_.FullName -Algorithm MD5).Hash}},@{N='SHA256';E={(Get-FileHash $_.FullName -Algorithm SHA256).Hash}},@{N='Sddl';E={(Get-Acl $_.FullName).Sddl}} | Export-Csv C:\test.csv -NoTypeInformation
```

The above command will provide a recursive listing of all files from the C:\ drive including the Parent Path, File Name, Full File Path, Size in Bytes (Length), Creation Time (UTC), Last Access Time (UTC), Last Write Time (UTC), and Attributes associated with the file. Additionally, the MD5 and SHA256 hash will be populated for all but very few files in the list. The command will output to `test.csv`. 

Yes, you may see these files as well as other files related to the user profile on the VM that was created using Easy Install with VMWare Workstation Pro. Obviously, these files are not expected on a vanilla Windows install, but they are unavoidable without modifying the output which is disingenuous to what this repo is attempting to accomplish.

## Differences Between Previous Windows OS Version

Differences go here *work in progress*
