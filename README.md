# Plaintext Passwords in Memory via PowerShell Direct

During testing of Windows Hyper-V functionality, it was discovered that PowerShell Direct stores passwords in plaintext in process memory during use. The following method was used to identify the issue:

1. Start a PowerShell Direct session with the command:

` Enter-PSSession -VMName "Name of VM"`

2. Enter username and password in the authentication prompt

3. Dump the process memory of PowerShell using Procdump:

 ` procdump -ma <PID>`

 3. Use Sysinternals strings to analyze the memory dump:

 ` strings dumpfile.dmp > procdump.txt`

 4. Search for **Client Connected**, in the dumped file:

` Select-String -Path .\procdump.txt -Pattern "Client Connected" -Context 0, 5`

This revealed that passwords from the authentication prompt were stored in plaintext in process memory. While Microsoft considers this "expected behavior," it could pose a security risk if exploited by users with the Hyper-V Administrator role.
