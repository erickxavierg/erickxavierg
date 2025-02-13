# Nfdump

<img width="427" alt="Screenshot 2024-11-16 at 11 29 22 AM" src="https://github.com/user-attachments/assets/ba3261ba-27dc-4b5f-b473-c36e45881da6">

Nfdump is a network flow data collector and analyzer that captures and stores information about network traffic flows in a format that's efficient for analysis. It's useful because it provides insights into network usage and can be used to troubleshoot network issues and detect potential security threats.

### Scope:

I will be installing Nfdump and utilizing it to convert a pcap file into a nfcapd file, which is the format required for analyzing flow data. I will then use nfdump to analyze the resulting flow data. Doing this offers several benefits:

+ By using nfdump to analyze flow data, you can quickly identify performance issues, network bottlenecks, or anomalies like unexpected traffic spikes or security threats.
+ Flow-based data is more compact than raw packet captures, enabling easier handling and storage of large volumes of network traffic over time.
+ Allows for more efficient analysis of network traffic, focusing on high-level flow statistics rather than raw packet data.

### Tools and Technology:
Linux and Nfdump

## Installing Nfdump

This is the process I used to get Nfdump installed. To start, I first installed the needed dependencies:

```
apt install make gcc flex rrdtool librrd-dev libpcap-dev php librrds-perl libsocket6-perl apache2 libapache2-mod-php libtool dh-autoreconf pkg-config libbz2-dev byacc doxygen graphviz librrdp-perl libmailtools-perl build-essential autoconf
```

![Pasted image 20240402011608](https://github.com/lm3nitro/Projects/assets/55665256/3be383d0-f17a-4ce8-84db-07f4717ced61)

Next, I downloaded Nfdump. At the time of this writing, the latest version was v1.6.23:

```
wget https://github.com/phaag/nfdump/archive/v1.6.23.tar.gz
```

![Pasted image 20240402011755](https://github.com/lm3nitro/Projects/assets/55665256/5fa026b7-c2df-4af8-a29a-894cec1f0c8e)

Untar download:

```
tar xvzf nfdump-1.6.23.tar.gz

```
![Pasted image 20240402011851](https://github.com/lm3nitro/Projects/assets/55665256/3f4f0c07-a74b-45bb-9aa7-46306bf4872f)

```
cd nfdump-1.6.23/  
sh ./autogen.sh  
```

![Pasted image 20240402013240](https://github.com/lm3nitro/Projects/assets/55665256/1e642a35-6516-41b0-aebf-0493b1240553)


```
./configure --enable-nsel --enable-nfprofile --enable-sflow --enable-readpcap --enable-nfpcapd --enable-nftrack  
```

Build:

```
make && make install
```

![Pasted image 20240402013454](https://github.com/lm3nitro/Projects/assets/55665256/a307952d-c493-49fd-904c-477504a2e6bf)

> [!TIP]
> It may be necessary to run **/sbin/ldconfig** or **ldconfig** as root after the installation.

Once the install completed, I verified the version with th following command:

```
nfdump -v
```

## Pcap Analysis

Now that I have nfdump installed, I can use it. Here I will be using nfcapd to convert a pcap to nfcapd:

```
nfcapd -r ICMP_attack_DoS_attack.pcap -l.
```

![Pasted image 20240402021211](https://github.com/lm3nitro/Projects/assets/55665256/4c2aaef2-ed14-4e4f-b0e1-37a715e88a58)

Now that it has been converted, I used nfdump to read the nfcapd file: 

![Pasted image 20240402021758](https://github.com/lm3nitro/Projects/assets/55665256/cea98049-d3bf-4355-b064-41bc7ee5f66d)

A view of the different protocols and information found within the file:

![Pasted image 20240402022640](https://github.com/lm3nitro/Projects/assets/55665256/5effc527-e197-4202-b838-ff46bdd609f6)

Nfdump's filter syntax is similar to tcpdump, but it's designed for NetFlow data. It lets you easily extract and analyze specific data based on criteria like IP addresses, ports, and protocols. In addition to filtering, nfdump also provides features for displaying top statistics, such as the top talkers, services, destinations, and more.

### Summary

In this exercise, I was able to download and install Nfdump. I then converted a pcap file into a nfcapd and used Nfdump to read the information provided in the file. This information provided insights into the protocols and traffic captured. The flow data that Nfdump offers has several benefits:

+ Identify which devices or IP addresses are generating the most traffic (Top Talkers)
+ Find which services are active on the network
+ Detect traffic patterns that are out of the ordinary (unexpected IP addresses or ports)

Overall, Nfdump can be used for performance monitoring, troubleshooting, reporting, and traffic trend analysis. 
