---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 1"
permalink: /TDD/cpp/RefactoringSockets/Step1.html
---

Once upon a time at *StupdendousCorp*, I was teaching a week-long class on Scrum and TDD, and the project for the class had to do with networking/sockets.
Most of the students were not experts at writing socket code, and since I consider the socket APIs to be some of the most poorly designed interfaces on the planet (it's very old, from the 1983's Berkeley Unix, after all), I searched MSDN for sample code.

This is what I found:

**client side**:
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
    WSADATA wsaData;
    SOCKET ConnectSocket = INVALID_SOCKET;
    struct addrinfo *result = NULL,
                    *ptr = NULL,
                    hints;
    const char *sendbuf = "this is a longer test";
    char recvbuf[DEFAULT_BUFLEN];
    int iResult;
    int recvbuflen = DEFAULT_BUFLEN;

    // Initialize Winsock
    iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult != 0) {
        printf("WSAStartup failed: %d\n", iResult);
        return -1;
    }

    ZeroMemory( &hints, sizeof(hints) );
    hints.ai_family = AF_UNSPEC;
    hints.ai_socktype = SOCK_STREAM;
    hints.ai_protocol = IPPROTO_TCP;

    // Resolve the server address and port
    iResult = getaddrinfo(server, DEFAULT_PORT, &hints, &result);
    if ( iResult != 0 ) {
        printf("getaddrinfo failed: %d\n", iResult);
        WSACleanup();
        return -1;
    }

    // Attempt to connect to an address until one succeeds
    for(ptr=result; ptr != NULL ;ptr=ptr->ai_next) {

        // Create a SOCKET for connecting to server
        ConnectSocket = socket(ptr->ai_family, ptr->ai_socktype, 
            ptr->ai_protocol);
        if (ConnectSocket == INVALID_SOCKET) {
            printf("Error at socket(): %ld\n", WSAGetLastError());
            freeaddrinfo(result);
            WSACleanup();
            return -1;
        }

        // Connect to server.
        iResult = connect( ConnectSocket, ptr->ai_addr, (int)ptr->ai_addrlen);
        if (iResult == SOCKET_ERROR) {
            closesocket(ConnectSocket);
            ConnectSocket = INVALID_SOCKET;
            continue;
        }
        break;
    }

    freeaddrinfo(result);

    if (ConnectSocket == INVALID_SOCKET) {
        printf("Unable to connect to server!\n");
        WSACleanup();
        return -1;
    }

    // Send an initial buffer
    iResult = send( ConnectSocket, sendbuf, (int)strlen(sendbuf), 0 );
    if (iResult == SOCKET_ERROR) {
        printf("send failed: %d\n", WSAGetLastError());
        closesocket(ConnectSocket);
        WSACleanup();
        return -1;
    }
    int totalBytesSent = iResult; 

    printf("Bytes Sent: %ld\n", iResult);

    // shutdown the connection since no more data will be sent
    iResult = shutdown(ConnectSocket, SD_SEND);
    if (iResult == SOCKET_ERROR) {
        printf("shutdown failed: %d\n", WSAGetLastError());
        closesocket(ConnectSocket);
        WSACleanup();
        return -1;
    }

    // Receive until the peer closes the connection
    do {

        iResult = recv(ConnectSocket, recvbuf, recvbuflen, 0);
        if ( iResult > 0 )
            printf("Bytes received: %d\n", iResult);
        else if ( iResult == 0 )
            printf("Connection closed\n");
        else
            printf("recv failed: %d\n", WSAGetLastError());

    } while( iResult > 0 );

    // cleanup
    closesocket(ConnectSocket);
    WSACleanup();

    return totalBytesSent;
}
}
```

And here's the **server side**:
```cpp
#include <winsock2.h>
#include <WS2tcpip.h>
#include <Wspiapi.h>

// Need to link with Ws2_32.lib, Mswsock.lib, and Advapi32.lib
#define DEFAULT_BUFLEN 512
#define DEFAULT_PORT "27015"

namespace LegacySockets
{
int Server(HANDLE hEvent) // returns how many bytes were received or -1 on error
{
    WSADATA wsaData;
    SOCKET ListenSocket = INVALID_SOCKET,
           ClientSocket = INVALID_SOCKET;
    struct addrinfo *result = NULL,
                    hints;
    char recvbuf[DEFAULT_BUFLEN];
    int iResult, iSendResult;
    int recvbuflen = DEFAULT_BUFLEN;

    // Initialize Winsock
    iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult != 0) {
        printf("WSAStartup failed: %d\n", iResult);
        return -1;
    }

