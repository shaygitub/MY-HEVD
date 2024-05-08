# Initial analysis of 0x22202B (NullPointerDereferenceIoctlHandler):
this handler function just passes the input buffer to the triggering function.
![image](https://github.com/shaygitub/MY-HEVD/assets/122000611/686341b4-626a-4206-9f2c-8e244387f99e)

the trigger function, , allocates 0x10 bytes of nonpaged memory and checks a 4 byte value pointed to by the user input buffer:

# UserValue == 0xBAD0B0B0:
kernel pool layout:
0-4 -> 0xBAD0B0B0
4-8 -> zero
8-16 -> NullPointerDerefrenceObjectCallback

# UserValue != 0xBAD0B0B0:
kernel pool layout: all zero (nothing is changed)

![asdasdsadsadsadsadsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/0e6de07e-a7ac-4d3d-b59a-f9ef636efb95)

then it initializes a variable as a pointer to a function that receives and returns nothing (void), the address casted
here is KernelPool[1] (second 8 bytes of buffer, NullPointerDerefrenceObjectCallback in first case, NULL in second case).
then the variable gets called, specifically:

# Variable == NullPointerDerefrenceObjectCallback:
call NullPointerDerefrenceObjectCallback()  (Fine, just prints a log message)

# Variable is not the same value:
call Variable();  // In the case it gets here, this means the trigger function will call NULL

![adsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/0aa8e7b3-8478-49bf-a46d-5a66df3b280a)

# Exploit:
for this, we just need to provide a pointer to a 4 byte value that is not 0xBAD0B0B0, size of 4 bytes for input (not mandatory here) and an
exception is supposed to be triggered (I provided 0xG00DB0B0)
![userv](https://github.com/shaygitub/MY-HEVD/assets/122000611/388d242b-9a7a-4ce7-9293-a975841d520a)
![asdasd](https://github.com/shaygitub/MY-HEVD/assets/122000611/b49023ba-4045-4410-8a45-a49b74410fb0)
