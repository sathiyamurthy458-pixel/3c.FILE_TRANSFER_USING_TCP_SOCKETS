# 3c.CREATION FOR FILE TRANSFER USING TCP SOCKETS
## AIM
To write a python program for creating File Transfer using TCP Sockets Links
## ALGORITHM:
1. Import the necessary python modules.
2. Create a socket connection using socket module.
3. Send the message to write into the file to the client file.
4. Open the file and then send it to the client in byte format.
5. In the client side receive the file from server and then write the content into it.
## PROGRAM
CLIENT
```
import socket
import os
s = socket.socket()
s.connect(('localhost', 5001))
filename = input("Enter file name to send: ")
if not os.path.isfile(filename):
    print("File not found!")
    s.close()
    exit()
filesize = os.path.getsize(filename)
name = os.path.basename(filename).encode()
s.sendall(len(name).to_bytes(2, 'big'))
s.sendall(name)
s.sendall(filesize.to_bytes(8, 'big'))
print("Sending", filename, "(", filesize, "bytes)")
sent = 0
with open(filename, "rb") as f:
    while True:
        data = f.read(1024)
        if not data:
            break
        s.sendall(data)
        sent += len(data)
        progress = (sent / filesize) * 100 if filesize else 100
        print("Progress: {:.2f}%".format(progress))
print("File sent successfully!")
s.close()


```
SERVER
```
import socket
import os
s = socket.socket()
s.bind(('localhost', 5001))
s.listen(1)
print("Server listening on port 5001")
c, addr = s.accept()
print("Connected from:", addr)
name_size = int.from_bytes(c.recv(2), 'big')
filename = c.recv(name_size).decode()
print("Receiving file:", filename)
size = int.from_bytes(c.recv(8), 'big')
print("File size:", size, "bytes")
received = 0
with open("received_" + os.path.basename(filename), "wb") as f:
    while received < size:
        data = c.recv(min(1024, size - received))
        if not data:
            break
        f.write(data)
        received += len(data)
        progress = (received / size) * 100
        print("Progress: {:.2f}%".format(progress))
if received == size:
    print("File received successfully!")
else:
    print("File transfer incomplete!")
c.close()
s.close()


```
## OUTPUT
CLIENT

<img width="1752" height="998" alt="image" src="https://github.com/user-attachments/assets/1ef03c7d-a9a6-48ea-a6cb-11a9c452db7b" />

SERVER

<img width="1842" height="993" alt="image" src="https://github.com/user-attachments/assets/95ed65ab-cb31-479f-8407-624e84c13e86" />


## RESULT
Thus, the python program for creating File Transfer using TCP Sockets Links was 
successfully created and executed.
