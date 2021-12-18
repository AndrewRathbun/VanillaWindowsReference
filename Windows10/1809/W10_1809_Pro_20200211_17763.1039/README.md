# Windows 10 (1809) Build 17763.1039 Directory Listing

Release date: 20200211

Official documentation: https://support.microsoft.com/en-us/topic/february-11-2020-kb4532691-os-build-17763-1039-bcb1a6dc-c423-4638-37b9-e614272e48a5

## Actions Taken Prior to Generating Directory Listing

1. Copied `PsExec_IgnoreThisFile_ResearchTool.exe` to the root of `C:\`
2. Opened `CMD (Admin)` and typed the command listed below

## Command Ran

```cmd
C:\PsExec_IgnoreThisFile_ResearchTool.exe -accepteula -i -s cmd.exe /c powershell.exe "Get-ChildItem -Recurse 'C:\' | Where-Object { ! $_.PSIsContainer } | Select-Object DirectoryName,Name,FullName,Length,@{N='CreationTimeUtc';E={(Get-Date -Format 's' $_.CreationTimeUtc).Replace('T', ' ')}},@{N='LastAccessTimeUtc';E={(Get-Date -Format 's' $_.LastAccessTimeUtc).Replace('T', ' ')}},@{N='LastWriteTimeUtc';E={(Get-Date -Format 's' $_.LastWriteTimeUtc).Replace('T', ' ')}},Attributes,@{N='MD5';E={(Get-FileHash $_.FullName -Algorithm MD5).Hash}},@{N='SHA256';E={(Get-FileHash $_.FullName -Algorithm SHA256).Hash}},@{N='Sddl';E={(Get-Acl $_.FullName).Sddl}} | Export-Csv C:\test.csv -NoTypeInformation; systeminfo > C:\SystemInfo_.txt"
```

The above command will execute PsExec (renamed `PsExec_IgnoreThisFile_ResearchTool.exe`) and escalate privileges from `HOSTNAME\user` to `NT AUTHORITY\SYSTEM`. The above command will also provide a recursive listing of all files from the `C:\` drive including the Parent Path, File Name, Full File Path, Size in Bytes (Length), Creation Time (UTC), Last Access Time (UTC), Last Write Time (UTC), and Attributes associated with the file. Additionally, the MD5 and SHA256 hash will be populated for all but very few files in the list. The command will output to `C:\test.csv`. Lastly, the command automates the `SystemInfo` command and outputs to `C:\SystemInfo_.txt`. 

Yes, you may see these files as well as other files related to the user profile (`CFUser`) on the VM that was created using Easy Install with VMWare Workstation Pro. Obviously, these files are not expected on a vanilla Windows install, but they are unavoidable without modifying the output which is disingenuous to what this repo is attempting to accomplish. 

## Using Timeline Explorer to Examine CSV Output

[Timeline Explorer](https://ericzimmerman.github.io/#!index.md) has been optimized with a plugin specifically designed to handle output from this repo. That plugin can be found [here](https://github.com/EricZimmerman/TLEFilePlugins/blob/08ab79bb55b4338d76abe9942f52da47261e736c/TLEFileMisc/Misc.cs#L1001).

To further optimize viewing the CSVs in this repo, download this [Timeline Explorer layout file](https://github.com/AndrewRathbun/VanillaWindowsReference/blob/main/SupportingFiles/TLEFileMisc.VanillaWindowsReference.layout.local) and place it in the following location: `.\TimelineExplorer\Layouts`.

Once the layout file is in place, open up any CSV and you'll find that known files introduced during the creation of this repo are crossed out so it's obvious that they are not a file that ships with Windows. See below for a preview of this. 

![test](https://raw.githubusercontent.com/AndrewRathbun/VanillaWindowsReference/main/SupportingFiles/TLEPluginLayoutPreview.gif)

To be very clear, you can still look at this CSV using any other application you wish. Timeline Explorer simply has extra features and capabilities that I've leveraged to my own liking. 

## Differences Between Previous Windows OS Version

Differences go here *work in progress*
