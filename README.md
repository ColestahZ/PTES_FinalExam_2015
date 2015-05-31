# PTES_FinalExam_2015

#PTES FinalExamination 2015#


    MS 14 9607 96 Nishani E.D.S.

##Question 01##
    
    A. 
**The Date:** 28th of August 2013

    B. 
**Source:** United States     
**Destination:** China          
**How long the attack has been occurring:** 7 days     
**How has the attack been pulled off:**

China hit by DDoS attack. The CINIC confirmed that the country suffered a DDoS attack over the weekend causing the Internet inaccessibility for hours. During the weekend China’s Internet was taken down by a powerful DDoS attack, according to security experts behind the offensive there is a group of skilled hackers that on Sunday hit the country networks. But security expert clarified that China could have been perpetrated by sophisticated hackers or by a single individual. The China Internet Network Information Center [CINIC] reported that the attack began at 02:00 local time on Sunday with a peek at 04:00 that made it the largest DDoS attack the country’s networks have ever faced. CloudFlare security service provider revealed to have observed a 32 per cent drop in traffic for thousands of “.cn” websites compared with the same period in the preceding 24 hours. The China Internet Network Information Center confirmed the attack with an official statement informing internet users that it is gradually restoring web services and that will operate to improve the security level of the Internet infrastructure of the country to prevent and mitigate further attacks. 

The Wall Street Journal was the first media agency that reported the important outage, the official source of Chinese Government confirmed that its network suffered the biggest distributed denial-of-service attack ever. All the “.cn” websites went down four hours.

At the moment there is no news on the origin of the attack and on its motivations, CloudFlare CEO Matthew Prince said that there is no certainty that behind the attack there is a group of hackers, he added that “it may have well been a single individual“. 
Prince’s affirmation is reinforced by the possibility to retrieve on the underground market a huge quantity of DIY DDoS hacking tools that could allow the arrangement of a DDoS attack, it must be considered also the possibility to rent a botnet to hit a specific target for a limited period, both options accessible practically also to single individuals and small gangs. Ironically, China is considered by analysts as the principal origin for this type of attack as revealed by many reports: such as the one published by Akamai security firm (“State of The Internet” report).

