# Windows 10 (1511) `dir` Listing

## Actions Taken Prior to Generating `dir` listing

1. Changed timezone to UTC and adjusted date/time formats to my liking
2. Opened `CMD` (Admin) and typed the commands listed below

## Commands Ran

```
cd c:\ && dir /b /s /a /r > W10_1511_dir_b.txt
```

The above command will provide bare output showing just full file paths of all files that come with this version of Windows.

```
cd c:\ && dir /s /a /r /t:c > W10_1511_dircreated.txt
```

The above command will show more verbose output that includes the file creation timestamp, file size (bytes), and groups files and subfolders within a given folder together and summarizes their file sizes at the bottom of each group.

Yes, you may see these files as well as other files related to the user profile on the VM that was created using Easy Install with VMWare Workstation Pro. Obviously, these files are not expected on a vanilla Windows install, but they are unavoidable without modifying the `dir` output which is disingenuous to what this repo is attempting to accomplish.

## Differences Between Previous Windows OS Version

Differences go here *work in progress*
