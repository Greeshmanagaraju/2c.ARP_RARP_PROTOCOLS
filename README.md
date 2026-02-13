# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
1. start
2. Create a ARP/RARP table containing
3. Read the number of entries (n).
4. Ask user to enter address.
5. Search the table if address is found print 'found', else 'not found'
6. Stop

## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
## PROGRAM - ARP
client.py
```
import socket
c = socket.socket()
c.connect(('localhost', 8000))
      
while True:
    ip = input("Enter IP address to find MAC (or type 'exit' to quit): ")
    if ip.lower() == "exit":  
         break
    c.send(ip.encode())
    mac = c.recv(1024).decode()
    print(f"MAC Address for {ip}: {mac}")
c.close()
```
server.py
```
import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening...")
      
c, addr = s.accept()
print(f"Connection established with {addr}")
      
address = {
    "165.165.80.80": "6A:08:AA:C2",
    "165.165.79.1": "8A:BC:E3:FA"
    }
      
while True:
          ip = c.recv(1024).decode()
      
          if not ip:  
              break
      
          try:
              mac = address[ip]  # Get the MAC address for the IP
              print(f"IP: {ip} -> MAC: {mac}")
              c.send(mac.encode())  
          except KeyError:
              print(f"IP: {ip} not found in ARP table.")
              c.send("Not Found".encode())
c.close()
s.close()
```
## OUPUT - ARP
<img width="1917" height="1018" alt="image" src="https://github.com/user-attachments/assets/c6b61b35-7454-42ee-b933-3162da4a2ee3" />
<img width="1918" height="1021" alt="image" src="https://github.com/user-attachments/assets/723c957d-1fd6-4d9b-a21b-dcdac2dd6af3" />

## PROGRAM - RARP
client.py
```
import socket
c = socket.socket()
c.connect(('localhost', 8000))
  
while True:
    mac = input("Enter MAC address to find IP (or type 'exit' to quit): ")
    if mac.lower() == "exit":  
       break
    c.send(mac.encode())
    ip = c.recv(1024).decode()
    print(f"IP Address for {mac}: {ip}")
c.close()
```
server.py
```
import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening for RARP requests...")
c, addr = s.accept()
print(f"Connection established with {addr}")

rarp_table = {
    "6A:08:AA:C2": "165.165.80.80",
    "8A:BC:E3:FA": "165.165.79.1"
    }
      
while True:
     mac = c.recv(1024).decode()
     
     if not mac:  
        break
     try:
        ip = rarp_table[mac]  
        print(f"MAC: {mac} -> IP: {ip}")
        c.send(ip.encode())  
     except KeyError:
        print(f"MAC: {mac} not found in RARP table.")
        c.send("Not Found".encode())
c.close()
s.close()
```
## OUPUT -RARP
<img width="1918" height="1018" alt="image" src="https://github.com/user-attachments/assets/6f29280d-270f-4542-b221-aebfccdf2d1c" />
<img width="1918" height="1022" alt="image" src="https://github.com/user-attachments/assets/fbd3aeca-ff37-4449-a688-2465f73138a4" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