![capture 1](https://cloud.githubusercontent.com/assets/12673434/7900511/98870ac2-077a-11e5-8d90-063baec6aa8b.png)

![capture 2](https://cloud.githubusercontent.com/assets/12673434/7900517/d8ce08f6-077a-11e5-873d-fa72eaa2614c.jpg)

    C. 
**Which country(s) seemed to be generating the most botnet attacks**   
   Answer: China

![capture 3](https://cloud.githubusercontent.com/assets/12673434/7900521/2fb4d2f8-077b-11e5-8e7d-601643d932f6.jpg)

##Question 02##

###Wargames: Leviathan###

####Leviathan – Level 0####

Leviathan’s levels are called leviathan0, leviathan1, etc. and can be accessed on leviathan.labs.overthewire.org through SSH. Data for the levels can be found in the homedirectories.
To login to the first level use:
    
    Username: leviathan0
    Passowrd: leviathan0

![capture 5](https://cloud.githubusercontent.com/assets/12673434/7900535/b1399510-077c-11e5-8a4b-79fbeaca8958.JPG)

![capture 6](https://cloud.githubusercontent.com/assets/12673434/7900539/e1a6d640-077c-11e5-8774-e16e36b49f18.JPG)

![capture 7](https://cloud.githubusercontent.com/assets/12673434/7900551/00117bc6-077d-11e5-874c-b339bdff8bfc.JPG)

    Username: leviathan1
    Password: rioGegei8m

![capture 8](https://cloud.githubusercontent.com/assets/12673434/7900554/25cbeb94-077d-11e5-84d9-2f1087ab73fa.JPG)

####Leviathan – Level 1####

Leviathan 1 presents us with a binary in our home directory. If we run it we see that is a utility that ask and checks for a password. Knowing this, the binary is either reading the correct password from somewhere else or has it stored within the binary. In this case, the password is stored within the binary. We need to trace the binary. Many of you might catch the small hint to the movie *Hackers* within the binary. When Plague states the **4 most common passwords are love, sex, secret, and god.**

    Username: leviathan2
    Password: ougahZi8Ta

![capture 9](https://cloud.githubusercontent.com/assets/12673434/7900558/8d8f7e80-077d-11e5-869f-3e452b651e12.JPG)

To see what functions it is calling let’s use “ltrace”. We can see it is using strcmp() to check if an entered password is the same as it's stored string, "sex". Running the binary again we can now try entering "sex" as the password to test what will happen.

![capture 10](https://cloud.githubusercontent.com/assets/12673434/7900561/b27178d4-077d-11e5-906a-cb9e660035ad.JPG)

####Leviathan – Level 2####

Leviathan2 presents us with a small binary that belongs to the **user leviathan3 and group leviathan2**. The program contains a small security hole that can be exploited using a symbolic link.  To understand how the program functions at its core and what is happening behind the scenes when the program executes, we will use a few Linux commands and techniques to enlighten us with this information.

    Username: leviathan3
    Password: Ahdiemoo1j

When you run "printfile" we can see that it wants a file path argument. We are going to go ahead and create a directory and a file with a space in the name.

![capture 11](https://cloud.githubusercontent.com/assets/12673434/7900567/f591d8a2-077d-11e5-95ac-9eb4ac298ec4.JPG)

![capture 12](https://cloud.githubusercontent.com/assets/12673434/7900569/0d140f7c-077e-11e5-8e0c-efb5ace7746f.JPG)

So, what is happening here is a small security hole in how this program functions. We can see that the function access() and /bin/cat are being called on the file. What access() does is check permissions based on the process’ real user ID rather than the effective user ID.

While access does use the full file path, the cat on the file is not using the full file path. We can see this near the end of the output where /bin/cat/ is being called to tmp.txt as if it were a separate file, it’s really the second half of our filename. This can be exploited if we create a symbolic link from the password file to the file we created in /tmp.

Let’s create the symbolic link, but with only the first part of the filename we created before the space. Issue the command ls –al; we see file tmp.txt was not converted to a symlink, but a separate symlink called "file" was created.

![capture 13](https://cloud.githubusercontent.com/assets/12673434/7900577/339635d0-077e-11e5-886b-b31fda13d38a.JPG)

Calling ~/printfile on the file that actually links to the password will fail. The file did actually access the symbolic link correctly up until file, but then tried to incorrectly cat the part of the file name after the space as if it were another file.

This all works because /printfile is owned by leviathan3. Access will call the symlink with that privilege. But we also needed to utilize a syntax hack to make it work, hence the filename with a space in it.

![capture 14](https://cloud.githubusercontent.com/assets/12673434/7900580/5f192636-077e-11e5-9624-f84382a1cef4.JPG)

![capture 15](https://cloud.githubusercontent.com/assets/12673434/7900582/77fcefc0-077e-11e5-922f-c95ae0fa6cf7.JPG)

####Leviathan – Level 3####

For Leviathan 3, the process is fairly simple.  We only need a few commands that we are already familiar with to get access to Leviathan4’s shell. When we finally do get his shell, we can cat his password in /etc/leviathan_pass/leviathan4 and correctly log in as him to begin level 4->5. 
    
    Username: leviathan4
    Password: vuH0coox6m

![capture 16](https://cloud.githubusercontent.com/assets/12673434/7900585/9fa32c42-077e-11e5-8776-740bfdb42a58.JPG)

![capture 17](https://cloud.githubusercontent.com/assets/12673434/7900586/b1d97b00-077e-11e5-8391-9e0b87b577af.JPG)

The point of interest for us will be the **snlprintf** word. Right after that you see the words, "You've got shell!” Let's use that as the password.

![capture 18](https://cloud.githubusercontent.com/assets/12673434/7900589/deffdd68-077e-11e5-9236-3746f5e45ad9.JPG)

All are familiar with snprintf() in C, but not snlprintf(). So we can assume it is a made up function or actually is the password. Clearly it is a visible string in the binary. If you follow the logic in the strings output, you can crudely tell this is what pops the shell in main.

![capture 19](https://cloud.githubusercontent.com/assets/12673434/7900592/fda920da-077e-11e5-96a2-9c063ab22200.JPG)

####Leviathan – Level 4####
    
    Username: leviathan5
    Password: Tith4cokei

![capture 20](https://cloud.githubusercontent.com/assets/12673434/7900594/26330c82-077f-11e5-8500-f711b861b8df.JPG)

Covert binary values to ASCII and obtain the password. 

![capture 22](https://cloud.githubusercontent.com/assets/12673434/7900596/5146deda-077f-11e5-87b1-6fd61ec64a66.JPG)

####Leviathan – Level 5####

    Username: leviathan6
    Password: UgaoFee4li

In Leviathan 6, we will exploit a binary by using symbolic linking to a file we have permission of. When we run the binary in Leviathan5’s home directory, it appears to be attempting to read from a file in /tmp. The binary is owned by Leviathan6 but belongs to the Leviathan5‘s group. Therefore, it can pull Leviathan6’s password. We successfully exploited bad permissions placement on files via symlinking and now have Leviathan6’s pass as a result.

![capture 23](https://cloud.githubusercontent.com/assets/12673434/7900600/8a5173a2-077f-11e5-8fec-3543e3e174a5.JPG)

![capture 24](https://cloud.githubusercontent.com/assets/12673434/7900602/9ecd2c18-077f-11e5-9c40-6bdcf78388e5.JPG)

####Leviathan – Level 6####

    Username: leviathan7
    Password: ahy7MaeBo9

The final stage of Leviathan presents us with a problem that can be solved via some quick bash scripting. The binary in our home directory accepts a 4 digit combination as the password. Iterating through all the possible combination manually would take too much time. So we create a bash script to do this in a matter of seconds.

![capture 25](https://cloud.githubusercontent.com/assets/12673434/7900606/d895376a-077f-11e5-88bd-06bf5a9d82bb.JPG)

![capture 26](https://cloud.githubusercontent.com/assets/12673434/7900609/ee753a44-077f-11e5-8499-e96cf8f289b2.JPG)

![capture 27](https://cloud.githubusercontent.com/assets/12673434/7900610/0b19a1b2-0780-11e5-87fa-0a3acc2136e0.JPG)

Run the binary and simply wait for it to iterate through all possible number combinations. Here the script will simply input the pass to the binary and pop us into leviathan7’s shell when it finds a match.

![capture 28](https://cloud.githubusercontent.com/assets/12673434/7900611/4001bdf6-0780-11e5-9ac4-7f12f01cc0c9.JPG)

![capture 29](https://cloud.githubusercontent.com/assets/12673434/7900613/628a8650-0780-11e5-8384-ffcbbb8bb6b9.JPG)

####Leviathan – Level 7####

![capture 30](https://cloud.githubusercontent.com/assets/12673434/7900615/7e9ac6fc-0780-11e5-8449-42c7785a2726.JPG)

##Question 03##


    A.
**/etc/passwd file:** Text file that contains the basic information about each user or account running a linux Operating System.         

**/etc/shadow file:** Stores actual password in encrypted format for user's account 
   
**setuid bit:** Unix access right flag that allow users to run an executable with the permission of the owner or group respectively and change the behavior in  its directories.

**chroot:** An operation in linux that changes the apparent root directory for the current running process and its children.
    
    B.
**lsattr** command lists the file attributes on a second extended file system whereas **ls -l** shows the long listing information about the file or directory.
    
    C.
Android system implements the **principle of least privilege**. This means that each app, by default, has access to only the components that it requires to do its work and no more. The granularity of control that the OS has over privileges of an individual process is limited.

In practice as to explain the general problem; it is very difficult to  control a process's access to memory, processing time, I/O device addresses or modes with the precision needed to facilitate only the precise set of privileges a process will require. The problem is once the rights are given the user has no way of modifying them again.

    D.
Access control lists provides an additional, more flexible permission mechanism for file systems. ACL allows you to give permissions for any user or group to any resource while implementing more control over it. Yet it is designed to assist with unix file permissions.

In standard unix permission model, the ownership of the files is one of the crucial component. Access to the files and read, write, execute permission depends on the permissions given by the owner or the group.
    
    E.
**ruid** is used to identify who the user is actually. once it is setup by the system, it cannot be changed till your session terminates. Users cannot change the ruid. only the root can change it.

**euid** is used to determine what level of access the current process has. when the euid is zero then the process has unlimited access.

    F.
**Method 1:** In a unix system, *Syslog* file provides information about the activities of the system, such as user activities and history.If the attacker clears this file, the illegal activities cannot be discovered or he can simply cover his traces.

**Method 2:** Altering accounting files in unix is another method that could be useful of covering tracks. utmp, wtmp and lastlog files are the main accounting files in unix. 

**Method 3:** clear log files like messageslog,xferlog,utmp,wtmp ect. Most off the following files can be only accssed by root,for clearing them root access is needed.

**Method 4:** Clear bash history for the account; once the attack is done we need to make sure we clear all our command history before leaving ,since this holds all our tracks on the attack. 

##Question 04##

    mov DWORD PTR [ebp-0x4], 0x8

Moves the memory address 0x8 into the location pointed to by EBP (Existing Base Pointer) holds the base address of the stack; like pushing a value onto the stack so we can work with it. DWORD is 32-bits in size.

    mov eax, DWORD PTR [ebp+0x8]

Moves value at epb+0x8 into EAX which is a general 32-bit register.

    lea eax, [ecx + eax*1]

The address of the quantity ecx+eax*1 is placed in EAX; ECX and EAX all the three are general 32-bit registers.

    call _htons

Call _htons function. Call puts the address of the next instruction on the stack so the program can return to it.

    cmp [ebp+0x8], 0

Compare the values of the two operands ebp+0x8 and 0. 


##Question 05##

![capture 31](https://cloud.githubusercontent.com/assets/12673434/7901012/b6cfd740-0791-11e5-978d-c33f803974c5.JPG)

![capture 32](https://cloud.githubusercontent.com/assets/12673434/7901016/c8a0c498-0791-11e5-8e1b-c793fe8558e2.JPG)

![capture 33](https://cloud.githubusercontent.com/assets/12673434/7901018/da15a6f8-0791-11e5-8aab-d4aec8d7f4de.JPG)

