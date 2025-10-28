# Windows Server 2012 Build 9200 Directory Listing

## Actions Taken Prior to Generating Directory Listing

1. Mounted ISO Containing Files as Disk Drive in VMWare Workstation.
2. Opened `CMD` or `PowerShell` (Admin) and typed the commands listed below

## Commands Ran

```
D:\
PsExec_IgnoreThisFile_ResearchTool.exe -accepteula -i -s cmd.exe /c powershell.exe
```

The above command will execute PsExec (renamed `PsExec_IgnoreThisFile_ResearchTool.exe`) and escalate privileges from `HOSTNAME\Administrator` to `NT AUTHORITY\SYSTEM`. This allows for more system files to be hashed properly in the next command.

```PowerShell
function Get-Hash {
    param (
        [string]$Path,
        [string]$Algorithm = "MD5"
    )
    if (-not (Test-Path $Path)) { return $null }

    try {
        $stream = [System.IO.File]::OpenRead($Path)
        $hasher = [System.Security.Cryptography.HashAlgorithm]::Create($Algorithm)
        $hashBytes = $hasher.ComputeHash($stream)
        $stream.Close()
        # Convert bytes to lowercase hex string
        return -join ($hashBytes | ForEach-Object { $_.ToString("x2") })
    }
    catch {
        return $null
    }
}

Get-ChildItem -Recurse 'C:\' | Where-Object { -not $_.PSIsContainer } | 
    Select-Object `
        DirectoryName,
        Name,
        FullName,
        Length,
        @{ N = 'CreationTimeUtc';   E = { (Get-Date -Format 's' $_.CreationTimeUtc).Replace('T',' ') } },
        @{ N = 'LastAccessTimeUtc'; E = { (Get-Date -Format 's' $_.LastAccessTimeUtc).Replace('T',' ') } },
        @{ N = 'LastWriteTimeUtc';  E = { (Get-Date -Format 's' $_.LastWriteTimeUtc).Replace('T',' ') } },
        Attributes,
        @{ N = 'MD5';    E = { Get-Hash $_.FullName 'MD5' } },
        @{ N = 'SHA1';   E = { Get-Hash $_.FullName 'SHA1' } },
        @{ N = 'SHA256'; E = { Get-Hash $_.FullName 'SHA256' } },
        @{ N = 'Sddl';   E = { (Get-Acl $_.FullName).Sddl } } |
    Export-Csv C:\test.csv -NoTypeInformation

systeminfo > C:\SystemInfo_.txt
```

The above command will provide a recursive listing of all files from the C:\ drive including the Parent Path, File Name, Full File Path, Size in Bytes (Length), Creation Time (UTC), Last Access Time (UTC), Last Write Time (UTC), and Attributes associated with the file. Additionally, the MD5, SHA1 and SHA256 hash will be populated for all but very few files in the list. The command will output to `test.csv`. 

Yes, you may see these files as well as other files related to the user profile on the VM that was created with VMWare Workstation Pro. Obviously, these files are not expected on a vanilla Windows install, but they are unavoidable without modifying the output which is disingenuous to what this repo is attempting to accomplish.

## Differences Between Previous Windows OS Version

Differences go here *work in progress*
