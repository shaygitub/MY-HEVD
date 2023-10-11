# MY-HEVD
uses the HackSysExtreme vulnerable driver, inspired by and based on https://connormcgarr.github.io/Kernel-Exploitation-1/
this tool expands on the token stealing functionality presented in the original exploit with a remotely controlled
"command center" with priveledged operations that can be executed by request, some of them are:
- changing bootloader settings via BCDedit (turning on debugging of computer, changing debugging settings..)
- reading, writing, copying and transfering files (including SYSTEM files)
- executing specific scripts sent by attacker
- http operations like making requests for resources, opening an http server on the target..
- executes default regular and priviledged cmd operations and commands
(NOT YET FINISHED - a side project)
