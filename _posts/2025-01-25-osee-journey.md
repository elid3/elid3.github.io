---
title: "My Journey to OSEE: Conquering the Advanced Windows Exploitation (AWE) Course & Exam"
categories: 
    - Blog
tags:
    - OSEE
    - exploit development
---

If you’re reading this, you’re probably either considering Advanced Windows Exploitation (AWE) or preparing for the Offensive Security Exploitation Expert (OSEE) exam. Maybe you’re just curious about what it takes to pass one of the most challenging certifications in cybersecurity.  

Either way, buckle up—this is an unfiltered breakdown of my experience. This is the story of months of preparation, intense debugging sessions, two exam attempts, and a complete reevaluation of how I approach exploitation.  

Who Should Take the AWE Course?
---
Before signing up for AWE, I asked myself:  

- Am I ready for this?  
- What experience should I have before jumping in?  

Here’s the truth: AWE is not a beginner-friendly course. You need a strong foundation in exploit development before you even think about enrolling.  

Must-Have Skills Before AWE:

- Basic exploit development knowledge—If you’ve done OSCP, OSED, or have real-world experience writing exploits, you’re on the right path.  
- Understanding of Windows internals—You should be familiar with memory management, process isolation, and exploit mitigations.  
- Reverse engineering skills—IDA Pro and WinDbg will become your best friends (or worst enemies).  
- Some exposure to kernel exploitation—Even if you’ve never written a full kernel exploit, knowing the fundamentals (e.g., HEVD) helps.  

If you don’t have these skills yet, I recommend:  

- Start with OSED (EXP-301)—This builds a great foundation in user-mode exploitation.  
- Practice Windows exploitation in VulnServer or HEVD—This will teach you memory corruption techniques.  

The AWE Course: A Week of Intense Learning
---
I attended the AWE course in Singapore at SinCon 2024, and it was an intense five-day experience. By the time I boarded my flight to Singapore for the AWE course in May 2024, I felt ready. Spoiler: I wasn’t.

Our instructors were Morten Schenk (@blomster81) and Alexandru Uifalvi (@sickness).

Day 1: Welcome to Windows Exploitation
---
The first day began with a relatively straightforward warm-up exercise: writing a custom x64 reverse shellcode from scratch. This helped establish a strong foundation before diving into more complex exploitation techniques.

Once warmed up, we shifted focus to VMware Workstation internals, specifically analyzing the VMware Backdoor RPC Guest-to-Host communication mechanism. We explored how commands are sent between the guest and host, understanding the role of functions like Backdoor_InOut and the process of opening, sending, receiving, and closing an RPC communication channel.

With this knowledge in hand, we moved on to an advanced Use-After-Free (UAF) vulnerability in VMware Workstation's Drag & Drop feature. The challenge was transforming the UAF bug into a reliable arbitrary read/write primitive, which required an in-depth understanding of the Windows heap memory manager. We studied both the front-end and back-end allocators, including how the Low Fragmentation Heap (LFH) operates.

Once we had reliable heap control, we took a deeper look at reallocation strategies and memory corruption techniques. We examined functions like guest.upgrader_send_cmd_line_args, addressed the NULL byte issue, and built a fake virtual table to control execution flow.

To escalate our attack, we leveraged Return-Oriented Programming (ROP) to bypass modern exploit mitigations. We started by locating useful gadgets with rp++, setting up a stack pivot, and crafting a ROP chain to bypass Data Execution Prevention (DEP). Using GetModuleHandle, GetProcAddress, and WriteProcessMemory ROP chains, we successfully executed arbitrary code on the host machine.

The final challenge of the day was bypassing Address Space Layout Randomization (ASLR) by hunting for leaked pointers, followed by defeating Windows Defender Exploit Guard (WDEG) protections. We analyzed WDEG mitigations like Export Address Filtering (EAF) and implemented techniques to disarm and disable these protections in practice.

