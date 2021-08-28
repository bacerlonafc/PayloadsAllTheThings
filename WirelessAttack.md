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


