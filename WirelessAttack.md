# Wireless Hacking
Wireless Hacking wil be more efficient if we know the attack flow of the target. In this project, the wireless hacking tools that will be using are Airgeddon and Evilgrade.

Airgeddon is written in bash and multi-use for Linux system to audit wireless networks and developed by V1s1t0rsh3r3. Evilgrade is a modular framework that allows the user to take advantage of poor upgrade implementations by injecting fake updates.

## Attacking Procedures
### 1. Reconnaissance and Scanning
![image](https://user-images.githubusercontent.com/86700132/131205508-b0bfc5ed-5072-44a6-a30d-887e94de6b7a.png)


From airgeddon main menu terminal window, the first step we need to do is capture some handshake. Choose option 5 and the script will move to handshake menu.


![image](https://user-images.githubusercontent.com/86700132/131205518-8c1e03c3-5ca0-4d6a-b13b-43b19cc2e925.png)


The next step we need to perform is to scan on all available wireless networks detected by wlan0mon and select the targeted network by choosing option 4. After capture some network, press CTRL+C to stop the scan.


![image](https://user-images.githubusercontent.com/86700132/131205534-6dcabe16-dd9f-4074-b612-9d5e13355277.png)

![image](https://user-images.githubusercontent.com/86700132/131205536-3da2d3f7-e9dc-4cef-8333-c5c3f8f2ac67.png)


Select the target network. In our case, we choose No. 17 which is Lenovo P70-A.


![image](https://user-images.githubusercontent.com/86700132/131205543-d85990f2-4e90-439f-8efa-7d2c41c48df8.png)


Next, choose option 5 capture the handshake. In our case, we are going to capture network for Lenovo P70-A.


![image](https://user-images.githubusercontent.com/86700132/131205549-dab1e1a0-5df7-48bf-83a4-db56ff51d68b.png)


In this step, select the deauth / disassoc amok mdk3 attack which is option 1 and wait around 20 seconds to forcefully disconnect a client from the access point and get the handshake.


![image](https://user-images.githubusercontent.com/86700132/131205552-b80bf9a8-3293-4ce5-b847-a341c67a6d74.png)

![image](https://user-images.githubusercontent.com/86700132/131205554-becfb657-db15-40dd-b672-b806615d141a.png)


The script will ask whether you get the handshake or not, insert y and press enter if you successful get the handshake which we will using it to perform wireless attack. Last, return to main menu and prepare for gaining access.


![image](https://user-images.githubusercontent.com/86700132/131205559-2d7a715e-f9e8-4728-b1d6-69032d9bea68.png)

