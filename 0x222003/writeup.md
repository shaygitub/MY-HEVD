# Initial analysis of 0x222003 (BufferOverflowStackIoctlHandler, trigger TriggerBufferOverflowStack):
handler receives a buffer as input that "should be" 0x800/2048 bytes and the size of input.
size of input is not verified and the user data gets copied into the 2048 bytes array in the kernel
by the size provided to the driver, not the size of the array.
so, if i provide a buffer the size of 3000 and provide 3000 bytes as the size i would cause a buffer
overflow in the kernel stack
![asdasdsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/7c665f7b-1ca3-49e5-b903-775a6aa32c01)
![dgdfgdfgdf](https://github.com/shaygitub/MY-HEVD/assets/122000611/4ca9a9aa-6c70-4700-95ae-8f1571722fec)

# Exploit (like i explained above):
![asdasdasdsadsadsa](https://github.com/shaygitub/MY-HEVD/assets/122000611/e614400f-22be-43ea-9b91-66f5b5684c3f)
