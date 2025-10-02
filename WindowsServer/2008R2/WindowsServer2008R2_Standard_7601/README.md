# Windows Server 2008 R2 SP1 Build 7601 Directory Listing

## Actions Taken Prior to Generating Directory Listing

1. Mounted ISO Containing Files as Disk Drive in VMWare Workstation.
2. Ran rootsupd.exe which updated root certificates.
3. Installed .NET 4.6.2 and WMF5.1 to upgrade PowerShell to 5.1.
4. Opened `CMD` or `PowerShell` (Admin) and typed the commands listed below

## Commands Ran

```
D:\
PsExec_IgnoreThisFile_ResearchTool.exe -accepteula -i -s cmd.exe /c powershell.exe
```

The above command will execute PsExec (renamed `PsExec_IgnoreThisFile_ResearchTool.exe`) and escalate privileges from `HOSTNAME\Administrator` to `NT AUTHORITY\SYSTEM`. This allows for more system files to be hashed properly in the next command.

```PowerShell
Get-ChildItem -Recurse 'C:\' | Where-Object { ! $_.PSIsContainer } | Select-Object DirectoryName, Name, FullName, Length, @{ N = 'CreationTimeUtc'; E = { (Get-Date -Format 's' $_.CreationTimeUtc).Replace('T', ' ') } }, @{ N = 'LastAccessTimeUtc'; E = { (Get-Date -Format 's' $_.LastAccessTimeUtc).Replace('T', ' ') } }, @{ N = 'LastWriteTimeUtc'; E = { (Get-Date -Format 's' $_.LastWriteTimeUtc).Replace('T', ' ') } }, Attributes, @{ N = 'MD5'; E = { (Get-FileHash $_.FullName -Algorithm MD5).Hash } }, @{ N = 'SHA1'; E = { (Get-FileHash $_.FullName -Algorithm SHA1).Hash } }, @{ N = 'SHA256'; E = { (Get-FileHash $_.FullName -Algorithm SHA256).Hash } }, @{ N = 'Sddl'; E = { (Get-Acl $_.FullName).Sddl } } | Export-Csv C:\test.csv -NoTypeInformation; systeminfo > C:\SystemInfo_.txt
```

The above command will provide a recursive listing of all files from the C:\ drive including the Parent Path, File Name, Full File Path, Size in Bytes (Length), Creation Time (UTC), Last Access Time (UTC), Last Write Time (UTC), and Attributes associated with the file. Additionally, the MD5, SHA1 and SHA256 hash will be populated for all but very few files in the list. The command will output to `test.csv`. 

Yes, you may see these files as well as other files related to the user profile on the VM that was created with VMWare Workstation Pro. Obviously, these files are not expected on a vanilla Windows install, but they are unavoidable without modifying the output which is disingenuous to what this repo is attempting to accomplish.

## Differences Between Previous Windows OS Version

Differences go here *work in progress*
