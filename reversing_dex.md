# 4. Reverse Engineering Android Apps - DEX Bytecode

Now that we have starting points for where we think we ought to begin reversing, let's get started and then talk more about the "how" and "tips and tricks".

## Exercise 2 - Reverse Engineer the DEX 
In Exercise #2, we're going to continue with where we left off in [Exercise #1](reversing_intro.html#exercise-1---beginning-re-with-jadx) You can find this sample in the VM at `~/samples/ThaiCamera.apk`. The SHA256 digest of this sample is 55da412157e93153e419c3385ebcd5335bd0d0c3f77a75e2d2413dd128270be2. We are going to reverse engineer this application to determine if it is doing premium SMS fraud. 

To be doing premium SMS fraud, the application must send a premium SMS message (premium phone numbers are usually shortcodes) without disclosure that includes the phone number the SMS is being sent to and the cost of the SMS message and user consent.

### Goal
The goal of this exercise is to: Reverse engineer our 1st Android application (DEX code)! More specifically:

1. Learn to analyze the Java decompilation of the DEX bytecode in jadx to understand what the Android application is doing. 
1. Experience 1st hand how Android malware analysts apply reverse engineering to their context
1. 

### Exercise Context
You are a malware analyst for Android applications. You are concerned that this sample maybe doing premium SMS fraud, meaning that it sends an SMS to a premium phone number without disclosure & user consent. In order to flag as malware, you need to determine if the Android application is:

1. Sending an SMS message, and 
1. That SMS message is going to a premium number, and
1. If there is an obvious disclosure, and
1. If the SMS message is only sent to the premium number after user consent.

### Instructions

1. If jadx is not already started, start jadx by opening the terminal in the VM and running the `jadx-gui` command in the terminal and open ThaiCamera.apk in the jadx GUI in the same way as in Exercise #1.
1. Remember that the manifest is available under "Resources" by clicking on AndroidManifest.xml. To analyze the DEX bytecode, you can click on classes under the source code tab to open each class. jadx is a decompiler, rather than a disassembler, so it decompiles the DEX bytecode back to the Java. The decompiled Java will not look exactly the same as when the developer wrote it, however, it's often pretty close to functionally correct.
1. Begin analyzing the classes you identified as starting points in Exercise #1. When I say analyze, it's as simple as reading each line of decompiled output until you understand what a block of code is doing. Once you understand it, take a step back and evaluate the new information you learned against the questions you're trying to answer with your analysis (Exercise Context). Then decide where to analyze next. If you come from a Java development background, think about reverse engineering as debugging.

[//]: # TODO write answer pages for the different steps.


[**NEXT** > 4. Reverse Engineering Android Apps - DEX Bytecode](reversing_dex.md)