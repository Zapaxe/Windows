# Windows

Windows API bindings for [Rux](https://rux-lang.dev). All functions follow the official [Microsoft Win32 documentation](https://learn.microsoft.com/en-us/windows/win32/api/).

## Installation

Add `Windows` to your `Rux.toml`:

```toml
[Dependencies]
Windows = "0.1.0"
```

## Overview

| Module     | DLL            | Category                          |
| ---------- | -------------- | --------------------------------- |
| `Kernel32` | `kernel32.dll` | Memory, Console, Process, Strings |

## API Reference

### Memory Management

Functions for heap-based memory allocation.

| Function                               | Description                                                 |
| -------------------------------------- | ----------------------------------------------------------- |
| `GetProcessHeap()`                     | Returns a handle to the default heap of the calling process |
| `HeapAlloc(heap, flags, bytes)`        | Allocates a block of memory from a specified heap           |
| `HeapReAlloc(heap, flags, mem, bytes)` | Reallocates a block of memory from a heap                   |
| `HeapFree(heap, flags, mem)`           | Frees a memory block allocated from a heap                  |

### Memory Operations

Low-level memory utility routines (RTL).

| Function                                     | Description                                    |
| -------------------------------------------- | ---------------------------------------------- |
| `RtlFillMemory(destination, length, fill)`   | Fills a block of memory with a specified value |
| `RtlZeroMemory(destination, length)`         | Fills a block of memory with zeros             |
| `RtlCopyMemory(destination, source, length)` | Copies a block of memory to another location   |

### Console I/O

Functions for allocating and writing to a console.

| Function                                                              | Description                                                     |
| --------------------------------------------------------------------- | --------------------------------------------------------------- |
| `AllocConsole()`                                                      | Allocates a new console for the calling process                 |
| `GetStdHandle(stdHandle)`                                             | Retrieves a handle to a standard device (stdin, stdout, stderr) |
| `WriteConsoleA(handle, buffer, charsToWrite, charsWritten, reserved)` | Writes a string of ANSI characters to the console               |
| `WriteConsoleW(handle, buffer, charsToWrite, charsWritten, reserved)` | Writes a string of UTF-16 characters to the console             |
| `Beep(freq, duration)`                                                | Generates a tone on the system speaker                          |

### String Conversion

| Function                                                                               | Description                                                 |
| -------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `MultiByteToWideChar(codePage, flags, multiByteStr, multiByte, wideCharStr, wideChar)` | Converts a multi-byte (ANSI) string to a UTF-16 wide string |

### Process & Thread Control

| Function                | Description                                        |
| ----------------------- | -------------------------------------------------- |
| `Sleep(milliseconds)`   | Suspends the execution of the current thread       |
| `ExitProcess(exitCode)` | Terminates the calling process and all its threads |

### Error Handling

| Function         | Description                                         |
| ---------------- | --------------------------------------------------- |
| `GetLastError()` | Retrieves the last-error code of the calling thread |

## Usage Example

```rux
import Windows;

func Main() -> int {
    // Allocate memory from the process heap
    let heap = GetProcessHeap();
    let mem  = HeapAlloc(heap, 0, 1024);
    RtlZeroMemory(mem, 1024);

    // Write to console
    let stdout = GetStdHandle(0xFFFFFFF5);
    let msg    = "Hello, Windows!\n";
    let written: uint32 = 0;
    WriteConsoleA(stdout, msg.data, msg.length as uint32, &written, null);
    HeapFree(heap, 0, mem);
    ExitProcess(0);
}
```

## License

MIT — see [LICENSE](LICENSE).
