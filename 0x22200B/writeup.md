# Initial analysis of 0x22200B (ArbitraryWriteIoctlHandler, trigger TriggerArbitraryWrite):
handler only takes the input buffer provided as WRITE_WHAT_WHERE* (input size = sizeof(WRITE_WHAT_WHERE)).
this struct is passed to the trigger parameter, struct is:
typedef struct _WRITE_WHAT_WHERE{
ULONG64* What;
ULONG64* Where;
} WRITE_WHAT_WHERE, *PWRITE_WHAT_WHERE;

trigger function only writes *What into *Where (arbitrary write)
this arbitrary write can be used to trigger 2 primitives:
1) write into a specific address that is writeable
2) trigger a PAGE_FAULT_IN_NON_PAGED_AREA/access violation when writing into an invalid address

# Exploitation:
just pass this struct with a pointer to the value to write and a pointer to the destination of
writing (in this case i triggered a write into 0x1122334455667788 so an access violation will be triggered,
8 byte value to write is 0x99AABBCCDDEEFF00)
![asdsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/046e40e3-a223-44d5-b84a-7b8f8c1e3970)
![asdsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/d5b0ec77-12b7-4c97-93d4-b6aa717b587f)
