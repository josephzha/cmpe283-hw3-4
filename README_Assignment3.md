# cmpe283-hw3-4

## Answers to questions:
1. Work Distribution
   1. Yucong Ma: wrote the code in cpuid.c and vmx.c
   2. Joseph Zhao: debug and run the code by Yucong Ma, wrote test.c in inner VM.
2. Steps
   1. In cpuid.c, create a list of u32 to store counters for each exit reason.
   2. In cpuid.c, kvm_emulate_cpuid(), insert if statement to intercept when eax is equal to 0x4ffffffe
   3. Within the block, check if exit reason(ecx) is between 0 and 74. If not, set eax=ebx=ecx=0, edx = 0xffffffff
   4. If exit reason is between 0 and 74, check if kvm support that exit reason(using linux/arch/x86/include/uapi/asm/vmx.h). If exit reason is not supported, set eax=ebx=ecx=edx=0, and return. 
   5. Set eax to the counter value of that exit reason from the counter list.
   6. use "make -j 4 && sudo make -j 4 modules && sudo make install && sudo make modules_install" to build modules and restart the VM.
   7. Boot up inner VM. In test.c, use a loop to call cpuid with eax = 0x4ffffffe and ecx = 0 through 74, print out the result from eax.
3. Each reboot entail 700K exits. The exits increase at a stable rate. 
4. The most frequent exit type is 48:EPT violation. There are many exit types with 0 exit counts.
