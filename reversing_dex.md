#### Table of Contents

1. [Introduction](index.html)
1. [Android Application Fundamentals](app_fundamentals.html)
1. [Getting Started with Reversing Android Apps](reversing_intro.html)
    * [Exercise 1](reversing_intro.html#exercise-1---beginning-re-with-jadx)
1. [Reverse Engineering Android Apps - DEX Bytecode](reversing_dex.html)
	* [Exercise 2](reversing_dex.html#exercise-2---reverse-engineer-the-dex)
	* [Exercise 3](reversing_dex.html#exercise-3---reverse-engineer-the-dex-to-identify-the-vuln)
	* [Exercise 4](reversing_dex.html#exercise-4---arbitrary-command-execution-take-2)
1. [Reverse Engineering Android Apps - Native Libraries](reversing_native_libs.html)
	* [Exercise 5](reversing_native_libs.html#exercise-5---find-the-address-of-the-native-function)
	* [Exercise 6](reversing_native_libs.html#exercise-6---find-and-reverse-the-native-function)
1. [Reverse Engineering Android Apps - Obfuscation](obfuscation.html)
	* [Exercise 7](obfuscation.html#exercise-7---string-deobfuscation)
1. [Conclusion](conclusion.html)



# 4. Reverse Engineering Android Apps - DEX Bytecode

Now that we have starting points for where we think we ought to begin reversing, let's get started and then talk more about the "how" and "tips and tricks".

## Exercise 2 - Reverse Engineer the DEX 
In Exercise #2, we're going to continue with where we left off in [Exercise #1](reversing_intro.html#exercise-1---beginning-re-with-jadx) You can find this sample in the VM at `~/samples/ThaiCamera.apk`. The SHA256 digest of this sample is 55da412157e93153e419c3385ebcd5335bd0d0c3f77a75e2d2413dd128270be2. We are going to reverse engineer this application to determine if it is doing premium SMS fraud. 

To be doing premium SMS fraud, the application must send a premium SMS message (premium phone numbers are usually shortcodes) without disclosure that includes the phone number the SMS is being sent to and the cost of the SMS message and user consent.

### Goal
The goal of this exercise is to: Reverse engineer our 1st Android application (DEX code)! More specifically:

1. Learn to analyze the Java decompilation of the DEX bytecode in jadx to understand what the Android application is doing. 
1. Experience 1st hand how Android malware analysts apply reverse engineering to their context

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

### Solution

<iframe width="560" height="315" src="https://www.youtube.com/embed/pvgLRWxsOd0" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<br/>

## Exercise 3 - Find the Vulnerability in the Adups OTA Application
The two exercises up until this point have focused on reverse engineering an Android app in order to determine if it is malware. Now, let's apply our reverse engineering skills to finding a vulnerability in an application. You can find the sample for this exercise in `~/samples/FotaProvider.apk`. The SHA256 digest for the sample is 6fddd183bc832659cbea0e55d08ae72016fae25a4aa3eca8156f0a9a0db7f491. 

### Goal
The goal of this exercise is to apply our DEX reverse engineering skills to finding a vulnerability in an Android app. This example is a little more complex and will introduce us to reversing across different components of the application.

### Exercise Context
You are auditing a set of phones for security issues prior to allowing them onto your enterprise network. You are going through the apps that come pre-installed. For this pre-installed application, you are concerned that there may be a vulnerability that allows it to run arbitrary commands.

### Instructions

1. Start jadx as described in Exercise #1 and open FotaProvider.apk. 
1. Use the Manifest to identify interesting components and the bytecode to analyze the code.
1. Find a code path that allows other applications or code on the phone to run arbitrary commands as a privileged user. 

Suggested Steps:

1. How do we execute arbitrary commands?
1. How could the app receive remote commands?
1. What would be a starting point for the classes?
1. Put it all together and how does it work?

### Solution

<iframe width="560" height="315" src="https://www.youtube.com/embed/WvJ8bDUCf4Y" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br/>
There is a vulnerability in that any application or component on the device can have an aribtrary command executed as the privileged system user through this application. The FotaProvider.apk sample exports the `WriteCommandReceiver` broadcast receiver through the action `android.intent.action.AdupsFota.operReceiver`. Any component on the device can send an intent with this action and with the String extra "cmd" and that "cmd" will be executed as the system user. On Android, the system UID is the most privileged UID behind root. 

This vulnerability/backdoor was first identified by Kryptowire in 2016. They give a detail explanation of this command execution issue as well as other identified security issues in the Adups OTA apps in their 2017 BlackHat USA presentation, "All your SMS and Contacts Belong to Adups & Others" \[[slides](https://www.blackhat.com/docs/us-17/wednesday/us-17-Johnson-All-Your-SMS-&-Contacts-Belong-To-Adups-&-Others.pdf)\] \[[video](https://www.youtube.com/watch?v=2AL5oKdiNrs&list=PLH15HpR5qRsUyGhBVRDKGrHyQC5G4jQyd&index=46&t=6s)\].

## Exercise 4 - Arbitrary Command Execution Take 2

In this exercise we will once again try and identify whether an application has a vulnerability that permits arbitrary command execution, but this time it is done in a different way. This will introduce us to how to reverse engineer apps that are using Binder, an Android inter-process communication mechanism.

For this exercise, you will use the app at `~/samples/HonSystemService.apk` in the VM. The SHA256 digest for the sample is 86a1ba436be2d1ee480e1d40418df3aa5f47571d154a8c41e74c439fbfb04b27.

### Goal

The goal of this exercise is to understand how to reverse engineer applications that are using Binder, specifically to look for vulnerabilities. 

### Exercise Context 

Let's use the same context as Exercise #3, but this time the solution will look different. This will show us another pattern and help us understand Binder. 

### Instructions
1. Start jadx as described in Exercise #1 and open HonSystemService.apk. 
1. Use the Manifest to identify interesting components and the bytecode to analyze the code.
1. Find a code path that allows other applications or code on the phone to run arbitrary commands as a privileged user.

### Solution

<iframe width="560" height="315" src="https://www.youtube.com/embed/CNkIX8OafF8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<br/>
If you're interested in more resources on analyzing and reverse-engineering pre-installed Android applications, you can check out my Blackhat USA 2019 talk, "Securing the System: A Deep Dive into Reversing Android Pre-Installed Apps" \[[slides](https://github.com/maddiestone/ConPresentations/raw/master/Blackhat2019.SecuringTheSystem.pdf)\]. This example is covered in Case Study #1.


[**NEXT** > 5. Reverse Engineering Android Apps - Native Libraries](reversing_native_libs.html)