By the end of the first day, we had successfully developed a full VMware guest-to-host escape exploit, proving our ability to manipulate memory, bypass modern exploit mitigations, and execute shellcode on the host machine.

Days 2-3: Browser Exploits & Sandbox Escapes
---
After successfully completing the VMware guest-to-host escape, we shifted our focus to browser exploitation, specifically targeting a Type Confusion vulnerability in Microsoft Edge’s Chakra JavaScript engine. This involved an in-depth study of Edge internals, beginning with the JavaScript engine, Chakra internals, and the Just-In-Time (JIT) compiler, which plays a crucial role in Type Confusion vulnerabilities.

We started by triggering the vulnerability and performing a root cause analysis to understand how memory corruption occurs. The first major challenge was transforming a Use-After-Free (UAF) bug into reliable arbitrary read and write primitives. We achieved this by controlling the auxSlots pointer, abusing it to create a stable memory corruption primitive, and developing a method for arbitrary memory access.

With a working read/write primitive, we moved towards achieving code execution. This involved overwriting return addresses and bypassing Control Flow Guard (CFG) protections. We explored various methods, including return address overwrites, Intel CET protections, and out-of-context calls, each designed to harden modern browsers against exploitation.

Once we bypassed CFG, the next step was defeating Data Execution Prevention (DEP) using Return-Oriented Programming (ROP). We built a ROP chain to locate required functions dynamically, ensuring our exploit was adaptable across different system configurations. Additionally, we tackled Address Space Layout Randomization (ASLR) by hunting for leaked pointers, allowing us to precisely control memory execution.

However, even after achieving remote code execution, the exploit was still confined within Edge’s sandbox. To fully compromise the system, we needed to chain our exploit with a separate sandbox escape vulnerability in a COM component. We analyzed sandbox theory and escape mechanisms, identifying weaknesses in Windows' sandboxing model that allowed us to break out. The final step involved leveraging insecure access permissions, manipulating language execution environments, and abusing Activation Factories to execute arbitrary code outside the sandbox.

By the end of the third day, I had developed a fully functional, version-independent exploit that not only bypassed multiple security mechanisms but also escaped the browser sandbox, culminating in a reverse shell execution—triggered simply by clicking a button in Edge.

Days 3-4: Driver Callback Overwrite
---
We began the day with an in-depth look into the Windows kernel, covering privilege levels, Interrupt Request Levels (IRQL), and debugging techniques such as remote kernel debugging over TCP/IP, serial ports, and VMware’s VirtualKD.

Once the foundation was set, we moved on to kernel exploitation, starting with how the Windows kernel communicates with user mode through system calls and device drivers. From there, we explored various kernel security mitigations and vulnerability classes, setting the stage for practical exploitation.

The key challenge of the day was exploiting a driver callback overwrite vulnerability. We worked through triggering the vulnerability and gaining control over the callback context before redirecting execution to user mode while bypassing Supervisor Mode Execution Prevention (SMEP). To achieve stable exploitation, we studied memory paging structures, including PML4 self-references and their randomization.

We then transitioned to ROP-based techniques, beginning with stack pivoting and crafting a reliable kernel read/write primitive. After leaking the Virtual PTE Start and manipulating Page Table Entries (PTEs), we flipped the User/Supervisor (U/S) bit to gain arbitrary kernel memory access. Finally, we explored techniques for bypassing Kernel VA Shadow (Meltdown mitigation) and executing token stealing payloads for privilege escalation.

By the end of the day, we had successfully developed a working kernel exploit, proving our ability to bypass SMEP and achieve arbitrary code execution in kernel mode.

Days 4-5: Unsanitized User-Mode Callback
---
The final days was dedicated to attacking Windows kernel user-mode callbacks, focusing on a vulnerability in the Windows Desktop Window Manager. We began by analyzing Windows desktop applications and their interaction with the kernel, studying how the Windows kernel allocates and manages pool memory.

