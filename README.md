# Auto-IP-Rotation-with-Tor-TorNet-on-Kali-Linux
#### 1.TOR

Installing Tor plays a central role in changing your IP address by routing your internet traffic through a global network of volunteer-operated servers called Tor relays. Here's how it works and why it's essential:

## 🌐 What Tor Does for IP Obfuscation
IP Masking: When you connect to the internet through Tor, your real IP address is hidden. Instead, websites see the IP of the Tor exit node, which changes periodically.

Multi-layered Encryption: Tor encrypts your traffic in layers (like an onion), making it extremely difficult to trace back to you.

Circuit Switching: Tor builds a new path (or "circuit") through different relays every 10 minutes by default, which results in a new IP address being assigned.

## 🧩 Why You Need to Install Tor
Without installing Tor, tools like TorNet, ProxyChains, or Tor IP Changer won’t work—they rely on the Tor service running in the background.

Tor provides the SOCKS5 proxy at 127.0.0.1:9050, which is what these tools use to route and rotate your traffic.

It enables commands like SIGNAL NEWNYM, which tells Tor to generate a new identity (i.e., a new IP address).
### 🚀 Commands Used

# Update system packages
'''bash 
sudo apt update
'''

# Install Tor service (background daemon)
'''bash
sudo apt install tor
'''

# for changing ip you have to set some settings in fire fox or any brouser that you use in network proxy
![Screenshot 2025-06-27 175316](https://github.com/user-attachments/assets/99ab6e39-c61f-4ee3-8382-6e5b49f4409d)
![Screenshot 2025-06-27 175323](https://github.com/user-attachments/assets/3c42369a-a3f0-40ce-8b25-2158c31e486e)
![Screenshot 2025-06-27 175359](https://github.com/user-attachments/assets/c687b61a-546b-479b-8ce6-97803b770fef)
![Screenshot 2025-06-27 175437](https://github.com/user-attachments/assets/19b81263-8fc5-4b9e-b15c-e606ddee0f75)

# activate tor by
'''bash
sudo systemctl start tor
'''

# check status of tor is active or not
'''bash
sudo systectl status tor
'''

#### 2.TORNET
Installing TorNet is what transforms the Tor network from a passive anonymity layer into an active IP rotation engine. Here's how it fits into the picture:

## 🔄 What TorNet Actually Does
TorNet is a Python-based automation tool that:

Connects to the Tor service running on your system

Sends a SIGNAL NEWNYM command to Tor, which tells it to build a new circuit (i.e., change your exit node)

Rotates your IP address at a user-defined interval (e.g., every 10 seconds)

Provides a command-line interface to control and monitor the process

## 🧩 Why You Need to Install TorNet
While Tor itself anonymizes your traffic, it doesn’t automatically change your IP on a schedule. That’s where TorNet comes in:

## Feature	Tor	TorNet
Routes traffic anonymously	✅	✅
Changes IP manually	⚠️ (requires command)	✅ (automated)
Scheduled IP rotation	❌	✅
CLI for automation	❌	✅
Useful for scripting & testing	❌	✅
🛠️ How It Works Together
Tor provides the anonymizing network and SOCKS5 proxy.

TorNet acts as a controller that:

Talks to the Tor control port

Issues commands to rotate circuits

Monitors and displays your current IP

Automates the entire process

### 🚀 Commands Used
'''bash
sudo pip install tornet
'''
![Screenshot 2025-06-27 175627](https://github.com/user-attachments/assets/36650aa3-965b-48d7-a4c4-6df43594bc4d)
##problem 1 dpkg: error processing package python3 (--configure):
##solution:
![Screenshot 2025-06-27 184157](https://github.com/user-attachments/assets/5ca0e502-5511-4271-9e3d-a0d14cce9794)

the install python
'''bash
sudo apt install python3 
,,,

## 🧩 Why Python3 Is Required
TorNet is written in Python — it uses Python scripts to control the Tor service and rotate IPs.

It’s distributed as a Python package, so you need Python3 to install and run it.

The pip or pipx tools (which install Python packages) require Python3 to function.




## promblem 2: externally managed environment
Ah, that “externally-managed-environment” message is a common roadblock when using pip on newer versions of Kali or Debian-based systems. It’s not a TorNet-specific issue—it’s Python’s way of saying: “Hey, your system manages Python packages through APT, not pip, so I’m blocking this to avoid breaking things.”

##🧩 Why This Happens
Your system is protecting itself from potential conflicts between:

System-managed packages (via apt)

User-installed packages (via pip)

This protection is defined in PEP 668, and it’s triggered when you try to install Python packages globally using pip.
##Solution :Use a Virtual Environment

'''bash
python3 virtualenv
'''
A Python virtual environment is like a personal sandbox where you can install packages and run Python code without affecting your system-wide Python setup. It’s especially useful in cybersecurity projects like yours, where different tools might require different versions of libraries.

###🧱 What Is a Virtual Environment?
A virtual environment is an isolated directory that contains:

Its own Python interpreter

Its own pip (Python package installer)

A separate folder for installed packages

This means:

You can install tornet in one project without affecting another.

You avoid conflicts between tools that need different versions of the same library.

You don’t mess up your system Python by accident.

'''bash
virtualenv <NAMEOFENV>
'''
![Screenshot 2025-06-27 184504](https://github.com/user-attachments/assets/58c680ce-a361-4e97-a963-be2bb017a432)

##🧩 Why Use a Virtual Environment for RouterSploit?
RouterSploit is a Python-based exploitation framework, and like many Python tools, it has specific dependencies (like requests, paramiko, pycrypto, etc.). These dependencies might:

Conflict with other tools on your system

Require different versions than what’s globally installed

Break system tools if installed globally with sudo pip install

That’s where a virtual environment comes in — it creates a safe, isolated space just for RouterSploit.
![Screenshot 2025-06-27 185037](https://github.com/user-attachments/assets/7c7c7c56-a1c2-4511-9e67-fb21f91fa4ff)

##go inside the file named routersploit then install requirements.txt
'''bash
![Screenshot 2025-06-27 185155](https://github.com/user-attachments/assets/fe325bb1-ef1a-406b-bfe3-6faeca86d189)
![Screenshot 2025-06-27 185457](https://github.com/user-attachments/assets/65d187e9-91bd-4c72-93e0-94b55ef3c27e)
![Screenshot 2025-06-27 185549](https://github.com/user-attachments/assets/6e46df60-db4f-4b90-ade1-b54167e6a9ac)

'''bash
python3 -m pip install tornet --break-system-packages
'''

python3 -m pip: Runs pip using the system’s Python interpreter (safer and more explicit than just pip).

install tornet: Installs the TorNet package from PyPI.

--break-system-packages: Overrides the system’s protection that prevents global pip installs.

##🔐 Why This Flag Is Needed
Modern Linux distros (like Kali, Ubuntu 23+, Debian 12+) follow PEP 668, which:

Blocks global pip installs to avoid breaking system-managed Python packages.

Shows the error: error: externally-managed-environment.

Using --break-system-packages tells pip: > “Yes, I know this might mess with system packages — do it anyway.”

##install tornet
'''bash
python3 pip install tornet
''bash

##hurray ! tornet is downloaded in kali device for check
'''bash 
sudo tornet --interval 3 --count 0
'''
##🔄 What It Does
This command tells TorNet to:
Connect to the Tor control port.Send a SIGNAL NEWNYM every 3 seconds.Rotate your IP address continuously without stopping becaus of zero .You can put number instead of zero it tell how many ip you have to change

##to check ip go for dnsleaktest









