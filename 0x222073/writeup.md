# Initial analysis of 0x222073 (ArbitraryIncrementIoctlHandler, trigger TriggerArbitraryIncrement):
Handler just takes input of driver and passes it to the trigger function
![hn1](https://github.com/shaygitub/MY-HEVD/assets/122000611/fb4db823-c37c-4967-a2c8-95c56c48027f)

Trigger function takes a BYTE* from the input parameters (input parameter is 8 bytes and is a pointer to an
address to increment). then the trigger increments the value inside the value of the user provided pointer
that points to another pointer/address value
![tr1](https://github.com/shaygitub/MY-HEVD/assets/122000611/5e6e3588-e06a-46ad-af72-c923f23637cc)

# Exploitation:
with this functionality i made two iterations:
1) i gave a legitimate pointer to a BYTE value that is supposed to be incremented
2) i gave an invalid address so the driver will crash the system
![asdsadsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/f4895409-a40b-4e70-9335-321c739fb258)
![actf](https://github.com/shaygitub/MY-HEVD/assets/122000611/6d9a1f81-4734-4d2d-95a7-3ad5710ddaf7)
![adsd](https://github.com/shaygitub/MY-HEVD/assets/122000611/68928c6d-6b09-4087-8108-24a9bbc8b22b)
