---
layout: post
title: Gradle does not launch in IntelliJ
---

This happened to me several times that when I start my Gradle project with IntelliJ, update my gradle file and run the Gradle refresh, the Gradle update doesn't run citing some issue with allocating heap memory to the JVM and hence the JVM couldn't start.

After scouting stack overflow, I figured out that it has to do with Java version. At that time, I had Java 8, which was causing the problem. I un-installed Java 8 and IntelliJ started using Java 7 and everything worked fine.

After a few weeks, when I started the project, I am faced with the same error again. This time, I didn't have Java 8 on my machine. To make sure, I opened the IntelliJ terminal and ran the command

`java -version`

As suspected, the JVM couldn't start. Some stack overflow posts suggested updating registry, which I tried, but couldn't get past the issue. Finally, I ran the Java 7 installer once again and restarted IntelliJ. This finally fixed the issue.

In the times where we have less and less time for working on projects, issues like these are a buzz kill, throwing us away from the main work. Especially, if we encounter an issue that we solved several weeks ago, but some how forgot the way we fixed it.
