# Introduction
Welcome to Android App Reverse Engineering 101! This workshop's goal is to give you the foundations to begin reverse engineering Android applications. While this workshop won't teach you the details of Android app development, Android malware analysis, Android vulnerability hunting, etc., I hope to give you all the necessary foundations through this workshop such that you can apply your new Android reversing skills to doing those things.

This workshop will be wholly based on reverse engineering through static analysis, or analyzing and understanding an application by examining its code. I won't be covering dynamic analysis where an analyst runs an application and understands the application by executing it, debugging it, etc. Why? Static analysis tends to be a less approachable skill for people to pick up on their own, so I want to help you do it! (And I really love static analysis)

## Environment

# Android Application Fundamentals

## Build an App
One of my biggest suggestions for folks looking to reverse engineer things, whatever they may be, is to try and build what you want to reverse. In the case of Android, you're lucky because there are so many free resources available to build your first application. If you've never built an Android application before, I suggest you start there. Pick any of the available tutorials and videos that spark your interest and get to building. When you understand *how* a developer builds something, it makes it much easier to understand how to reverse engineer it. 

## Fundamentals Review
Great! You've built an app or learned basic Android app development principles. Here is a review of some of the important points. This []"Application Fundamentals" page](https://developer.android.com/guide/components/fundamentals.html) in the Android developers' docs is a great review.

* Android applications are in the _APK file format_. APK is basically a ZIP file. (You can rename the file extension to .zip and use unzip to open and see its contents.)
* APK Contents (Not exhaustive)
    * AndroidManifest.xml
    * META-INF/
        * Certificate lives here!
    * classes.dex
        * Dalvik bytecode for application in the DEX file format. This is the Java (or Kotlin) code that the application will run by default.
	* lib/
        * Native libraries for the application, by default, live here! Under the lib/ directory, there are the cpu-specific directories. Ex: armeabi, mips, 
    * assets/
        * Any other files that may be needed by the app. 
        * Additional native libraries or DEX files may be included here. This can happen especially when malware authors want to try and "hide" additional code, native or Dalvik, by not including it in the default locations.

## Dalvik & Smali
Most Android applications are written in Java. Kotlin is also supported and interoperable with Java. For ease, for the rest of this workshop, when I refer to "Java", you can assume that I mean "Java or Kotlin". Instead of the Java code being run in Java Virtual Machine (JVM) like desktop applications, in Android, the Java is compiled to the _Dalvik Executable (DEX) bytecode_ format. For earlier versions of Android, the bytecode was translated by the Dalvik virtual machine. For more recent versions of Android, the Android Runtime (ART) is used.
</br>
If developers, write in Java and the code is compiled to DEX bytecode, to reverse engineer, we work the opposite direction. 
</br>
![Flowchart of Developer's process. Java to DEX bytecode](https://github.com/maddiestone/AndroidAppRE/images/DevelopersFlow.jpg)
</br>
![Flowchart of Reverse Engineer's process. DEX bytecode to SMALI to Decompiled Java](https://github.com/maddiestone/AndroidAppRE/images/ReversersFlow.jpg)

Smali is the human readable version of Dalvik bytecode. Technically, Smali and baksmali are the name of the tools (assembler and disassembler, respectively), but in Android, we often use the term "Smali" to refer to instructions. If you've done reverse engineering or computer architecture on compiled C/C++ code. SMALI is like the assembly language: between the higher level source code and the bytecode. 

For the following Hello World Java code:
```
public static void printHelloWorld() {
	System.out.println("Hello World")
}
```
The Smali code would look like:
```
.method public static printHelloWorld()V
	.registers 2
	sget-object v0, Ljava/lang/System;->out:Ljava/io/PrintStream;
	const-string v1, "Hello World"
	invoke-virtual {v0,v1}, Ljava/io/PrintStream;->println(Ljava/lang/String;)V
	return-void
.end method

```
The Smali instruction set is available [here](https://source.android.com/devices/tech/dalvik/dalvik-bytecode#instructions). 

Most often when reverse engineering Android applications, you will not need to work in Smali. Most applications can be lifted to an even higher level, decompiled Java. Like all tools, Java decompilers may have bugs. My suggestion to you is that whenever the decompiled Java output looks questionable, look at the Smali output. Work line by line with the instruction reference to figure out what the code is doing. 

To get the Smali from DEX, you can use the baksmali tool (disassembler) available at https://github.com/JesusFreke/smali/wiki. The smali tool will allow you to assemble smali back to DEX.

	
