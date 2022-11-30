# Lian_Yu---TryHackme-Writeup-Walkthrough

Room link : https://tryhackme.com/room/lianyu

# 1. IP SCANNING

First we will open the terminal, after opening the terminal, we will use the root kali. Then we will use VPN in root kali.

Now, We will use NMAP.

<img width="955" alt="Screenshot 2022-11-30 190141" src="https://user-images.githubusercontent.com/109613277/204809561-5b574acc-84ac-4ea7-813f-6db11e34af2f.png">

Here we can see that four PORTS are open.

port 21/tcp - FTP - (vsftpd 3.0.2)
port 22/tcp - SSH - (OpenSSH 6.7p1)
port 80/tcp - HTTP - (Apache httpd)
port 111/tcp - RPC - (rpcbind) 

# 2. ENUMERATION

Now, We will go to the browser and search the IP. <your IP>

<img width="952" alt="Screenshot_20221130_191247" src="https://user-images.githubusercontent.com/109613277/204811897-3ccaf4a6-b7e3-477a-b921-a451647f570f.png">

Now, We will go to terminal and use gobuster to see the hidden directories.

gobuster dir -u http://10.10.156.228/ -w/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt (You will provide your own IP address)

<img width="962" alt="image" src="https://user-images.githubusercontent.com/109613277/204814850-bec86b28-ca7f-4b7d-8fef-71aa9b939ec7.png">

Yes, We have found the hidden directory : /island 

Now, We will go to the next step.

Now, We will go to the browser again and search http://10.10.156.228/island/


We will find the code by highlighting the page or view page source. Code - 'vigilante' - ( Must remember this is our FTP username)

Via Page Highlight.

<img width="962" alt="image" src="https://user-images.githubusercontent.com/109613277/204819661-b3835298-2336-482d-bf15-b35efd70a580.png">

Via View Page source.

<img width="957" alt="image" src="https://user-images.githubusercontent.com/109613277/204819894-03f48920-7f90-4bda-ac8d-6ecd62e8d12d.png">

We will again run gobuster on /island to get another directory.

gobuster dir -u http://10.10.156.228/island -w/usr/share/wordlists/dirbuster/directory-list-lowercase-2.3-medium.txt

<img width="612" alt="Screenshot_20221130_195403" src="https://user-images.githubusercontent.com/109613277/204837604-7c6eba72-9412-4324-811a-17bcec81ce44.png">

Here again we found a separate directory. : /2100

Now, We will go to the browser again and Search http://10.10.228.22/island/2100

<img width="958" alt="Screenshot_20221130_195603" src="https://user-images.githubusercontent.com/109613277/204840376-1c02fc5c-5c1c-44cd-a260-b0b36f77a375.png">

After search in browser, this page will open. 

Now, We will go back to the view page source again.

<img width="960" alt="11" src="https://user-images.githubusercontent.com/109613277/204842073-6057d544-c41c-4d46-b0ef-ea0e14134ac8.png">

Here it shows a file with '.ticket' extension.

Now, We will run gobuster o again to see the file with .ticket extension file

gobuster dir --url 10.10.156.228/island/2100 --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x .ticket 

<img width="963" alt="12" src="https://user-images.githubusercontent.com/109613277/204844621-29b81c3b-eb25-4723-b187-f2018adbc25c.png">

We found a directory again : /green_arrow.ticket

We will go back to the browser and search http://10.10.156.229/island/2100/green_arrow.ticket

<img width="963" alt="13" src="https://user-images.githubusercontent.com/109613277/204845715-53db855d-90ed-4293-af80-747ab3e9d76f.png">

We will find this encode and decode it.

We will go to this site to decode - https://www.browserling.com/tools/base58-decode

<img width="960" alt="15" src="https://user-images.githubusercontent.com/109613277/204846942-bb95ac32-2a37-4f2c-a3de-d7b40a41a96f.png">

After we decoded it, we got - '!#th3h00d' (Must remember this is our FTP password)

# 3. FTP Login

Now, We have FTP Username and Password.

Username - 'vigilante'

Password - '!#th3h00d'

Now, We can login to FTP service.

<img width="960" alt="16" src="https://user-images.githubusercontent.com/109613277/204849262-36f1d4e2-f561-49b4-b7d9-f98ee7f8a263.png">

![Screenshot (85)](https://user-images.githubusercontent.com/109613277/204849752-6fd86bbd-2712-46c6-b97f-ea0becf25845.png)

Here, We got two users - 'vigilante' and 'slade' and also found three image files.

Now, We will go to the pictures file and see the images.

![Screenshot (86)](https://user-images.githubusercontent.com/109613277/204851239-fd68f186-5d1c-4ee9-a3ef-111935ffd5ef.png)

Here the other two images opening but 'Leave.me.alone.png' isn't opening.

![Screenshot (87)](https://user-images.githubusercontent.com/109613277/204852547-e0abdd08-41a5-4e3c-9ec3-74a412f9a856.png)

exiftool showing 'File format error'.

So, What do we do now? Let's see the header file of the image once.

![Screenshot (88)](https://user-images.githubusercontent.com/109613277/204854035-dbd677d5-28e5-430b-93e3-adb874ec2d53.png)

Oho! Here the header file of the image is wrong.

We will get the correct header from here - https://en.wikipedia.org/wiki/Portable_Network_Graphics

<img width="960" alt="Screenshot_20221130_203016" src="https://user-images.githubusercontent.com/109613277/204855043-f95938ba-67af-400e-bc1a-dc056cd1497f.png">

Now, Let's change it.























