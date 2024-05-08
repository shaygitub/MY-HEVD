# First review of 0x222023 (TypeConfusionIoctlHandler):
![sadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/78da322c-fa96-4862-8339-aafd656c3aef)
so as can be seen in WriteNULL (0x222047) also, Parameters will be a pointer to the user provided parameters, but now Parameters are provided to handler
as a pointer to USER_TYPE_CONFUSION_OBJECT, which means that the user input will need to be an instance of this struct

an analysis of this local struct:
typedef struct _USER_TYPE_CONFUSION_OBJECT{
PVOID ObjectID;
PVOID ObjectType;
} USER_TYPE_CONFUSION_OBJECT, *PUSER_TYPE_CONFUSION_OBJECT;  // User input size = 16 bytes

then as can be seen in the next picture, a nonpaged memory pool sized 16 bytes is allocated for KERNEL_TYPE_CONFUSION_OBJECT
this struct is defined locally like so:
typedef struct _KERNEL_TYPE_CONFUSION_OBJECT{
PVOID ObjectID;
union {
PVOID ObjectType;
PVOID Callback;
}___u1;
} KERNEL_TYPE_CONFUSION_OBJECT, *PKERNEL_TYPE_CONFUSION_OBJECT;  // Also 16 bytes
then, ObjectId and ObjectType are assigned from the UM structure into the allocated KM structure and the UM ObjectId is saved as a PVOID value
![sadsadasdsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/346a9adb-86b2-4375-845b-b0f0dc3180b2)

if memory cannot be allocated STATUS_NO_MEMORY is returned as the result of the operation. if not the return status of another function,
TypeConfusionObjectInitializer(KERNEL_TYPE_CONFUSION_OBJECT), is returned by the handler.

# TypeConfusionObjectInitializer analysis:
![asdasdsadsadsadasdsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/a3bded8c-7745-469a-84e7-7dbb9c8f4622)
this is very simple - the ObjectType, which is also interpreted as the Callback attributes of the kernel structure, is called
which means: i can make the driver execute any address of code that i want it to by providing ObjectType with an address

# Exploitation:
this is very simple - define the user structure in the exploit, move an address to execute to ObjectType and call the driver
in exploitation i provided 3 pictures:

1 - control over user provided buffer:
![dasdasdasdasdsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/114f380b-e543-4e1e-9f73-5a760b267046)

2 - assignment of my provided values into the KM structure:
![asdsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/9efdabfb-cf6b-4f61-8874-c475bc4cda22)

3 - execution of my provided address in ObjectType (in this case address is invalid for KM so ATTEMPTED_EXECUTE_OF_NOEXECUTE_MEMORY is triggered with my address):
![adasdsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/8aa15c3f-099c-4149-8c9f-d65c50ba088c)

# Possible primitives:
execution of any KM code that is valid and executable / system crash with execution of non-executable code
