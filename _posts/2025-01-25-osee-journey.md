---
title: "My Journey to OSEE: Conquering the Advanced Windows Exploitation (AWE) Course & Exam"
categories: 
    - Blog
tags:
    - OSEE
    - exploit development
---

If you’re reading this, you’re probably either considering Advanced Windows Exploitation (AWE) or preparing for the Offensive Security Exploitation Expert (OSEE) exam. Maybe you’re just curious about what it takes to pass one of the most challenging certifications in cybersecurity.  

Either way, buckle up—this is an unfiltered breakdown of my experience.  

This is the story of months of preparation, intense debugging sessions, two exam attempts, and a complete reevaluation of how I approach exploitation.  

Who Should Take the AWE Course?
---

Before signing up for AWE, I asked myself:  

- Am I ready for this?  
- What experience should I have before jumping in?  

Here’s the truth: AWE is not a beginner-friendly course. You need a strong foundation in exploit development before you even think about enrolling.  

### Must-Have Skills Before AWE:

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

The first day eased us in with a simple warm-up:  

- Write a custom x64 reverse shellcode from scratch.  

Then things escalated quickly.  

- Exploiting a Use-After-Free (UAF) vulnerability in VMware’s RPC backdoor mechanism.  
- Bypassing Windows Defender Exploit Guard hardening.  

Days 2-3: Browser Exploits & Sandbox Escapes
---

Next, we focused on browser exploitation, specifically targeting a Type Confusion vulnerability in Microsoft Edge’s Chakra JavaScript engine.  

The challenge was turning arbitrary read/write primitives into remote code execution while bypassing:  

- Control Flow Guard (CFG)  
- Arbitrary Code Guard (ACG)  
- Code Integrity Guard (CIG)  
- CET (Shadow Stack)  

Even after achieving code execution, we were still sandboxed. We had to chain it with a separate CVE in a COM component to break out of the sandbox.  

By the end of the third day, I had successfully popped a reverse shell just by (optionally) clicking a button in Edge.  

Days 4-5: Kernel Exploitation & HVCI Bypasses
---

The last two days focused entirely on Windows kernel exploitation.  

We started with a third party driver vulnerability, bypassing SMEP (Supervisor Mode Execution Prevention) by manipulating Page Table Entries (PTEs). Then, we moved on to exploiting native driver, where we:  

- Used pool memory leaks for info leaks.  
- Built heap spray techniques to gain stable exploitation.  
- Bypassed HVCI (Hypervisor-Protected Code Integrity) using a novel gadget chain.  

By the end of the course, I had learned more about Windows exploitation in five days than in the past year of self-study.  

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
