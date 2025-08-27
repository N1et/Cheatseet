```
# pip install pyftpdlib
from pyftpdlib.authorizers import DummyAuthorizer
from pyftpdlib.handlers import FTPHandler
from pyftpdlib.servers import FTPServer

authorizer = DummyAuthorizer()
authorizer.add_anonymous(homedir=".", perm='welr')  

handler = FTPHandler
handler.authorizer = authorizer

address = ("0.0.0.0", 21)
server = FTPServer(address, handler)

print("[*] FTP server running on port 21 (anonymous access enabled)")
server.serve_forever()

```