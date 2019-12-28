## SSH to a remote server using a login node

To transport files to the local area net of school, we needs a server in the school that has already obtained a open IP.

For convinience, we named the computer A(a), B(b), C(c) for client computer, transit computer and the server.

The following commands are using to transfer files from A to C using B.

``` javascript
ssh -g -L 5050:c:22 FYK@b

scp -P 5050 ${FILE_NAME} username@localhost:/${HOME_ROOT}
```