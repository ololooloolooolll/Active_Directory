#  Creating a Windows domain in a lab environment

1. Install Windows 2022 
2. Install Windows 11 

3. Create a Domain Controller

Windows Server:

```
PS > Enable-PSRemoting
```

**No output, no response, assume it worked**

4. From the GUI

Add a trusted host and connect to it:

```
PS C:\Users\User> get-item wsman:\localhost\
PS C:\Users\User> Start-Service WinRM
PS C:\Users\User> ls wsman:\localhost\Client\TrustedHosts
PS C:\Users\User> set-item wsman:\localhost\Client\TrustedHosts -value 192.168.111.139
Y
PS C:\Users\User> New-PSSession -ComputerName 192.168.111.139 -Credentials (Get-Credential)
PS C:\Users\User> Enter-PSSession 1
PS C:\Users\Administrator\Documents>
```

5. Installing Active Directory on Windows Server 2022

**sconfig can't be used with PSSession**

Use the `sconfig` tool to change hostname, static ip and dns setings:

```
>sconfig 
>2
>DC-01
>Y
>sconfig
>8
>1
>S
>192.168.111.239
>255.255.255.0
>192.168.111.1
> 2)Set DNS servers
> 192.168.111.239
```

	
6. From the Gui, change the TrustedHosts

```
PS C:\Users\User> set-item wsman:\localhost\Client\TrustedHosts -value 192.168.111.239
PS C:\Users\User> Enter-PSSession 192.168.111.239 (Get-Credential)
PS C:\Users\Administrator\Documents> Install-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

7. Install ADDS

```
PS C:\Users\Administrator\Documents> import-Module ADDSDeployment
PS C:\Users\Administrator\Documents> install-ADDSForest
mydomain.local
SecretPassword
SecretPassword
Y
```

8. At this point, the DNS server was reset to local host

Use sconfig to set it, once again to 192.168.111.239



