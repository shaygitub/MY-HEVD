# Initial analysis of 0x22201F (AllocateFakeObjectNonPagedPoolIoctlHandler):
NOTE: Copied the exact same from  0x22205F
handler gets 92 bytes of data, trigger function organizes it into nonpaged memory object that is formated this way:

![asdasdsadsadsadsadsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/f697c872-4beb-4854-8adc-34bdf932f2f8)
![asdsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/28289d84-91e8-488e-b257-f8a5688df92e)

# Exploit:
i made a specific payload that is supposed to look like this when looking at the memory with windbg
note: it ended up that the two lines interrupting existing data in buffer are just ida's fault, the actual driver
just RtlCopyMemory'ed the entire object into the kernel buffer and null terminated it
![idanig](https://github.com/shaygitub/MY-HEVD/assets/122000611/8b587351-ef08-4e8c-b47a-7b9e85ec0c67)
![asdasdas](https://github.com/shaygitub/MY-HEVD/assets/122000611/95955ee7-cadd-4f5c-98f6-50c05c9daf78)
