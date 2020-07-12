---
layout: post
title:  "How to install Java"
date:   2020-07-10 15:41:14 +0300
categories: jekyll update
---

There are moments when the client want to have custom version of Java installed, but 
your Ubuntu repository does not support this version. Here is a solution for this problem.

### Install Oracle Java step-by-step guide for Ubuntu
#### Step1:  
Download the desired jdk(jdk-11.0.5 for example)

#### Step2:
Open a terminal (Ctrl + Alt + T) and enter the following command.  
``` console
 sudo mkdir /usr/lib/jvm
```  
If the /usr/lib/jvm folder does not exist, this command will create the directory. If you already have this folder, you can ignore this step and move to the next step.  
#### Step3:  
Enter the following command to change directory.  
``` console
cd /usr/lib/jvm
```  
#### Step4:  
Extract the jdk-Xuxx-linux-xXX.tar.gz file in that directory using this command.  
```console
sudo tar -xvzf ~/Downloads/jdk-11.0.5_linux-x64_bin.tar.gz
```  
According to this command, the JDK filename is jdk-11.0.7_linux-x64_bin.tar.gz and which is located in the ~/Downloads folder. If your downloaded file is in any other location, change the command according to your path.  
#### Step5:  
Enter the following command to open the environment variables file.  
``` console
sudo nano /etc/environment
```  
According to your personal preference, you can choose any text editors instead of nano.
#### Step6:  
In the opened file, add the following bin folder to the existing PATH variable.  
``` console
/usr/lib/jvm/jdk-11.0.7/bin
```  
The PATH variables must be separated by colon.  
Add the following environment variables at the end of the file.  
``` console 
JAVA_HOME="/usr/lib/jvm/jdk-11.0.5"
```  
The environment file before the modification:  
``` console
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
```  
The environment file after the modification:  
``` console
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk-11.0.5/bin"
JAVA_HOME="/usr/lib/jvm/jdk-11.0.5"
```  
Save the changes and close nano (Ctrl + O, Ctrl + X).  
#### Step7:  
Enter the following commands to inform the system about the Java's location. Depending on your JDK version, the paths can be different.  
``` console
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk-11.0.5/bin/java" 0
```  
``` console
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk-11.0.5/bin/javac" 0
```  
``` console
sudo update-alternatives --set java /usr/lib/jvm/jdk-11.0.5/bin/java
```  
``` console 
sudo update-alternatives --set javac /usr/lib/jvm/jdk-11.0.5/bin/javac
```  
#### Step8:  
To verify the setup enter the following commands and make sure that they print the location of java and javac as you have provided in the previous step.  
``` console
update-alternatives --list java
```  
``` console
update-alternatives --list javac
```  
#### Step9:  
Restart the computer (or just log-out and login) and open the terminal again.  
#### Step10:  
Enter the following command.
``` console
java -version
```  
If you get the installed Java version as the output, you have successfully installed the Oracle JDK in your system.