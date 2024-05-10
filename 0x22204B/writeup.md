# Initial analysis of 0x22204B (BufferOverflowNonPagedPoolNxIoctlHandler, trigger TriggerBufferOverflowNonPagedPoolNx):
handler receives a "496" sized buffer and the "size" of the buffer that "should be" 496, trigger copies it
into nonpaged memory by the size provided, not the actual nonpaged size
![asdasdsadsadsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/16f8247e-a7b4-4f8a-aec7-13a9b1d9b620)
![asdasdasd](https://github.com/shaygitub/MY-HEVD/assets/122000611/fe8b4228-4c4c-443a-b367-92d9a1c881bf)

# Exploitation:
for the exploit i provided a buffer thats 1024 bytes so it will overflow the nonpaged memory and will cause
a blue screen and terminate the system
![sdas](https://github.com/shaygitub/MY-HEVD/assets/122000611/ac259d05-b64d-4423-9014-f422cd5f02c8)
in this case i probably overflowed some important LIST_ENTRY sitting in nonpaged memory
