# First review of 0x222027 (IntegerOverflowIoctlHandler):
this handler is really simple - take the user input and its size (as can be seen from intrepretation in the next function) and provide both
of them to the function -
![handler](https://github.com/shaygitub/MY-HEVD/assets/122000611/93863f47-3cf1-4991-94a8-5642056d2cb8)
the called function is even simplier:
- Provided size needs to be <= 0x7FC (2044)
- while current local index < quarter of the size (each value is 4 bytes, dummy is the count of objects) and value != 0xBAD0B0B0,
  iterate over DWORD objects inside user array and assign them to an INT array (integer overflow, 4 bytes unsigned value assigned to 4 bytes signed variables)

if we make the size of the user provided array 0x7FC (2044):
loop will iterate on indexes 0 to 510 (including first and last)
![sass](https://github.com/shaygitub/MY-HEVD/assets/122000611/3dea02c8-584d-42a8-bc98-35472961420e)

# Exploitation:
for exploitation i just created an DWORD array with 511 objects inside it. I initialized each value to 0x8CF19AC9 (MSB is on, value will be negative) to 
create an integer overflow on each element of the array. i passed the array to the driver and it indeed worked
![sn1](https://github.com/shaygitub/MY-HEVD/assets/122000611/6e6a102d-9caf-49dd-8ded-218e9458d355)
![asdasdasdasdas](https://github.com/shaygitub/MY-HEVD/assets/122000611/8028e85a-173f-4b89-be77-ff0963f7488d)


# Possible primitives:
Integer overflow? :)
