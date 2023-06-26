---
title: "How To Install Oracle WebLogic Server In Eclipse/STS"
weight: 3
date: 2023-01-23T20:37:21+05:00
draft: false
images: []
resources:
- name: "featured-image"
  src: "featured-image.png"
lightgallery: true
tags: ["eclipse", "weblogic server","plugin"]
categories: ["Technology"]

---
If you try to install Oracle Weblogic on Eclipse/STS 2020 or any latest version of Eclipse/STS. You will not be able to install Weblogic Servers in it. You will get errors like.


This happens when Eclipse is using OpenJDK instead of oracle.
There are two ways to set Java JRE/JDK for eclipse usage. Either we configure it as an environmental variable or manually provide the path to oracle JDK in INI file. In this article, we are using the second way.

Here are steps to install oracle weblogic.

#### Steps:
Download Oracle JDK 11/latest
Install Oracle JDK
Download Eclipse/STS
Install Eclipse/STS
Download Weblogic
Install WebLogic
Edit eclipse.ini / SpringToolSuite4.ini file and change VM from OpenJDK to Oracle JDK
Original eclipse.ini/ SpringToolSuite4.ini file


Updated eclipse.ini/ SpringToolSuite4.ini file


Note : remember to use forward slash(/) for file path instead of using backward slash

8. Un install JustJ OpenJDK

a. go to help-> About Eclipse IDE -> Installation Details -> Installed Software
b. search for Justj JDK and uninstall it

9. Window -> show view -> Servers

10. Add Oracle Repository in Eclipse

In eclipse, Go to Help -> Install New Software..
Click Add to add a new update site. Enter the “Name” Oracle and enter the “Location” http://download.oracle.com/otn_software/oepe/12.2.1.10/photon/repository/ . Then click “Add”.
Select Tools then weblogic tools as shown in images




11. Install new Server , Now there you can find Oracle Weblogic Server


