---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 5"
permalink: /TDD/cpp/RefactoringSockets/Step5.html
---

After the second round of refactoring, inverting the send of the ```if``` and putting all the remaining code inside the ```if```, you'll end up with this (after moving the variable declarations down):

```cpp
int Client(const char* server = "localhost") // returns total bytes sent or -1 on error
{
    int totalBytesSent = -1;

    // Initialize Winsock
    WSADATA wsaData;
    int iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult == 0) {
        struct addrinfo hints = {0};
        hints.ai_family   = AF_UNSPEC;
        hints.ai_socktype = SOCK_STREAM;
        hints.ai_protocol = IPPROTO_TCP;

        // Resolve the server address and port
        struct addrinfo* result = NULL;
        iResult = getaddrinfo(server, DEFAULT_PORT, &hints, &result);
        if (iResult == 0) {
            SOCKET ConnectSocket = INVALID_SOCKET;
            struct addrinfo* ptr = NULL;
            const char* sendbuf  = "this is a longer test";
            char recvbuf[DEFAULT_BUFLEN];
            int recvbuflen = DEFAULT_BUFLEN;

            // Attempt to connect to an address until one succeeds
            for (ptr = result; ptr != NULL; ptr = ptr->ai_next) {

                // Create a SOCKET for connecting to server
                ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype, ptr->ai_protocol);
                if (ConnectSocket == INVALID_SOCKET) {
                    freeaddrinfo(result);
                    WSACleanup();
                    return -1;
                }

                // Connect to server.
                iResult = connect(ConnectSocket, ptr->ai_addr, (int)ptr->ai_addrlen);
                if (iResult == SOCKET_ERROR) {
                    closesocket(ConnectSocket);
                    ConnectSocket = INVALID_SOCKET;
                    continue;
                }
                break;
            }

            freeaddrinfo(result);

            if (ConnectSocket == INVALID_SOCKET) {
                WSACleanup();
                return -1;
            }

            // Send an initial buffer
            iResult = send(ConnectSocket, sendbuf, (int)strlen(sendbuf), 0);
            if (iResult == SOCKET_ERROR) {
                closesocket(ConnectSocket);
                WSACleanup();
                return -1;
            }
            totalBytesSent = iResult;

            // shutdown the connection since no more data will be sent
            iResult = shutdown(ConnectSocket, SD_SEND);
            if (iResult == SOCKET_ERROR) {
                closesocket(ConnectSocket);
                WSACleanup();
                return -1;
            }

            // Receive until the peer closes the connection
            do {
                iResult = recv(ConnectSocket, recvbuf, recvbuflen, 0);
                if (iResult > 0)
                    ;
                else if (iResult == 0)
                    ;
                else
                    ;
            } while (iResult > 0);

            // cleanup
            closesocket(ConnectSocket);
        }
        WSACleanup();
    }
    return totalBytesSent;
}
}
```

Getting there:  we're down to 11 uncovered lines.

Continue this way, until all the code is covered. 
Make sure the test still passes and then click [Next](Step6.html).
