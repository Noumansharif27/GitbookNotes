> For the complete documentation index, see [llms.txt](https://savitar.gitbook.io/mynotes/llms.txt). Markdown versions of documentation pages are available by appending `.md` to page URLs; this page is available as [Markdown](https://savitar.gitbook.io/mynotes/certifications-and-notes/blue-team/cdsa/introduction-to-digital-forensics/memory-forensics/question.md).

# Question

#### Question 1

Examine the file "/home/htb-student/MemoryDumps/Win7-2515534d.vmem" with Volatility. Enter the parent process name for @WanaDecryptor (Pid 1060) as your answer. Answer format: \_.exe

```
vol.py -f /home/htb-student/MemoryDumps/Win7-2515534d.vmem --profile=Win7SP1x64 pslist | grep 1060
```

***

#### Question 2

Examine the file "/home/htb-student/MemoryDumps/Win7-2515534d.vmem" with Volatility. tasksche.exe (Pid 1792) has multiple file handles open. Enter the name of the suspicious-looking file that ends with .WNCRYT as your answer. Answer format: \_.WNCRYT

```
vol.py -f /home/htb-student/MemoryDumps/Win7-2515534d.vmem --profile=Win7SP1x64 handles -p 1792 -t File | grep WNCRYT
```

***

#### Question 3

Examine the file "/home/htb-student/MemoryDumps/Win7-2515534d.vmem" with Volatility. Enter the Pid of the process that loaded zlib1.dll as your answer.

```
volatility -f /home/htb-student/MemoryDumps/Win7-2515534d.vmem --profile=Win7SP1x64 dlllist | grep -B 10 "zlib1.dll"
```
