---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 4"
permalink: /TDD/cpp/RefactoringSockets/Step4.html
---

After the first round of refactoring, inverting the send of the ```if``` and putting all the remaining code inside the ```if```, you'll end up with this (after a little clean-up):

```cpp
#include <WinSock2.h>
#include <WS2tcpip.h>
#include <Wspiapi.h>

// Need to link with Ws2_32.lib, Mswsock.lib, and Advapi32.lib
#define DEFAULT_BUFLEN 512
#define DEFAULT_PORT "27015"

namespace LegacySockets
{
int Client(const char* server = "localhost") // returns total bytes sent or -1 on error
{
    int totalBytesSent = -1;

    // Initialize Winsock
    WSADATA wsaData;
    int iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult == 0) {
        SOCKET ConnectSocket = INVALID_SOCKET;
        struct addrinfo* result = NULL, * ptr = NULL;
        const char* sendbuf = "this is a longer test";
        char recvbuf[DEFAULT_BUFLEN];
        int recvbuflen = DEFAULT_BUFLEN;

        struct addrinfo hints = {0};
        hints.ai_family   = AF_UNSPEC;
        hints.ai_socktype = SOCK_STREAM;
        hints.ai_protocol = IPPROTO_TCP;

        // Resolve the server address and port
        iResult = getaddrinfo(server, DEFAULT_PORT, &hints, &result);
        if (iResult != 0) {
            WSACleanup();
            return -1;
        }

        // Attempt to connect to an address until one succeeds
        for (ptr = result; ptr != NULL; ptr = ptr->ai_next) {

            // Create a SOCKET for connecting to server
            ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype,
                ptr->ai_protocol);
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
        WSACleanup();
    }
    return totalBytesSent;
}
}
```

I got rid of all the useless ```printf```s, moved the variable declarations nearer to where they're used, etc.

That's better already:  we went from 14 uncovered lines to 12.

Let's do another round of the same refactoring, just to make sure we've got the hang of it.
Make sure the test still passes and then click [Next](Step5.html).
