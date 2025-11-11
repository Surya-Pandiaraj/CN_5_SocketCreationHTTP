### NAME: SURYA P <br>
### REG NO: 212224230280

## EX. No. 5a : SOCKET CREATION FOR HTTP

## AIM :

To write a PYTHON program for socket for HTTP for web page upload and download

## ALGORITHM :

1.Start the program.

2.Get the frame size from the user

3.To create the frame based on the user request.

4.To send frames to server from the client side.

5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.

6.Stop the program

## PROGRAM :

```
import socket

def send_request(host, port, request):
    # Create a socket connection to host:port
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request.encode())  # send HTTP request
        response = b""
        while True:
            part = s.recv(4096)
            if not part:
                break
            response += part
    return response.decode(errors="ignore")  # decode bytes to string

def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()
        content_length = len(file_data)
        # Create a simple HTTP POST request
        request = (
            f"POST / HTTP/1.1\r\n"
            f"Host: {host}\r\n"
            f"Content-Length: {content_length}\r\n"
            f"Connection: close\r\n"
            f"\r\n"
            f"{file_data.decode(errors='ignore')}"
        )
        response = send_request(host, port, request)
    return response

def download_file(host, port, filename):
    # Simple HTTP GET request
    request = f"GET / HTTP/1.1\r\nHost: {host}\r\nConnection: close\r\n\r\n"
    response = send_request(host, port, request)

    # Save the server's response to a file
    with open(filename, 'w', encoding='utf-8') as file:
        file.write(response)
    return response

if __name__ == "__main__":
    host = 'httpbin.org'   # Safe testing site
    port = 80

    # Create a sample file to upload
    with open('example.txt', 'w') as f:
        f.write("Socket upload test data")

    print("Uploading file...\n")
    upload_response = upload_file(host, port, 'example.txt')
    print("Upload Response:\n", upload_response)

    print("\nDownloading page...\n")
    download_response = download_file(host, port, 'downloaded_page.html')
    print("Download Response:\n", download_response[:300], "...\n")
    print("File 'downloaded_page.html' saved successfully.")

```

## OUTPUT: 



## RESULT :

Thus the socket for HTTP for web page upload and download created and Executed
