---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 8"
permalink: /TDD/cpp/RefactoringSockets/Step8.html
---

After doing all the same refactoring on server.h, we end up with:

```cpp
//#include <Windows.h>
#include <winsock2.h>
#include <WS2tcpip.h>
#include <Wspiapi.h>

// Need to link with Ws2_32.lib, Mswsock.lib, and Advapi32.lib

namespace LegacySockets
{
SOCKET CreateListener()
{
    SOCKET ListenSocket = INVALID_SOCKET;

    struct addrinfo hints   = {0};
    hints.ai_family         = AF_INET;
    hints.ai_socktype       = SOCK_STREAM;
    hints.ai_protocol       = IPPROTO_TCP;
    hints.ai_flags          = AI_PASSIVE;
    struct addrinfo* result = NULL;
    const char* DefaultPort = "27015";
    int iResult = getaddrinfo(NULL, DefaultPort, &hints, &result); // Resolve the server address and port
    if (iResult == 0) {
        ListenSocket = socket(result->ai_family, result->ai_socktype, result->ai_protocol); // Create a SOCKET for connecting to server
        if (ListenSocket != INVALID_SOCKET) {
            // Set up the TCP listening socket
            if ((SOCKET_ERROR == bind(ListenSocket, result->ai_addr, static_cast<int>(result->ai_addrlen))) ||
                (SOCKET_ERROR == listen(ListenSocket, SOMAXCONN))) {
                closesocket(ListenSocket);
                ListenSocket = INVALID_SOCKET;
            }
        }
        freeaddrinfo(result);
    }
    return ListenSocket;
}

int Server(HANDLE hEvent) // returns how many bytes were received or -1 on error
{
    int totalBytesReceived = -1;

    WSADATA wsaData;
    int iResult = WSAStartup(MAKEWORD(2, 2), &wsaData); // Initialize Winsock
    if (iResult == 0) {
        SOCKET ListenSocket = CreateListener();
        if (ListenSocket != INVALID_SOCKET) {

            ::SetEvent(hEvent); // ready to rock-n-roll

            SOCKET ClientSocket = accept(ListenSocket, NULL, NULL); // Accept a client socket
            if (ClientSocket != INVALID_SOCKET) {
                totalBytesReceived = 0;
                do {
                    const int        DefaultBufLen = 512;
                    char recvbuf    [DefaultBufLen];
                    int recvbuflen = DefaultBufLen;
                    iResult = recv(ClientSocket, recvbuf, recvbuflen, 0);
                    if (iResult > 0) {
                        totalBytesReceived += iResult;
                        send(ClientSocket, recvbuf, iResult, 0); // Echo the buffer back to the sender
                    }
                } while (iResult > 0); // Receive until the peer shuts down the connection

                shutdown(ClientSocket, SD_SEND); // shutdown the connection since we're done
                closesocket(ClientSocket);
            }
            closesocket(ListenSocket);
        }
        WSACleanup();
    }
    return totalBytesReceived;
}
}
```

Not quite as clean as the client side, because of the messy error returns from ```bind``` and ```listen```.
But the code coverage is 90.9%, which is just barely enough.

Again, it still looks C-ish, but maybe now would be a good time to mock up ```bind``` or ```listen``` using "[Test Base Class Injection](https://github.com/MiddleRaster/tbci)" so that we can cover those last couple of lines. We'll need a second test for the error path when ```bind``` returns an error.

So refactor using TBCI, write that second test and make sure both tests pass; then click [Next](Step9.html).
