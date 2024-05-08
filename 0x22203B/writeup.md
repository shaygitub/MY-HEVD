# Initial analysis of 0x22203B (InsecureKernelFileAccessIoctlHandler):
this function only calls another function named TriggerInsecureKernelFileAccess
![sadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/d902e32f-a074-460c-87d2-3008ea4c00de)

# TriggerInsecureKernelFileAccess:
![דשגדשגדשגדשג](https://github.com/shaygitub/MY-HEVD/assets/122000611/e5a78ace-2254-4602-b55b-09da522265ae)
this function just creates a file at C:\Windows\System32\HEVD.log and writes "HackSys Extreme Vulnerable Driver Log".

object attributes:
* Length is set to 48 which is supposed to be the size of the struct - sizeof(OBJECT_ATTRIBUTES). valid
* RootDirectory = 0: means that ObjectName correlates to full path not relative. valid
* ObjectName: valid
* Attributes = 576: correlates to OBJ_KERNEL_HANDLE | OBJ_CASE_INSENSITIVE. valid
* SecurityDescriptor = 0: will receive default. valid
* SUMMARY: VALID

ZwCreateFile:
* handle pointer: valid
* DesiredAccess = 0x2000000: MAXIMUM_ALLOWED, might very be a primitive (writing to file does not need maximum allowed permissions)
* object attributes and IoStatusBlock: valid.
* AllocationSize = 0: is a valid option (dont have to know the file size ahead of opening it)
* FileAttributes = 0x80: FILE_ATTRIBUTE_NORMAL, valid
* ShareAccess = FILE_SHARE_DELETE | FILE_SHARE_READ: seems to be valid
* CreateDisposition = 3 = FILE_OPEN_IF: opens file if exists and creates if does not. valid for log file
* CreateOptions = 0x60 = FILE_SYNCHRONOUS_IO_NONALERT | FILE_NON_DIRECTORY_FILE: seems to be valid
* EaBuffer = EaLength = NULL: valid
* SUMMARY: MAXIMUM_ALLOWED IS PROBABLY THE PRIMITIVE

ZwWriteFile:
* handle: "valid"
* Event = ApcRoutine = ApcContext = NULL: valid
* IoStatusBlock: valid
* Buffer: valid
* BufferSize: valid
* ByteOffset = Key = NULL: valid
* SUMMARY: VALID

this function returns the NTSTATUS returned from ZwCreateFile which makes sense as the vulnurability is probably there


# Possible vulnurabilities:
1) some kind of vulnurability when specifically using MAXIMUM_ALLOWED - this just means the driver could do anything with the file so if anyone got access
   to the driver and the handle it could be a possible primitive, but because it stays in this function there is nothing sensible with this primitive
2) given access to system32, someone could create a file called "HEVD.log" and instead data will be written in his file from the logs

# Exploit:
here i used the default way of communicating with a driver, but i created the file before calling the driver with read permissions.
because the driver specified that reading can be shared this would be OK, of course reading these logs could be achieved without creating 
the file yourself but i wanted to do everything in one script
![asdsadsadas](https://github.com/shaygitub/MY-HEVD/assets/122000611/318589a7-da5c-4ae6-91b7-96b898475cc6)

   
