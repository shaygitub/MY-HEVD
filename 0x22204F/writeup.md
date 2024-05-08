# Initial analysis of 0x22204F (MemoryDisclosureNonPagedPoolNxIoctlHandler):
this handler, similarly to 0x22203F, only calls another function, TriggerMemoryDisclosureNonPagedPoolNx, with the output buffer and size.
as can be seen both 0x22204F and 0x22203F do the exact same, only 0x22204F is with NX.
![asdasdsadsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/91884916-d318-4551-8cf7-64da2cc1723a)
![asdsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/f15f0f5f-dc9b-4958-ae1e-173030fc970c)

# Exploitation:
this exploit is the exact same as 0x22203F but this one has a different IOCTL obviously
![adsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/878fbe62-1c7d-47cd-9ec8-6c5502c70e04)
