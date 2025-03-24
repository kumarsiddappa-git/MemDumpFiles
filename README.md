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

so now lets start with the lisitng the process using windows.pslist