    ZeroMemory(&hints, sizeof(hints));
    hints.ai_family = AF_INET;
    hints.ai_socktype = SOCK_STREAM;
    hints.ai_protocol = IPPROTO_TCP;
    hints.ai_flags = AI_PASSIVE;

    // Resolve the server address and port
    iResult = getaddrinfo(NULL, DEFAULT_PORT, &hints, &result);
    if ( iResult != 0 ) {
        printf("getaddrinfo failed: %d\n", iResult);
        WSACleanup();
        return -1;
    }

    // Create a SOCKET for connecting to server
    ListenSocket = socket(result->ai_family, result->ai_socktype, result->ai_protocol);
    if (ListenSocket == INVALID_SOCKET) {
        printf("socket failed: %ld\n", WSAGetLastError());
        freeaddrinfo(result);
        WSACleanup();
        return -1;
    }

    // Setup the TCP listening socket
    iResult = bind( ListenSocket, result->ai_addr, (int)result->ai_addrlen);
    if (iResult == SOCKET_ERROR) {
        printf("bind failed: %d\n", WSAGetLastError());
        freeaddrinfo(result);
        closesocket(ListenSocket);
        WSACleanup();
        return -1;
    }
    freeaddrinfo(result);

    iResult = listen(ListenSocket, SOMAXCONN);
    if (iResult == SOCKET_ERROR) {
        printf("listen failed: %d\n", WSAGetLastError());
        closesocket(ListenSocket);
        WSACleanup();
        return -1;
    }

	::SetEvent(hEvent); // ready to rock-n-roll

    // Accept a client socket
    ClientSocket = accept(ListenSocket, NULL, NULL);
    if (ClientSocket == INVALID_SOCKET) {
        printf("accept failed: %d\n", WSAGetLastError());
        closesocket(ListenSocket);
        WSACleanup();
        return -1;
    }

    // No longer need server socket
    closesocket(ListenSocket);

    // Receive until the peer shuts down the connection
	int totalBytesReceived = 0;
    do {

        iResult = recv(ClientSocket, recvbuf, recvbuflen, 0);
        if (iResult > 0) {
            printf("Bytes received: %d\n", iResult);
            totalBytesReceived += iResult;

            // Echo the buffer back to the sender
            iSendResult = send( ClientSocket, recvbuf, iResult, 0 );
            if (iSendResult == SOCKET_ERROR) {
                printf("send failed: %d\n", WSAGetLastError());
                closesocket(ClientSocket);
                WSACleanup();
                return -1;
            }
            printf("Bytes sent: %d\n", iSendResult);
        }
        else if (iResult == 0)
            printf("Connection closing...\n");
        else  {
            printf("recv failed: %d\n", WSAGetLastError());
            closesocket(ClientSocket);
            WSACleanup();
            return -1;
        }

    } while (iResult > 0);

    // shutdown the connection since we're done
    iResult = shutdown(ClientSocket, SD_SEND);
    if (iResult == SOCKET_ERROR) {
        printf("shutdown failed: %d\n", WSAGetLastError());
        closesocket(ClientSocket);
        WSACleanup();
        return -1;
    }

    // cleanup
    closesocket(ClientSocket);
    WSACleanup();

    return totalBytesReceived;
}
}
```

I consider this truly awful code:  it looks like C code, written by a junior dev; it's got that repeated clean-up code, which is so error-prone; printfs, not a hint of RAII, etc.

My idea was that they could morph the sample code into something workable for the project at hand.
But that didn't happen:  instead, they read through the sample code, wrote their own, and when it didn't work, they came to me, looking for help (which I was unable to provide, not being an expert in socket code either).

So, the next time I taught the class, I made it an exercise:  the first task is to write a test that uses these two samples to send a little bit of data from the client to the server (which the server would echo back).

Turns out, they couldn't do it, not in the allotted time (1 hour).

So, the next, next time, I wrote the test for them and asked them to refactor the code while keeping the test passing.
This they could do.

So, your first mission, should you choose to accept it, is to write that test.

Then click [Next](Step2.html).
