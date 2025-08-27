```
net localgroup "Remote Desktop Users"
netsh advfirewall firewall add rule name="Allow RDP" protocol=TCP dir=in localport=3389 action=allow
net start termservice
```