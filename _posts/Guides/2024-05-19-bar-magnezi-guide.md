---
title: "Reverse Engineering Guide"
classes: wide
header:
  teaser:  /assets/images/2.jpg
ribbon: black
description: "Unpacking Malware"
categories:
  - Guides
tags:
  - Reverse
  - Encryption
toc: true
---
---
Sample:
```
ec67029dba040a8a3f98bda4601089a6
```

# Background
Breaking Down SmokeLoader Sample, using x32dbg to Debug and Unpack the original file.
# Static Analysis
<div style="text-align: center;">
    <img src="/assets/images/ReverseEngineering_Guide_01/Smokeloader entry.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 1: Malware Bazaar Entry</sub>
</div>
<br>

Initially, I conducted static analysis by examining strings and using PEStudio to understand the malware’s behavior, imports, libraries, and other characteristics.
It is important to gather as much information as possible before starting the reverse engineering process. The more information and understanding you have about the binary, the easier it will be to reverse engineer it.


You can identify packed malware by:

1. **Virtual Size Discrepancy:** Packed malware often has sections where the Virtual Size exceeds the Raw Size, indicating potential packing.
2. **High Entropy Sections:** Packed malware may contain sections with high entropy, suggesting compression or encryption.
3. **Import Table Anomalies:** Packed malware might show irregularities in the import table, like missing or incomplete entries, due to modifications by packers.
Most of this information can be observed using tools like PEStudio and DIE.

Since the binary is 32-bit, I opened it in x32dbg and examined the entry point. After analysis, I decided to set a breakpoint at **VirtualAlloc**.

When malware is packed, its code and data are often compressed or encrypted to evade detection. To execute its malicious payload, the malware needs to unpack itself into memory. This is where VirtualAlloc comes into play — it allows the malware to allocate memory where it can unpack and execute its payload.


# Reversing

<div style="text-align: center;">
    <img src="/assets/images/ReverseEngineering_Guide_01/Ctr+G to bp on virtual alloc.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 2: Using Ctl+G to find VirtualAlloc</sub>
</div>
<br>

<div style="text-align: center;">
    <img src="/assets/images/ReverseEngineering_Guide_01/setting bp at the start and the end.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 3: Setting breakpoints at the beginning and at the end of VirtualAlloc</sub>
</div>
<br>

After setting the breakpoint, the program can be executed until the breakpoint is reached. Observing the EAX register reveals changes in memory; after each execution, there is a slight modification. It was decided to continue running until the function returns and observe the memory, revealing indications of an executable file being unpacked in memory, as depicted in Figure 4.

<div style="text-align: center;">
    <img src="/assets/images/ReverseEngineering_Guide_01/after pressing execute till return - bulding AEX.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 4: MZ Header indicating of an exe</sub>
</div>
<br>


<div style="text-align: center;">
    <img src="/assets/images/ReverseEngineering_Guide_01/Follow in memory.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 5: Following in memory dump to this address</sub>
</div>
<br>

After dumping this to a file there was still some garbage code that was needed to be cleaned.

<div style="text-align: center;">
    <img src="/assets/images/ReverseEngineering_Guide_01/opening in hex editor - need to clean whats before MZ.PNG" alt="Screenshot1" />
    <br>
    <sub>Figure 6: Opening in HexEditor and cleaning the marked data</sub>
</div>
<br>

After cleaning the code, I was left with the new EXE, which is the actual malware that can be further investigated.

# Summary

1. **Conduct Initial Analysis:** Begin by conducting static analysis, This helps in understanding the malware’s behavior, imports, libraries, and other characteristics.
2. **Identify Packed Malware:** Look for signs of packed malware, such as discrepancies in virtual size, high entropy sections, and anomalies in the import table.
3. **Set Breakpoint at VirtualAlloc:** Set a breakpoint at VirtualAlloc, which is often used by packed malware to allocate memory for unpacking and executing its payload.
4. **Execute Until Breakpoint:** Run the program until the breakpoint is reached. Observe changes in the EAX register, which indicate modifications in memory.
5. **Continue Execution:** Continue running until the VirtualAlloc function returns. Observe memory for indications of an executable file being unpacked.
6. **Dump Unpacked File:** Once the unpacking process is complete, dump the unpacked file from memory to a file. There may be some garbage code that needs to be cleaned.


# IOCs

- Hash:
```
ec67029dba040a8a3f98bda4601089a6
46404801da9e3f92ceafdde930ca25ff
```





