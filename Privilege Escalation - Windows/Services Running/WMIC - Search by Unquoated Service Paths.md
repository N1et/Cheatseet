```
wmic service get name,pathname,StartName |  findstr /i /v "C:\Windows\\" | findstr /i /v """
```