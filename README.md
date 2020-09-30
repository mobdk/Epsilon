# Epsilon
In this PoC I am addressing the timer issue that exist in Defender.

This PoC successfully execute Mimikatz with no change to the orginal code, Mimikatz is converted to shellcode and embedded in source code, it uses the
following syscalls:

```
ZwOpenProcess
ZwAllocateVirtualMemory
ZwProtectVirtualMemory
ZwWriteVirtualMemory
"delay 15 seconds"
ZwOpenThread
ZwQueueApcThread
ZwResumeThread
ZwClose
```

If one insert a timer delay with value 15 seconds, anything below does not work, it breaks the engine detection logic and Mimikatz execute successfully. 
The delay can be inserted anywhere, between ZwOpenProcess and ZwClose.

Testing Defender try use extension .bin, .log, .edb or .db, this can be usefull if ASR rules exist. Fx. Epsilon.log (renamed from .dll)

Compile Epsilon.cs with csc.exe and insert .dll entrypoint as described here: https://github.com/mobdk/compilecs

Two precompiled version exist:
```
WithDelay.log execute with rundll32 WithDelay.log,#1
NoDelay.log execute with rundll32 NoDelay.log,#1
```
