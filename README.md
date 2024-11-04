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
```py
#SERVER
import socket
import os

HOST = '127.0.0.1'  
PORT = 65432  

def send_file(filename, conn):
   if os.path.isfile(filename):
       conn.sendall(b'EXISTS')
       file_size = os.path.getsize(filename)
       conn.sendall(str(file_size).encode('utf-8'))
       client_response = conn.recv(1024).decode('utf-8')
       if client_response == 'READY':
           with open(filename, 'rb') as f:
               chunk = f.read(1024)
               while chunk:
                   conn.sendall(chunk)
                   chunk = f.read(1024)
           print(f"File '{filename}' sent successfully.")
   else:
       conn.sendall(b'NOT_EXISTS')

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as server_socket:
   server_socket.bind((HOST, PORT))
   server_socket.listen()

   print(f"File server is listening on {HOST}:{PORT}")
   while True:
       conn, addr = server_socket.accept()
       with conn:
           print(f"Connected by {addr}")

           filename = conn.recv(1024).decode('utf-8')
           print(f"Client requested file: {filename}")

           send_file(filename, conn)
```
```py
#CLIENT
import socket

HOST = '127.0.0.1'  
PORT = 65432  


def receive_file(filename, conn):
   response = conn.recv(1024).decode('utf-8')

   if response == 'EXISTS':
       file_size = int(conn.recv(1024).decode('utf-8'))
       print(f"File '{filename}' exists on server, size: {file_size} bytes.")

       conn.sendall(b'READY')

       with open('received_' + filename, 'wb') as f:
           total_received = 0
           while total_received < file_size:
               data = conn.recv(1024)
               f.write(data)
               total_received += len(data)
               print(f"Received {total_received} of {file_size} bytes")

       print(f"File '{filename}' received and saved as 'received_{filename}'")
   else:
       print("File does not exist on the server.")

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as client_socket:
   client_socket.connect((HOST, PORT))

   filename = input("Enter the filename you want to download: ")
   client_socket.sendall(filename.encode('utf-8'))

   receive_file(filename, client_socket)
```
## OUPUT
### SERVER
<img width="306" alt="Screen Shot 1946-07-22 at 11 27 22" src="https://github.com/user-attachments/assets/052ed9b5-07c8-41f4-8d1d-8fc9b621b05f">

### CLIENT
<img width="274" alt="Screen Shot 1946-07-22 at 11 27 11" src="https://github.com/user-attachments/assets/ede1e470-7988-4c9a-a64b-18e8ab6fd0ca">

## RESULT
Thus, the python program for creating File Transfer using TCP Sockets Links was 
successfully created and executed.
