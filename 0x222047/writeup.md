# First review of 0x222047 (WriteNULLIoctlHandler):
if parameters are provided to the operation via the NAMED_PIPE_CREATE_PARAMETERS in Irp->Parameters.CreatePipe.Parameters the operation continues,
otherwise the operation returns STATUS_UNSUCCESSFUL. means:
the driver needs parameters for named pipe creation.

typedef struct _NAMED_PIPE_CREATE_PARAMETERS {
    ULONG NamedPipeType;
    ULONG ReadMode;
    ULONG CompletionMode;
    ULONG MaximumInstances;
    ULONG InboundQuota;
    ULONG OutboundQuota;
    LARGE_INTEGER DefaultTimeout;
    BOOLEAN TimeoutSpecified;
} NAMED_PIPE_CREATE_PARAMETERS, *PNAMED_PIPE_CREATE_PARAMETERS;
![asdasdsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/d88bbb78-5b3d-42f9-86f3-746029dc2af8)

function continues to call TriggerWriteNULL with the PNAMED_PIPE_CREATE_PARAMETERS parameter

# Inside TriggerWriteNULL:
- ProbeForRead is used to validate parameters pointer
- first 8 bytes of struct are copied as a QWORD (8 byte value) into a variable
- some logs are printed, and then that 8 byte value is used as an address that points to an 8 byte value
this means: if i can provide parameters for named pipe creation but specifically make NamedPipeType and ReadMode = 0,
this will trigger a NULL write vulnurability
![sadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/3a21920c-7c7d-4d14-ad30-11289d5196f0)


# Exploitation:
my first idea was to try and pass the NAMED_PIPE_CREATE_PARAMETERS* as the buffer for the exploit to the driver,
and then try and set the first 2 attributes of the driver (NamedPipeType, ReadMode) to a certain value i will
be able to see in windbg.
i decided to use   ReadMode = 0xDEADBEEF, NamedPipeType = 0xDEAFBEAD, and indeed the value of UserPointerToNullify
was 0xdeadbeefdeafbead (which is reversed for some reason as NamedPipeType is the first parameter):
![deaf](https://github.com/shaygitub/MY-HEVD/assets/122000611/8ec02393-d671-4b8b-99c2-c4cf3a08e04c)

so now when i figured how to exactly control the parameters to the driver that will make UserPointerToNullify = certain value,
i zero-ed out these 2 parameters to trigger an exception (NULL write). this exception will show up as status 0xC0000005/access violation
but because the driver catches the exception it will not crash the system:
before exception -
![asdasdsadas](https://github.com/shaygitub/MY-HEVD/assets/122000611/7827d37f-cd65-4d86-acd2-205edb974116)

after exception -
![after](https://github.com/shaygitub/MY-HEVD/assets/122000611/35bf587f-5b31-4e16-9da1-e0c59832a294)


# Final note:
as can be seen here, Parameters/UserBuffer are the pointer provided by the user and UserPointerToNullify is the value in it, constructed from
first 4 bytes as least significant bytes and the second 4 bytes provided will be the most significant 4 bytes. input does not have to be the 
struct i mentioned here, its probably some sort of obfuscation with IDA but i just had to provide an input of atleast 8 bytes with the address
to nullify its content at the first 8 bytes
