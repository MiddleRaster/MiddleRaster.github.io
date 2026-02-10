---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 2"
permalink: /TDD/cpp/RefactoringSockets/Step2.html
---

Here's what I came up with:

```cpp
#include <CppUnitTest.h>

#include "client.h"
#include "server.h"

#pragma comment(lib, "Ws2_32.lib")

using namespace Microsoft::VisualStudio::CppUnitTestFramework;

namespace LegacyRefactoringTests
{        
    TEST_CLASS(LegacyTests)
    {
        int totalBytesReceived{0};
        HANDLE hEvent{NULL};
    public:
        TEST_METHOD(ClientAndServerCanTalk)
        {
            hEvent = ::CreateEvent(NULL, TRUE, FALSE, NULL);

            HANDLE thread = ::CreateThread(NULL, 0, ThreadProc, reinterpret_cast<LPVOID>(this), 0, NULL);
            Assert::AreNotEqual(0ULL, (unsigned long long)thread);

            DWORD dw = ::WaitForSingleObject(hEvent, 1000);
            Assert::AreEqual(WAIT_OBJECT_0, dw, L"timed out?");

            int totalBytesSent = LegacySockets::Client();
            ::WaitForSingleObject(thread, 1000);
            ::CloseHandle(thread);

            Assert::AreEqual(21, totalBytesSent);
            Assert::AreEqual(21, totalBytesReceived);
        }
        static DWORD WINAPI ThreadProc(LPVOID lpParameter)
        {
            LegacyTests* pThis = reinterpret_cast<LegacyTests*>(lpParameter);
            pThis->totalBytesReceived = LegacySockets::Server(pThis->hEvent);
            return 0;
        }
    };
}
```

So, we create a thread for the server to run in, and an event for the server-side to trigger when it's ready to listen and is accepting connections.
If your code doesn't look exactly like this, that's ok, so long as it works in roughly the same way.
Once you've got it building, run the test to make sure it works.

Ok, now what? What's the real task?

The task I set my students was to get code coverage above 90%.

If you don't have a code coverage tool, you can do what I call "poor man's code coverage":  set a breakpoint on every block of code, and when running under a debugger, turn each bp off when you hit it.
Then count up the unhit breakpoints and divide by the lines of code.

So, once that's done, you'll see that the "happy path" is nicely covered, but none of the error paths are.

So, what now? We need to strategize a bit.  When you have a plan, click [Next](Step3.html).
