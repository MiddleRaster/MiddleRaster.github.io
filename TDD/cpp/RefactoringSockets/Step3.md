---
layout: page_with_comments
title: "Refactoring Socket Sample Code: Step 3"
permalink: /TDD/cpp/RefactoringSockets/Step3.html
---

The task is to get the code coverage number above 90%, but none of the error paths are hit at all.

We could mock heavily, but that's a ton of work, even if using my "[Test Base Class Injection](https://github.com/MiddleRaster/tbci)" technique.

Another idea would be to convert this C code into C++, with a socket class that cleans up after itself. That's a bunch of work, too, but doable.

My idea is to invert the sense of the ```if```s, nesting deeper only on success, something like this (for the first ```if```):

```cpp
// before
int Client(const char* server = "localhost") // returns total bytes sent or -1 on error
{
    // Initialize Winsock
    int iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult != 0) {
        return -1;
    }
    // ...
    WSACleanup();
    return totalBytesSent;
}

// after
int Client(const char* server = "localhost") // returns total bytes sent or -1 on error
{
    totalBytesSent = -1;

    // Initialize Winsock
    int iResult = WSAStartup(MAKEWORD(2,2), &wsaData);
    if (iResult == 0) {
      // nest deeper on success
      // etc.
      WSACleanup();
    }
    return totalBytesSent;
}
```

Go ahead and make that first refactoring and make sure that the test still passes.
Then click [Next](Step4.html).
