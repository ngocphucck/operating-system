#### OSI Architecture

| Layer    | Protocols | Function | Devices |
| -------- | --------- | -------- | ------- |
| Physical layer |Cables, interface cards | Digital-Analog conversion and tranmission| Switch |
| Data link layer | Ethernet (LAN), 802.11 (wifi) | Break data packet to frame and add MAC address | Hub |
| Network layer | IP, ICMP (ssh), ARP |Routing and logical addressing with IP address | Router |
| Transport layer | TCP, IP | Segmentation and Reassembly (correctness), port number is added to session| |
|Session layer | Sockets| | 
|Presentation layer |SSL, FTP | |
|Application layer |HTTP, DNS, SSH | |

#### Example
1. Simple webserver:
```
server_ip = '127.0.0.1'
server_port = 8080


def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((server_ip, server_port))
    server_socket.listen(1)

    print(f"Server listening on {server_ip}:{server_port}")

    while True:
        client_socket, client_address = server_socket.accept()
        print(f"Accept connection from {client_address}")

        request_data = client_socket.recv(1024).decode('utf-8')
        print(f"Receive request:\n{request_data}")

        response = "HTTP/1.0 200 OK\r\nContent-Length: 5\r\n\r\nHello"
        client_socket.send(response.encode('utf-8'))
        print("Sent response: 'Hello'/n")

        client_socket.close()


if __name__ == '__main__':
    start_server()

```
2. Enhance this code with multithreading:
```
import socket
import os


server_ip = '127.0.0.1'
server_port = 8080

def handle_client(client_socket):
    request_data = client_socket.recv(1024).decode('utf-8')
    print(f"Receive request:\n{request_data}")

    response = "HTTP/1.0 200 OK\r\nContent-Length: 5\r\n\r\nHello"
    client_socket.send(response.encode('utf-8'))
    print("Sent response: 'Hello'/n")

    client_socket.close()


def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((server_ip, server_port))
    server_socket.listen(5)

    print(f"Server listening on {server_ip}:{server_port}")

    while True:
        client_socket, client_address = server_socket.accept()
        print(f"Accept connection from {client_address}")

        child_pid = os.fork()

        if child_pid == 0:
            handle_client(client_socket)
            os._exit(0)

        else:
            client_socket.close()


if __name__ == '__main__':
    start_server()

```
3. Measure the speed of webserver: ```$ ab -n 100 -c 10 http://127.0.0.1:8080/```
4. Use mutex lock to protect a shared memory: 
```
import socket
import os
import threading


server_ip = '127.0.0.1'
server_port = 8080

cnt = 0
cnt_lock = threading.Lock()

def handle_client(client_socket):
    global cnt

    with cnt_lock:
        cnt += 1
        request_count = cnt

    request_data = client_socket.recv(1024).decode('utf-8')
    print(f"Receive request:\n{request_data}")

    response = "HTTP/1.0 200 OK\r\nContent-Length: 5\r\n\r\nHello" + str(request_count)
    client_socket.send(response.encode('utf-8'))
    print(f"Sent response: 'Hello{request_count}'/n")

    client_socket.close()


def start_server():
    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind((server_ip, server_port))
    server_socket.listen(5)

    print(f"Server listening on {server_ip}:{server_port}")

    while True:
        client_socket, client_address = server_socket.accept()
        print(f"Accept connection from {client_address}")

        client_thread = threading.Thread(target=handle_client, args=(client_socket, ))
        client_thread.start()


if __name__ == '__main__':
    start_server()

```