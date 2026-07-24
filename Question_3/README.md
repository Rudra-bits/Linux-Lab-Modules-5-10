1. Program Logic & Architecture
To meet the requirements using raw Linux system calls (low-level I/O), the utility treats employee records as fixed-size structures stored sequentially in a binary file.

Creating a File: Use the open() system call with flags O_CREAT | O_WRONLY | O_TRUNC and appropriate file permissions (e.g., 0644) to initialize a new data file.

Writing Employee Records: Calculate the exact byte offset for new entries or append them using write() sequentially.

Updating Specific Records (Without Rewriting the Whole File):

Each record has a known, fixed size (S).

To update record number n, compute its byte offset: Offset=n×S.

Use lseek() to jump directly to that position, and overwrite the specific record in-place using write().

Retrieving Records Efficiently:

Compute the target record's byte offset similarly.

Use lseek() to reposition the file pointer instantly and read the exact block using read() without scanning the preceding records.

2. Role of Linux System Calls
open():

Role: Establishes a connection between the file on disk and the program, returning a unique file descriptor.

Contribution: It configures access modes (like O_RDWR for reading and updating) and creation flags without using buffered standard I/O streams (FILE *).

write():

Role: Transfers raw bytes from a buffer in memory to the file descriptor.

Contribution: Used to write new employee records or overwrite specific bytes during an in-place update.

read():

Role: Pulls raw bytes from the file descriptor into a memory buffer.

Contribution: Fetches individual employee records from random access locations instantly.

lseek():

Role: Repositions the file offset pointer associated with the file descriptor.

Contribution: The core mechanism enabling efficient random access. Instead of reading sequentially from the beginning, lseek() moves the pointer directly to the target record's byte offset for targeted reads or updates.

close():

Role: Terminates the file descriptor link.

Contribution: Flushes kernel buffers to disk, releases system resources, and ensures file integrity once processing is complete.
