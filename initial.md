# Initial analysis of HEVD.sys:
![init](https://github.com/shaygitub/MY-HEVD/assets/122000611/bbb88ccb-90d7-4837-9ad2-7432f22f0404)
 - symbolic link and device name are initialized, IoCreateDevice is called to create a device
IoCreateDevice is called, driver object is ok,
* VECTOR: device extension is NOT allocated, could be trying to use unallocated memory later because of the 0 2nd parameter,
name seems ok, parameter passed for device type is 0x22 (FILE_DEVICE_UNKNOWN) which is usually used when type is not important,
device charectaristics is 0x100 while documentation says FILE_DEVICE_SECURE_OPEN (0x100) is usually used to describe the device,
exclusive is 0 (FALSE) which is usually used, DEVICE_OBJECT pointer is provided correctly (for now no primitive)

return status of IoCreateDevice triggers two flow paths:

# status >= 0 (0 <= status < 0x80000000):
this status continues the regular flow of commands, which show that even if an error occured, only certain groups of errors are considered
termination errors.  then memset64 is used to initialize all major functions to point to IrpNotImplementedHandler before setting the
handled major functions. before going into IrpDeviceIoCtlHandler, we can see that the Flags value of the driver is (0 | 0x10) & 0x80 = 16 / 0x10.
symbolic link is created and log is printed.

# status >= 0x80000000:
in here, if the device object was allocated it is freed and log is printed. but before this _mm_lfence() is called which is not relevant for
what im doing.

# IrpDeviceIoCtlHandler analysis:
clear switchcase of IOCTL codes, also implements an INVALID_PARAMETER handling.
here the handling for each IOCTL (format) is the exact same:
- log print
- operation is called and status is assigned (I called the variable OperationStatus)
- a string is saved in a variable
- jumps to LABEL_64 (named it OperationEndLabel)
- LABEL_64 prints the string, operation status is later assigned to another variable that is the final status returned by handler
![lg](https://github.com/shaygitub/MY-HEVD/assets/122000611/96ee5a7b-ec71-42bb-a557-44bb5391065b)
![gl](https://github.com/shaygitub/MY-HEVD/assets/122000611/7d7c3912-7586-42b5-b59b-23106fb084d7)
although ida chose to analyze IOCTL codes in seperated groups, i will analyze them one by one in each blog entry
