---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 7"
permalink: /TDD/cpp/RefactoringSockets/Step7.html
---

After that final round of clean-up, we end up with:

```cpp
#include <WinSock2.h>
#include <WS2tcpip.h>
#include <Wspiapi.h>

// Need to link with Ws2_32.lib, Mswsock.lib, and Advapi32.lib

namespace LegacySockets
{
SOCKET Connect(const char* server)
{
    SOCKET ConnectSocket = INVALID_SOCKET;

    struct addrinfo hints   = {0};
    hints.ai_family         = AF_UNSPEC;
    hints.ai_socktype       = SOCK_STREAM;
    hints.ai_protocol       = IPPROTO_TCP;
    struct addrinfo* result = NULL;
    const char* DefaultPort = "27015";
    int iResult = getaddrinfo(server, DefaultPort, &hints, &result); // Resolve the server address and port
    if (iResult == 0) {
        for (struct addrinfo* ptr = result; ptr != NULL; ptr = ptr->ai_next) {          // Attempt to connect to an address until one succeeds
            ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype, ptr->ai_protocol); // Create a SOCKET for connecting to server
            if (ConnectSocket != INVALID_SOCKET) {
                iResult = connect(ConnectSocket, ptr->ai_addr, (int)ptr->ai_addrlen);   // Connect to server.
                if (iResult == SOCKET_ERROR) {
                    closesocket(ConnectSocket);
                    ConnectSocket = INVALID_SOCKET;
                }
                continue;
            }
            break;
        }
        freeaddrinfo(result);
    }
    return ConnectSocket;
}

int Client(const char* server = "localhost") // returns total bytes sent or -1 on error
{
    int totalBytesSent = -1;

    WSADATA wsaData;
    int iResult = WSAStartup(MAKEWORD(2,2), &wsaData); // Initialize Winsock
    if (iResult == 0) {
        SOCKET ConnectSocket = Connect(server);
        if (ConnectSocket != INVALID_SOCKET) {
            const char* sendbuf = "this is a longer test";
            iResult = send(ConnectSocket, sendbuf, static_cast<int>(strlen(sendbuf)), 0); // Send an initial buffer
            if (iResult != SOCKET_ERROR) {
                totalBytesSent = iResult; // the return value
                iResult = shutdown(ConnectSocket, SD_SEND); // shutdown the connection since no more data will be sent
                if (iResult != SOCKET_ERROR) {
                    do {
                        const int        DefaultBufLen = 512;
                        char recvbuf    [DefaultBufLen];
                        int recvbuflen = DefaultBufLen;
                        iResult = recv(ConnectSocket, recvbuf, recvbuflen, 0); // Receive until the peer closes the connection
                    } while (iResult > 0);
                }
            }
            closesocket(ConnectSocket);
        }
        WSACleanup();
    }
    return totalBytesSent;
}
}
```

Ok, it still looks kind of C-ish, but at least the code coverage is much better.
We can refactor it more later.

Next, let's do the same thing to the server side, still making sure that the test passes, and then click [Next](Step8.html).
