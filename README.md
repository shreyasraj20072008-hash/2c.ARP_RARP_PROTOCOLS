# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
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
P
## PROGRAM - ARP
~~~
server.py
import socket

s=socket.socket()
s.bind(("127.0.0.1",8000))  
s.listen(5)
print("Server is listening on port 8000...")
conn,addr=s.accept()
print("Connection from:",addr)  
arp_table = {"10.23.45": "00:11:22:33:44:55", "10.23.46": "66:77:88:99:AA:BB"}
while True:
    data = conn.recv(1024).decode()
    if not data:
        break
    print("Received request for IP:", data)
    
    mac_address = arp_table.get(data, "MAC Address not found")
    conn.send(mac_address.encode())
    conn.close()
s.close()
~~~
~~~
client.py
import socket
s = socket.socket()
s.connect(("127.0.0.1", 8000))

ip_address = input("Enter IP address to resolve: ")
s.send(ip_address.encode())
mac_address = s.recv(1024).decode()
print("MAC Address:", mac_address)  
s.close()
~~~
## OUPUT - ARP
server
<img width="1049" height="193" alt="Screenshot 2026-03-19 131129" src="https://github.com/user-attachments/assets/5acb9fa2-655a-4f4e-a388-2679d112d85f" />
client
<img width="1042" height="160" alt="Screenshot 2026-03-19 131314" src="https://github.com/user-attachments/assets/385a136d-84bd-4bcd-9f40-1ef68c7f4206" />


## PROGRAM - RARP
~~~
server.py
s = socket.socket()
s.bind(('localhost', 8001))
s.listen(5)
print("RARP Server is listening on port 8001...")
c, addr = s.accept()

address = {
    "6A:08:AA:C2": "165.165.80.80",
    "8A:BC:E3:FA": "165.165.79.1"
}
while True:
    mac = c.recv(1024).decode()
    print(f"Received MAC: {mac}")
    ip = address.get(mac, "Not Found")
    c.send(ip.encode())
    exit_command = input("Type 'exit' to stop the server or press Enter to continue: ")
    if exit_command.lower() == 'exit':  
        break
    
c.close()
s.close()
~~~
~~~
client.py
import socket
s = socket.socket()
s.connect(('localhost', 8001))
while True:
    mac = input("Enter Physical Address (MAC): ")
    s.send(mac.encode())
    print("IP Address:", s.recv(1024).decode())
~~~
## OUPUT -RARP
srever
<img width="1256" height="214" alt="image" src="https://github.com/user-attachments/assets/fb7e7c05-bc9f-4c20-a7ae-cdfb8ae3b4c2" />
client
<img width="1211" height="196" alt="image" src="https://github.com/user-attachments/assets/f24dd78f-c3da-4e9a-a3c6-2f1c1e535b79" />


## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
