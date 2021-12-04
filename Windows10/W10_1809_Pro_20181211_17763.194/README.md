# Windows 10 (1809) Build 17763.194 Directory Listing

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
Get-ChildItem -Recurse "C:\" | Where { ! $_.PSIsContainer } | Select DirectoryName,Name,FullName,Length,CreationTimeUtc,LastAccessTimeUtc,LastWriteTimeUtc,Attributes,@{N='MD5';E={(Get-FileHash $_.FullName -Algorithm MD5).Hash}},@{N='SHA256';E={(Get-FileHash $_.FullName -Algorithm SHA256).Hash}},@{N='Sddl';E={(Get-Acl $_.FullName).Sddl}} | Export-Csv C:\test.csv -NoTypeInformation
```

The above command will provide a recursive listing of all files from the C:\ drive including the Parent Path, File Name, Full File Path, Size in Bytes (Length), Creation Time (UTC), Last Access Time (UTC), Last Write Time (UTC), and Attributes associated with the file. Additionally, the MD5 and SHA256 hash will be populated for all but very few files in the list. The command will output to `test.csv`. 

Yes, you may see these files as well as other files related to the user profile on the VM that was created using Easy Install with VMWare Workstation Pro. Obviously, these files are not expected on a vanilla Windows install, but they are unavoidable without modifying the output which is disingenuous to what this repo is attempting to accomplish.

## Differences Between Previous Windows OS Version

Differences go here *work in progress*

# Official Documentation

https://support.microsoft.com/en-us/topic/december-11-2018-kb4471332-os-build-17763-194-033f5756-5e3b-314f-a2e8-531d6fc21325
