```
 nmap -sU -sV monitored.htb -p161 --script "snmp* and not snmp-brute"
```