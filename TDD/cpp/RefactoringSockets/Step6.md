---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 6"
permalink: /TDD/cpp/RefactoringSockets/Step6.html
---

After inverting all of the ```if```s and nesting all the code deeper, you'll end up with this:

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
        struct addrinfo hints = {0};
        hints.ai_family   = AF_UNSPEC;
        hints.ai_socktype = SOCK_STREAM;
        hints.ai_protocol = IPPROTO_TCP;

        // Resolve the server address and port
        struct addrinfo* result = NULL;
        iResult = getaddrinfo(server, DEFAULT_PORT, &hints, &result);
        if (iResult == 0) {

            // Attempt to connect to an address until one succeeds
            SOCKET ConnectSocket = INVALID_SOCKET;
            for (struct addrinfo* ptr = result; ptr != NULL; ptr = ptr->ai_next) {

                // Create a SOCKET for connecting to server
                ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype, ptr->ai_protocol);
                if (ConnectSocket != INVALID_SOCKET) {
                    // Connect to server.
                    iResult = connect(ConnectSocket, ptr->ai_addr, (int)ptr->ai_addrlen);
                    if (iResult == SOCKET_ERROR) {
                        closesocket(ConnectSocket);
                        ConnectSocket = INVALID_SOCKET;
                    }
                    continue;
                }
                break;
            }

            if (ConnectSocket != INVALID_SOCKET) {
                const char* sendbuf = "this is a longer test";
                // Send an initial buffer
                iResult = send(ConnectSocket, sendbuf, (int)strlen(sendbuf), 0);
                if (iResult != SOCKET_ERROR) {

                    totalBytesSent = iResult;

                    // shutdown the connection since no more data will be sent
                    iResult = shutdown(ConnectSocket, SD_SEND);
                    if (iResult != SOCKET_ERROR) {
                        // Receive until the peer closes the connection
                        do {
                            char recvbuf[DEFAULT_BUFLEN];
                            int recvbuflen = DEFAULT_BUFLEN;
                            iResult = recv(ConnectSocket, recvbuf, recvbuflen, 0);
                        } while (iResult > 0);
                    }
                }
                // cleanup
                closesocket(ConnectSocket);
            }
            freeaddrinfo(result);
        }
        WSACleanup();
    }
    return totalBytesSent;
}
}
```

Now there's actually a bp left on the ```break;``` inside the first ```for``` loop.

The debugger is lying a little, as it's not lining up with the disassembly code.
In any case, the code coverage for this file is now 96.6%.

Are we done? Not quite: we could extract a function that does all the connecting attempts; we could get rid of those macros; we could remove or move those somewhat useless comments. 

Let's do all of that (still making sure that the test passes), and then click [Next](Step7.html).
