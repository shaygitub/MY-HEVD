# Initial analysis of 0x22203F (MemoryDisclosureNonPagedPoolIoctlHandler):
Handler function takes the output buffer and size into another function, TriggerMemoryDisclosureNonPagedPool.
trigger function allocated 0x1F8 bytes of nonpaged memory with tag "kcaH", sets everything to 65 decimal ('A'), moves ProvidedOutputSize bytes
into the ProvidedUserBuffer and free the original non paged memory.

![sadsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/50ffa2fa-d176-497e-8886-be4a895260f5)
![asdsadsadsadsadsad](https://github.com/shaygitub/MY-HEVD/assets/122000611/5253ed2b-27e1-479f-b2c1-c8e9f56ea2af)

# Exploitation:
exploit is simple - provide a 0x1F8 bytes sized array as OutputBuffer and 0x1F8 as the output size
![sdfsdfdsfdsfsd](https://github.com/shaygitub/MY-HEVD/assets/122000611/4886a137-0950-4e8e-bd9b-ad8beee765be)
