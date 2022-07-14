Copying a file from a workstation to PS managed server
```
PS C:\Users\User> $dc = New-PSSession 192.168.111.239 -Credential (Get-Credential)
PS C:\Users\User> echo $dc
PS C:\Users\User> Copy-Item .\somefile.json -ToSession $dc C:\Windows\
PS C:\Users\User> Enter-PSSession $dc
PS C:\Users\Administrator> cd C:\Windows
PS C:\Users\Administrator> cd C:\Windows\
PS C:\Users\Administrator> dir
somefile.json
```
