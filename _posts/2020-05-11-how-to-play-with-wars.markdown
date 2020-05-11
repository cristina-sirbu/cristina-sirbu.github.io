---
layout: post
title:  "How to play with WARs"
date:   2020-05-11 04:00:00
categories: java
---

I know we are in 2020 and in the era of cloud-native applications but not all of us have the luxury of playing with microservices and JAR files. There are people how have been assigned to take care of monoliths. And I must say that going from a small microservice back to a monolith is not an easy journey. These kinds of projects can be a bit intimidating I must say.

First question I had when I saw this huge monster is "How do I run it?". There was no documentation so I ended up in the pom.xml file. I noticed that the project is being packaged as a war so I ended up looking for “how to run a war file locally?”. Since I didn't found a complete guide to satisfy my needs I decided to write one. So next I will show you how to run a war file locally from IntelliJ to a Tomcat instance.

### 1. Web server

I downloaded Tomcat from [here](https://tomcat.apache.org/download-90.cgi).
I unzipped the file and copied the path where it will live.

### 2. Add Tomcat as an application server in IntelliJ

Go to IntelliJ's Preferences. Check for “Build, Execution and Deployment”, then “Application Servers”. Click on the "+" sign and choose “Tomcat server” and set the path to the Tomcat folder.

![Screenshot1](/assets/how-to-play-with-wars/Screenshot1.png)

### 3. Spring Boot Starter Tomcat dependency

In pom.xml make sure you have the following dependency:
```
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-tomcat</artifactId>
  <scope>provided</scope>
</dependency>
```

### 4. Set Artifacts

Go to Project Structure configuration. Check for “Artifacts” and click “+”. Add “Web Application: Exploded” from modules and set the module you want.

![Screenshot2](/assets/how-to-play-with-wars/Screenshot2.png)

### 5. Edit Run/Debug Configuration

Go to “Run/Debug Configurations". Click “+” and add “Tomcat Server: Local”. Set the JRE you want. It is important if you have multiple Java versions installed. Then, on the deployment tab, click “+” and add from artifacts the war you previously set. Set the application context you want. Then go down to “Before launch” section and double click on the “Build artifact’ item (usually the second item) and select the war you set at step 4.

![Screenshot3](/assets/how-to-play-with-wars/Screenshot3.png)

### 6. Click run

Voila! IntelliJ should build your WAR, start the Tomcat instance and deploy the WAR into it.

![Screenshot4](/assets/how-to-play-with-wars/Screenshot4.png)

So, as advice for people that will end up having to play with this kind of projects: Keep calm and don't freak out! A monolith is just another project that is screaming for attention and love. And now that you can run it from your computer you can start cleaning it up and breaking it down into manageable microservices! Good luck!
