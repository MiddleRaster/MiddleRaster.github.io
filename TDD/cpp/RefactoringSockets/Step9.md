---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 9"
permalink: /TDD/cpp/RefactoringSockets/Step9.html
---

To do "[Test Base Class Injection](https://github.com/MiddleRaster/tbci)", we need to wrap up these two functions in a class template, make an empty ```struct``` to use as a base class, add some ```using``` statement, etc., resulting in this:

```cpp
#include <winsock2.h>
#include <WS2tcpip.h>
#include <Wspiapi.h>

// Need to link with Ws2_32.lib, Mswsock.lib, and Advapi32.lib

namespace Legacy
{
    struct Empty
    {
        static int bind(_In_ SOCKET s, _In_reads_bytes_(namelen) const struct sockaddr FAR* name, _In_ int namelen) { return ::bind(s, name, namelen); }
    };

    template <typename Base>
    class SocketsT : private Base
    {
        using Base::bind;

        static SOCKET CreateListener()
        {
            SOCKET ListenSocket = INVALID_SOCKET;

            struct addrinfo hints = { 0 };
            hints.ai_family = AF_INET;
            hints.ai_socktype = SOCK_STREAM;
            hints.ai_protocol = IPPROTO_TCP;
            hints.ai_flags = AI_PASSIVE;
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
    public:
        static int Server(HANDLE hEvent) // returns how many bytes were received or -1 on error
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
                            char recvbuf[DefaultBufLen];
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
    };
    using Sockets = SocketsT<Empty>;
}
```

And the test code needed to be modified, too, to handle a template parameter on the ```ThreadProc```, like this:
```cpp

//#include <Windows.h>
#include <CppUnitTest.h>

#include "client.h"
#include "server.h"

#pragma comment(lib, "Ws2_32.lib")

using namespace Microsoft::VisualStudio::CppUnitTestFramework;

namespace LegacyRefactoringTests
{        
    TEST_CLASS(LegacyTests)
    {
        template <typename Base> static DWORD WINAPI TemplatedThreadProc(LPVOID lpParameter)
        {
            LegacyTests* pThis = reinterpret_cast<LegacyTests*>(lpParameter);
            pThis->totalBytesReceived = Legacy::SocketsT<Base>::Server(pThis->hEvent);
            return 0;
        }

        int totalBytesReceived = 0;
        HANDLE hEvent{ NULL };
    public:
        TEST_METHOD(ClientAndServerCanTalk)
        {
            int totalBytesSent = 0;
            hEvent = ::CreateEvent(NULL, TRUE, FALSE, NULL);

            HANDLE thread = ::CreateThread(NULL, 0, TemplatedThreadProc<Legacy::Empty>, reinterpret_cast<LPVOID>(this), 0, NULL);
            Assert::AreNotEqual(0ULL, (unsigned long long)thread);

            DWORD dw = ::WaitForSingleObject(hEvent, 1000);
            Assert::AreEqual(WAIT_OBJECT_0, dw, L"timed out?");

            totalBytesSent = LegacySockets::Client();
            ::WaitForSingleObject(thread, 1000);
            ::CloseHandle(thread);

            Assert::AreEqual(21, totalBytesSent);
            Assert::AreEqual(21, totalBytesReceived);
        }
        TEST_METHOD(WhenBindFailsNoBytesAreSentOrReceived)
        {
            hEvent = ::CreateEvent(NULL, TRUE, FALSE, NULL);

            struct TestBase
            {
                static int bind(...) { return SOCKET_ERROR; }
            };
            HANDLE thread = ::CreateThread(NULL, 0, TemplatedThreadProc<TestBase>, reinterpret_cast<LPVOID>(this), 0, NULL);
            Assert::AreNotEqual(0ULL, (unsigned long long)thread);

            DWORD dw = ::WaitForSingleObject(hEvent, 1000);
            Assert::AreEqual(static_cast<DWORD>(WAIT_TIMEOUT), dw, L"should time out because we never set the event");

            ::WaitForSingleObject(thread, 1000);
            ::CloseHandle(thread);

            Assert::AreEqual(-1, totalBytesReceived);
        }
    };
}
```

Now the code coverage is at 100%, which is why I think "[Test Base Class Injection](https://github.com/MiddleRaster/tbci)" is so cool, even on legacy code.

Are we done? I think so, though we could go farther and make these functions classes, add a real socket class that closes/cleans up after itself, etc.

Maybe I'll do that in the future, but the current task, to get the code coverage numbers nice and high on relatively bad legacy code, is complete.

[Return to the homepage](/)  
[TDD Tutorials](/TDD/tutorials.html)  
[C++ Dojo](/TDD/cpp/katas.html)  