The challenge of the day was crafting a reliable exploit for an unsanitized user-mode callback vulnerability. We started by reversing the TagWND object and exploring how Windows kernel user-mode callbacks work. By leaking pWND user-mode objects and spraying the desktop heap, we gained a primitive for arbitrary WndExtra overwrites, which allowed us to corrupt kernel memory structures.

Once we had control over the TagWND write primitive, we overwrote key fields such as pWND[0].cbWndExtra and pWND[1].WndExtra to escalate privileges. We then leveraged this primitive to leak memory addresses, modify security descriptors, and escalate privileges from low integrity.

In the final stage, we studied Windows virtualization-based security mechanisms, including the Windows Hypervisor, and explored how to debug and bypass these protections. Using a combination of kernel-mode routine hijacking and data-only attacks, we achieved full kernel code execution. By the end of the course, I had successfully leveraged multiple kernel vulnerabilities to escalate privileges, bypass security mitigations, and execute arbitrary code at the highest privilege level.

Looking back, I had learned more about Windows exploitation in five days than in the past year of self-study. The course provided a deep understanding of modern Windows security, exploit development, and bypassing kernel protections in real-world scenarios.

The OSEE Exam: 72 Hours of Intensity
---
After finishing the AWE course, I took 5 months to revisit the labs, build custom exploits, and refine my understanding of kernel internals.  

Then, I booked the OSEE exam—and this is where things got real.  

How the OSEE Exam Works:
- You get two challenges.  
- You have 72 hours.  
- You need 75/100 points to pass.
  
Attempt 1: The Strategic Failure
--- 
I walked into my first attempt knowing I wasn’t fully ready. But I also knew that failing early would expose my weaknesses and help me prepare better for the second try.  

The first few hours went well—I set up my environment, analyzed what I was dealing with, and mapped out a rough plan. Then, reality hit.  

I got stuck on small details that ate up hours. I second-guessed my approach, overcomplicated solutions, and burned way too much time on debugging instead of stepping back and reassessing. Sleep was an afterthought, and by the second night, I was running on fumes. I could feel my brain slowing down, but I kept pushing, convinced that more hours meant better results. Spoiler: it was not. Good sleep significantly impacts exam performance.

By the time I reached the final stretch, I was mentally exhausted, frustrated, and completely out of ideas. The realization sank in: I wasn’t going to pass.  

Attempt 2: The Redemption Arc
---
The next day, I started planning my rematch.  

I restructured my study plan, focusing on efficiency rather than sheer effort. I forced myself to take breaks, get sleep, and approach problems with a clearer mind.  

When the second attempt started, I felt different. The stress was still there, but this time, I controlled it instead of letting it control me.  

The same roadblocks appeared, but I tackled them faster. Instead of wasting hours chasing bad leads, I recognized when to pivot. I took naps, forcing myself to reset rather than spiral into frustration. My approach was deliberate, methodical, and far more effective.  

At hour 55, I knew I had passed. But I kept pushing—not because I had to, but because I finally felt in control. By hour 65, I had 100/100 points.  

When I submitted my report this time, there was no doubt in my mind. I had earned it.  

Lessons Learned
---
- Hard work isn’t enough—smart work matters more.  
- Sleep is a weapon. Fatigue makes simple problems feel impossible.  
- Struggle is part of the process, but drowning in it isn’t. Step back, breathe, and reassess.  

The first attempt broke me. The second attempt built me back stronger.

---
## Final Thoughts: Should You Go for OSEE?

If you love deep technical challenges, thrive on reverse engineering, and want to push your skills to the absolute limit, then OSEE is worth it.  

Just know this: It will test you in ways you’ve never been tested before.  

- Give yourself 4+ months to prepare.  
- Embrace failure—it’s the best teacher.

The experience changed how I think about exploitation, and despite all the struggles, I would do it again.
