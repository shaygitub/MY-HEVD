# Initial analysis of 0x222037 (DoubleFetchIoctlHandler, TriggerDoubleFetch):
handler sends a _DOUBLE_FETCH struct to the trigger function
_DOUBLE_FETCH: PVOID Buffer; ULONG64 Size
trigger takes the size, checks if LOCAL size is lower than 0x800/2048 bytes. if it is
the trigger continues to take the size ANOTHER TIME from the pointer
![asdasdasd](https://github.com/shaygitub/MY-HEVD/assets/122000611/d8629ca6-b212-421d-8fc6-96ca916810f5)
![adsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/a7add108-9966-4684-9fd7-4bbb8877ab15)

# Exploitation:
I passed a buffer with the size of 3000 and just before sending the IOCTL i made sure the value
is 2048, and then i created a thread that cnahges between the values to 3000
