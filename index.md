<link rel="icon" href="images/favicon.ico" type="image/x-icon">

# 1. Introduction
Welcome to Android<sup>TM</sup> App Reverse Engineering 101! This workshop's goal is to give you the foundations to begin reverse engineering Android applications. While this workshop won't teach you the details of Android app development, Android malware analysis, Android vulnerability hunting, etc., I hope to give you all the necessary foundations through this workshop such that you can apply your new Android reversing skills to doing those things.

This workshop will be wholly based on reverse engineering through static analysis, or analyzing and understanding an application by examining its code. I won't be covering dynamic analysis where an analyst runs an application and understands the application by executing it, debugging it, etc. Why? Static analysis tends to be a less approachable skill for people to pick up on their own, so I want to help you do it! (And I really love static analysis)

## Environment
All of the exercises in this workshop can be done in the virtual machine (VM) that is available here. 
* [Virtual Box VM](https://drive.google.com/file/d/1BB6HUxra1Rf2iiEsKcdzUfNiajYvgDEQ/view?usp=sharing)
* [.ova for VMWare](https://drive.google.com/file/d/1Og-mHwpIxX-ghxwJf_g7bS7Vt1gU9gsm/view?usp=sharing)

The VM is an Ubuntu 18.04 64-bit image. The username is `AndroidAppRE` and the password is `android`. The VM has the following tools installed:

* [jadx](https://github.com/skylot/jadx) - Android decompiler. We load the APKs into jadx. And are able to analyze the DEX bytecode using jadx.
* [Ghidra](https://ghidra-sre.org/) - Software reverse engineering tool. We use its ARM disassembler/decompiler functionality in the exercises to statically analyze the native libraries. 

## Table of Contents

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

## Acknowledgements

This workshop was modeled after the format used by Amanda Rousseau's ([@malwareunicorn](https://twitter.com/malwareunicorn)) RE 101 workshop which she released using GitHub pages. Thanks, Amanda, for the inspiration!

_"The Android robot used as the logo is reproduced or modified from work created and shared by Google and used according to terms described in the Creative Commons 3.0 Attribution License."_

_Android is a trademark of Google LLC._

[**NEXT** > 2. Android Application Fundamentals](app_fundamentals.html)

