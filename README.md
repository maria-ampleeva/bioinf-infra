## 2 - Working with remote servers


git branch name: jbrowser

**Theory [2]

### [0.4] What are computer ports on a high level? How many ports are there on a typical computer?**

A port on high level is a virtual point where network connections start and end. Ports are software-based and managed by a computer's operating system. Each port is associated with a specific process or service. 

The port identifiers are unsigned 16-bit integers, meaning that the largest number you can put in there is 216-1 = 65535. So there are 65,535 ports available for communication between devices.

A Computer Port is also an interface or a point of connection between the computer and its peripheral devices. Some of the common peripherals are mouse, keyboard, monitor or display unit, printer, speaker, flash drive etc.
The main function of a computer port is to act as a point of attachment, where the cable from the peripheral can be plugged in and allows data to flow from and to the device.
The ports are the physical docking points present in the computer through which the external devices are connected using cables. Or in other words, a port is an interface between the motherboard and an external device of the computer. There are different types of ports available:

* Serial port
* Parallel port
* USB port
* PS/2 port
* VGA port
* Modem port 
* FireWire Port 
* Sockets
* Infrared Port 
* Game Port 
* Digital Video Interface(DVI) Port
* Ethernet Port


### [0.4] What is the difference between http, https, ssh, and other protocols? In what sense are they similar? Name default ports for several data transfer protocols.


**HTTPS is HTTP with encryption and verification.** The only difference between the two protocols is that HTTPS uses TLS (SSL) to encrypt normal HTTP requests and responses, and to digitally sign those requests and responses. As a result, HTTPS is far more secure than HTTP.

**HTTPS:**

It is a secure version of HTTPS, where data sent between the client and server is encrypted. Now hacker when intercept those encrypted data and hackers cannot make any sense of those encrypted data and makes no sense to him and cannot reverse it. Https used port 443 to negotiate the connection. Its mode of authentication is Public/Private Pair. It is mainly developed for secure transferring of data between client and server.

**SSH:**

SSH means "Secured Shell".It is also a secured version, where data sent between the client and server is encrypted.SSH used port 22 is used to negotiate or authenticate the connection. The remote device authentication is done by public-key cryptography. Its mode of authentication is public/private Pair, or userid/password pair. It is made to reduce security threats for remote server login

*SSH, HTTP and HTTPS are protocols to communicate between client and server*

Port 22 for SSH

Port 80 for HTTP

Port 443 for HTTPS

Port 25 for e-mail communication.

Port 53 for DNS 

### [0.4] Explain briefly: (1) what is IP, (2) what IPs are called 'white'/public, (3) and what happens when you enter 'google.com' into the web browser.

(1)IP stands for "Internet Protocol," which is the set of rules governing the format of data sent via the internet or local network. 

(2)A public IP address is an IP address that can be accessed directly over the internet and is assigned to your network router by your internet service provider (ISP). 

(3)When you type www.google.com or any other URL (Uniform Resource Locator) in your web browser and press Enter. 
So the first thing that happens is that your browser looks up in its cache to see if that website was visited before and the IP address is known.
If it can't find the IP address for the URL requested then it asks the operating system to locate the web site.
The first place the operating system is going to check for the address of the URL you specified is in the hosts file (/etc/hosts in Linux and Mac, c:\windows\system32\drivers\etc\hosts in Windows). 
If the URL is not found inside this file, then the OS will make a DNS request to find the IP Address of the web page. 
The first step is to ask the Resolver (or Internet Service Provider) server to look up in its cache to see if it knows the IP Address, 
if the Resolver does not know then it asks the root server to ask the .COM TLD (Top Level Domain) server - the TLD server will again check in its cache to see if the requested IP Address is there. 
If not, then it will have at least one of the authoritative name servers associated with that URL, 
and after going to the Name Server, it will return the IP Address associated with the URL.


After the OS has the IP Address and gives it to the browser, 
it then makes a GET (a type of HTTP Method) to said IP Address. 
When the request is made the browser again makes the request to the OS which then, in turn, packs the request in the TCP traffic protocol, 
and it is sent to the IP Address. On its way, it is checked by both the OS' and the server's firewall to make sure that there are no security violations.
And upon receiving the request the server (usually a load balancer that directs traffic to all available server for that website) 
sends a response with the IP Address of the chosen server along with the SSL (Secure Sockets Layer) certificate to initiate a secure session (HTTPS).
Finally, the chosen server then sends the HTML, CSS, and Javascript files (If any) back to the OS who in turn gives it to the browser to interpret it. 
And then you get the website.

### [0.4] What is Nginx? How does it work on the high level? List several alternative web servers.

NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers.
, NGINX and NGINX Plus can handle hundreds of thousands of concurrent connections, and power more of the Internet’s busiest sites than any other server.

**NGINX Alternatives

* Apache HTTP Server
* Caddy 
* lighttpd


### [0.4] What is SSH, and for what is it typically used? Explain two ways to authenticate in an SSH server in detail.
The Secure Shell Protocol (SSH) is a cryptographic network protocol for operating network services securely over an unsecured network.

