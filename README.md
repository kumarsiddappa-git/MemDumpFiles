# MemDumpFiles
How to find the malicious or suspicious process and dump the files of a malware inside the memory 


Tools used 
1. Volatility
2. Windows OS
3. Python
4. FTK Imager

Install the volatility 
Download the latest Python from the url https://www.python.org/downloads/
Download the volatility from the https://github.com/volatilityfoundation/volatility3 , clone it and we can use vol.py

Once cloned , we need to download the plugins or symbol pack from the link
https://downloads.volatilityfoundation.org/volatility3/symbols/windows.zip and place the zip file in the volatility3/symbols folder

![image](https://github.com/user-attachments/assets/ca76e38f-934f-47c4-a501-4aabea542dee)


Have a file attached that the mem file which was capture using ftk imager
https://www.exterro.com/ftk-product-downloads/ftk-imager-4-7-3-81 

Lets validate if our volatility is working or not by using 
python.exe vol.py --help

![image](https://github.com/user-attachments/assets/60a654f6-441a-430b-8291-0005ee0563e9)

That means our environment is ready 

First lets understand what architecure or profile to use for furthe analysis

![image](https://github.com/user-attachments/assets/d08343f3-c4fa-4e2c-b04c-6486ed2997a7)

This Indicates its not 64 bit (Is64Bit = False) , PE Machine 332 which indicates its 32 Bit , NtProductYpe = NtProductWinNt which means its a Windows OS , NtMajorVersion is 6 and NtMinorVersion is 1, which corresponds to Windows 7 (specifically, Windows 7 SP1). and finally we have NTBuildLab=7601.17514.x86fre.win7sp1_rtm.10,which indicates its win 7 SP1.

Next lets start our Analysis in finding the process using windows.pslist - This will list all the Kernel Standard Process 

![image](https://github.com/user-attachments/assets/4dfdb574-bde9-4083-b6a7-747e19c66e61)

Did we find anything interesting in the process list , yes offcourse we could see the name @WanaDecryptor and its parent is or4qtckT.exe. How did we find it out , we used the PPID displayed beside the file @WanaDecryptor and compared to the PID of the other processes from the list and found the process or4qtckT.exe, but still we are not confirmed these are suspicious or malicious , so lets further continue our analysis by using other commands

We will now use the psscan which will list all the user and kernel process including the hiden process which was not visible, did we find out anything new here, if we closely look into the list of the process we can find out the parent of the taskdl.exe is also or4qtckT.exe

![image](https://github.com/user-attachments/assets/b7abff4e-a5ac-4dde-9b21-5f292dc8e0bb)


Now lets use pxsview which 

![image](https://github.com/user-attachments/assets/f8dcf5d2-10f3-4706-b1a0-e6903bd6b30c)

![image](https://github.com/user-attachments/assets/32662e35-bd93-402e-8cf7-6ce6fc65a49a)

We have four columns PSLIST , PSSCAN , THRDSCCAN and CSRSS , which gives more details into it 

PSLIST - Only Provides the list of process from the Kernel Standard Process table which is like executing the PS or TOP command in different Operating System.

PSSCAN - Is like list of process which are hidden or orphaned (Process without Parent), that may not be listed in the usual Process table, once strong contender for identify process like rootkit

THRDSCAN - Similiar to PSSCAN, instead it search for threads which are not attached to any process or hidden 

CSRSS (Client/ Server Runtime Subsystem) - Handling the user-mode process like console and creating new process , like client and server relationships

So we have to combine the results from each of the columns and try to idenfity the suspicious process. For example in our memory we can find 

![image](https://github.com/user-attachments/assets/1ff70c61-9890-48ad-a699-fef6043cf063) 

That PSLIST = FALSE and PSSCAN is True which means the process is hidden. so Consider that as a strong indicator.

We found that the taskdll is not listed in PSLIST, but found udner PSSCAN (which gives hidden process), WE can even try and find other process , here we are concentrating on ho to find the suspicious process. We can even consider other columns for example if PSLIST = FALSE and PSSCAN = False and THRDSCAN is TRUE , that is also a string indicator of Suspcious Process, so consider that as well.

Now lets see what is the parent process of the file and we found out that its 2732 which is the parent of multiple files, so lets try to find the file if present , and also cmdline for it. Since we have parent process , lets see what is inside it 

Lets execute the cmdline command and found the file is present in the Desktop, catching that lets see if we can dump, yes we can dump , using dumpFiles 

![image](https://github.com/user-attachments/assets/2083f110-ae92-42e7-9ed9-b2338b0a8a3b)

Now when we dump the file we get the files dumpped

![image](https://github.com/user-attachments/assets/c8ca601e-ab1d-4e5a-9d85-7b5da70313db)

Now use the same technique as mentioned in the my other GitHub to analyse Malware 

https://github.com/kumarsiddappa-git/Malware/blob/main/README.md












