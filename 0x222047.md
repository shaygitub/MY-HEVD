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
in work
