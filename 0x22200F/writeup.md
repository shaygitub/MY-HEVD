# Initial analysis of 0x22200F (BufferOverflowNonPagedPoolIoctlHandler, trigger TriggerBufferOverflowNonPagedPool):
handler receives a "504" sized buffer and the "size" of the buffer that "should be" 504, trigger copies it into nonpaged memory by the size provided, not the actual nonpaged size
![asdasdsadsadsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/2c687479-7e20-4bd8-86a2-fb706a4718e9)
![asdasdasdsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/d25c3cc0-8c0d-482d-a474-687b90097e5a)

# Exploitation:
for the exploit i provided a buffer thats 1024 bytes so it will overflow the nonpaged memory and will cause a blue screen and terminate the system 
![asdasdasasdsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/b59e9bc4-e290-4e27-bac9-5fb26734dc72)
in this case i overloaded the LIST_ENTRY which triggered kernel protection