SSH uses public-key cryptography to authenticate the remote computer and allow it to authenticate the user, if necessary

SSH applications are based on a client–server architecture, connecting an SSH client instance with an SSH server.[2] SSH operates as a layered protocol suite comprising three principal hierarchical components: the transport layer provides server authentication, confidentiality, and integrity; the user authentication protocol validates the user to the server; and the connection protocol multiplexes the encrypted tunnel into multiple logical communication channels.

SSH is typically used to log into a remote machine and execute commands, but it also supports tunneling, forwarding TCP ports and X11 connections; it can transfer files using the associated SSH file transfer (SFTP) or secure copy (SCP) protocols.[2] SSH uses the client–server model.

Two ways to authenticate in an SSH server:

1) Authentication by password. You have a username and password combination that you use to login to your SFTP server. When you try to log in, the server checks whether your username and password are both correct and if so, approves your request.
2) Authentication by key couple. An SSH key pair, which includes a public and private cryptographic key, is generated by a computer.
The public key is stored on the server that you log into, while the private key is stored on your computer.
When you attempt to log in, the server will check for the public key and then generate a random string and encrypt it using this public key. This encrypted message can only be decrypted with the associated private key.
The server will send this encrypted message to your computer. Upon receipt of the message, your computer will decrypt it using the private key and send this message back to the server. If everything matches up, you’re good to go.

## Problem [6.5]
A real-life situation that occurred to me several times over the years.

Imagine wrapping up a large bioinformatics project and wanting to share raw data with your colleagues in a friendly and straightforward format. The best option would be to use an online genome browser and host your data remotely, so it is easily accessible by anyone with a valid link. This is exactly what we will be doing here.

Please consider doing this HW using Linux since setting up the SSH client on Windows is painful, and I won't be able to help you.

Remote Server:

[2] Create a new virtual machine in the Yandex/Mail/etc cloud (order at least 10GB of free disk space). Generate SSH key pair and use it to connect to your server.

Firstly I created virtual machine in the Yandex cloud :

Name marusya

Platform Intel Ice Lake

Garanted part of vCPU 20%

vCPU 2

RAM 4 GB

Disk SSD 30 GB


Generate SSH key pair and use it to connect to your server.

<code><pre>
ssh-keygen
</code></pre>

When the VM generated - add SSH key and login "avatar"

Connected to VM

<code><pre>
ssh avatar@51.250.12.193 #public IP, then type yes
</code></pre>

[1] Download the latest human genome assembly (GRCh38) from the Ensemble FTP server (fasta, GFF3). Index the fasta using samtools (samtools faidx) and GFF3 using tabix.

download human genome and unzip
<code><pre>wget https://ftp.ensembl.org/pub/release-108/fasta/homo_sapiens/dna/Homo_sapiens.GRCh38.dna.primary_assembly.fa.gz
wget https://ftp.ensembl.org/pub/release-108/gff3/homo_sapiens/Homo_sapiens.GRCh38.108.gff3.gz
gunzip *
</code></pre>

**samtools and tabix installation

<code><pre>
sudo apt-get install -y samtools
sudo apt-get install -y tabix
</code></pre>
Indexing human genome 
<code><pre>
samtools faidx Homo_sapiens.GRCh38.dna.primary_assembly.fa
</code></pre>
[1] Select and download BED files for three ChIP-seq and one ATAC-seq experiment from the ENCODE (use one tissue/cell line). Sort, bgzip, and index them using tabix.
JBrowse 2

[1] Download and install JBrowse 2. Create a new jbrowse repository in /mnt/JBrowse/ (or some other folder).

[0.25] Install nginx and amend its config(/etc/nginx/nginx.conf) to contain the following section:

http {
# Don't touch other options!
# ........
# ........

# Comment this line(!):
# include /etc/nginx/sites-enabled/*;

# Add this:
server {
  listen 80 default_server;
  index index.html;
  server_name _;

  # Don't put JBrowse inside the home directory!
  # You will have problems with permissions
  location /jbrowse/ {
    alias /mnt/JBrowse/;    
  }
}
}
[0.25] Restart the nginx (reload its config) and make sure that you can access the browser using a link like this: http://64.129.58.13/jbrowse/. Here 64.129.58.13 is your public IP address.

[1] Add your files (BED & FASTA & GFF3) to the genome browser and verify that everything works as intended. Don't forget to index the genome annotation, so you could later search by gene names.

Remember to put a persistent link to a JBrowse 2 session with all your BED files and the genome annotation in the report (like this). I must be able to access it without problems.

Extra points [1.5]
[1] Create a Docker container for running JBrowse 2. It should be a self-contained application, listening on the default HTTP port. Users must be able to mount directories with custom configs and access them later without any problems.
Hint: to specify the config, use the config=PATH query parameter. E.g. http://64.129.58.13/jbrowse/?config=my_folder%2Fconfig.json where my_folder%2Fconfig.json is the escaped path to the config file.

[0.5] Give an in-depth explanation of the OSI model and how the TCP/IP stack works. Don't copy-paste descriptions from the internet; paraphrase and shorten as much as possible (imagine writing a cheat sheet for yourself).
