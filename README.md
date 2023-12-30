## Part 1. ipcalc tool
### 1.1. Networks and masks
* Install the ipcalc tool with the command `sudo apt install ipcalc'<br>
* To determine the network address, we use the command `ipcalc 192.167.38.54/13`, we look at the calculated address in the line `Network` <br>
![Network address 192.167.38.54/13](screen/1.1.png)<br>*Network address 192.167.38.54/13*<br>

* We convert the mask 255.255.255.0 into the prefix and binary entry `ipcalc 255.255.255.0`, we look at the result in the string `Network` (prefix - 24, binary - 11111111.11111111.11111111.00000000)<br>
![255.255.255.0 to prefix and binary entry](screen/1.2.png)<br>*255.255.255.0 to prefix and binary entry*<br>
* We convert the mask /15 to the usual and binary `ipcalc /15`, we look at the result in the line `Network'<br>
![/15 to normal and binary](screen/1.3.png)<br>*/15 to normal and binary*<br>
* ipcalc does not convert the mask from the binary system. To calculate the prefix, let's count the number of bits used - 11111111.11111111.11111111.11110000 28 bits are occupied, so the prefix is /28 `ipcalc /28` the usual mask entry in the string `Address`<br>
![/28 to normal](screen/1.4.png)<br>*/28 to normal*<br>

* The minimum and maximum host in the network are displayed in the lines `HostMin` and `HostMax' respectively. We determine the minimum and maximum host in the network 12.167.38.4 with the mask /8`ipcalc 12.167.38.4/8`<br>
![MinMaxHost with mask /8](screen/1.5.png)<br>*MinMaxHost with mask /8*<br>
* with the mask 11111111.11111111.00000000.00000000 `ipcalc 12.167.38.4/16`<br>
![Minmaxhost with mask 11111111.11111111.00000000.00000000](screen/1.6.png)<br>*MinMaxHost with mask 11111111.11111111.00000000.00000000*<br>
* with mask 255.255.254.0 `ipcalc 12.167.38.4 255.255.254.0`<br>
![MinMaxHost with mask 255.255.254.0](screen/1.7.png)<br>*MinMaxHost with mask 255.255.254.0*<br>
* with a mask /4`ipcalc 12.167.38.4/4`<br>
![MinMaxHost with mask /4](screen/1.8.png)<br>*MinMaxHost with mask /4*<br>

### 1.2. localhost
* **localhost** -in computer networks, a standard, officially reserved domain name for private IP addresses (in the range 127.0.0.1 — 127.255.255.254)<br>
* Based on this, you can access the application with the IP: `127.0.0.2` and `127.1.0.1'<br>
* It is not possible to access the application with the IP: `194.34.23.100` and `128.0.0.1`<br>

### 1.3. Network ranges and segments
* Private IP addresses are:<br>
10.0.0.45<br>
192.168.4.2<br>
172.20.250.4<br>
172.16.255.255<br>
10.10.10.10<br>
* Public IP addresses are:<br>
134.43.0.2<br>
172.0.2.1<br>
192.172.0.1<br>
172.68.0.2<br>
192.169.168.1<br>

* To determine which of the listed gateway IP addresses are possible on the 10.10.0.0/18 network, use the command `ipcalc 10.10.0.0/18`, after which we will determine which of the listed addresses fall into the HostMin - HostMax range<br>
![ipcalc command output 10.10.0.0/18](screen/1.9.png)<br>*ipcalc command output 10.10.0.0/18*<br>
* Possible addresses:<br>
10.10.0.2<br>
10.10.10.10<br>
10.10.1.255<br>

## Part 2. Static routing between two machines
* Using the 'ip a` command, we will check the existing network interfaces.<br>
![ws1 network interfaces](screen/2.1.png)<br>*ws1 network interfaces*<br>
![ws2 network interfaces](screen/2.2.png)<br>*ws2 network interfaces*<br>

* In order to describe the network interface, run the command `sudo vim /etc/netplan/00-installer-config.yaml` on each machine. After that, we set the necessary addresses.<br>
![Modified ws1 network interfaces](screen/2.3.png)<br>*Modified ws1* network interfaces<br>
![Modified ws2 network interfaces](screen/2.4.png)<br>*Modified ws2 network interfaces*<br>
* Execute the `netplan apply` command to restart the network service<br>
![netplan apply ws1](screen/2.5.png)<br>*netplan apply ws1*<br>
![netplan apply ws1](screen/2.6.png)<br>*netplan apply ws1*<br>

### 2.1. Adding a static route manually
* Add a static route from one machine to another and back using the command `ip r add`, after which we ping the connection<br>
![ping ws1](screen/2.7.png)<br>*ping ws1*<br>
![ping ws2](screen/2.8.png)<br>*ping ws2*<br>

### 2.2. Adding a static route with saving
* Restarting the machines with the command `sudo reboot'<br>
* Using the text editor, open the file `/etc/netplan/00-installer-config.yaml` on each machine and add a static route<br>
![Static route ws1](screen/2.9.png)<br>*Static route ws1*<br>
![Static route ws2](screen/2.10.png)<br>*Static route ws2*<br>
* Ping the connection on both machines<br>
![Ping ws1](screen/2.11.png)<br>*Ping ws1*<br>
![Ping ws2](screen/2.12.png)<br>*Ping ws2*<br>

## Part 3. iperf3 utility

### 3.1. Connection speed
* 8 Mbps = 1 MB/s<br>
* 100 MB/s = 819200 Kbps,<br>
* 1 Gbps = 1024 Mbps<br>

### 3.2. iperf3 utility
* Install the utility on both machines with the command `sudo apt install iperf3`<br>
* We install one of the machines as a server using the command `iperf3 -s`, on the second we use the command `iperf3 -c 192.168.100.10 -p 5201` to measure the connection speed<br>
![Connection speed ws1](screen/3.1.png)<br>*Connection speed ws1*<br>
![ws2 connection speed](screen/3.2.png)<br>*WS2 connection speed*<br>

## Part 4. Network firewall

### 4.1. iptables utility
* Creating a file /etc/firewall.sh , simulating a firewall, on ws1 and ws2 and add the necessary rules<br>
![firewall.sh ws1](screen/4.1.png)<br>*firewall.sh ws1*<br>
![firewall.sh ws2](screen/4.2.png)<br>*firewall.sh ws2*<br>
* Run the files on both machines with the sudo chmod `+x / commandetc/firewall.sh && sudo sh /etc/firewall.sh `<br>
![Running the ws1 file](screen/4.3.png)<br>*Starting the file ws1*<br>
![Running the ws2 file](screen/4.4.png)<br>*Running the ws2 file*<br>
* The difference between the strategies is that they are executed from top to bottom, that is, if the prohibition rule is higher, it works, and the permission rule is lower, it does not.<br>

### 4.2. nmap utility
* With the `ping` command, we find a machine that does not "ping", after which we use the `nmap` utility to show that the host of the machine is running<br>
![ping and nmap ws2](screen/4.5.png)<br>*ping and nmap ws2*<br>
* Save image dumps of both machines<br>

## Part 5. Static network routing
* We understand five virtual machines (3 workstations (ws11, ws21, ws22) and 2 routers (r1, r2))<br>

### 5.1. Configuration of machine addresses
* Configure machine configurations in etc/netplan/00-installer-config.yaml according to the network in the figure<br>
![ws11 network configuration](screen/5.1.png)<br>*WS11 network configuration*<br>
![ws21 network configuration](screen/5.2.png)<br>*WS21 network configuration*<br>
![ws22 Network Configuration](screen/5.3.png)<br>*WS22 network configuration*<br>
![Network configuration r1](screen/5.4.png)<br>*Network configuration r1*<br>
![Network Configuration r2](screen/5.5.png)<br>*R2 Network Configuration*<br>
* Restart the network service, then use the command `ip -4 a` to check that the address of the machine is set correctly<br>
![ws11 network address](screen/5.6.png)<br>*The network address is ws11*<br>
![ws21 network address](screen/5.7.png)<br>*The network address is ws21*<br>
![ws22 network address](screen/5.8.png)<br>*The network address is ws22*<br>
![Network address r1](screen/5.9.png)<br>*Network address r1*<br>
![Network address r2](screen/5.10.png)<br>*Network address r2*<br>
* Ping ws22 with ws21 in both directions<br>
![Ping ws22 to ws21](screen/5.11.png)<br>*Ping ws22 to ws21*<br>
![Ping ws21 to ws22](screen/5.12.png)<br>*Ping ws21 to ws22*<br>
* Similarly, ping r1 with ws11 in both directions<br>
![Ping r1 on ws11](screen/5.13.png)<br>*Ping r1 on ws11*<br>
![Ping ws11 to r1](screen/5.14.png)<br>*Ping ws11 to r1*<br>

### 5.2. Enabling IP forwarding
* To enable IP forwarding, run the command `sysctl -w net.ipv4.ip_forward=1` on routers<br>
* With this approach, redirection will not work after the system is restarted<br>
![Calling the command on r1](screen/5.15.png)<br>*Calling the command on r1*<br>
![Calling the command on r2](screen/5.16.png)<br>*Calling a command on r2*<br>

* Open the /etc/sysctl.conf file and add the following line to it `net.ipv4.ip_forward = 1`<br>
* When using this approach, IP forwarding is enabled on a permanent basis<br>
![Changed sysctl.conf to r1](screen/5.17.png)<br>*Changed sysctl.conf to r1*<br>
![Changed sysctl.conf to r2](screen/5.18.png)<br>*Changed sysctl.conf to r2*<br>

### 5.3. Default route configuration
* Setting up the default route (gateway) for workstations<br>
![Modified file to ws11](screen/5.19.png)<br>*Modified file to ws11*<br>
![Modified file to ws21](screen/5.20.png)<br>*Modified file to ws21*<br>
![Modified file to ws22](screen/5.21.png)<br>*Modified file to ws22*<br>
* Calling 'ip r` to show the route added to the routing table<br>
![Route to ws11](screen/5.22.png)<br>*Route to ws11*<br>
![Route to ws21](screen/5.23.png)<br>*Route to ws21*<br>
![Route to ws22](screen/5.24.png)<br>*Route to ws22*<br>
* Ping ws11 with r2 and check on r2 that the ping reaches. To do this, use the command `sudo tcpdump -tn -i enp0s3`<br>
![Ping from ws11 to r2](screen/5.25.png)<br>*Ping from ws11 to r2*<br>
![Ping reaches r2](screen/5.26.png)<br>*Ping reaches r2*<br>

### 5.4. Adding static routes
* Adding static routes to routers r1 and r2 in the configuration file<br>
![r1 configuration file](screen/5.27.png)<br>*r1 configuration file*<br>
![r2 configuration file](screen/5.28.png)<br>*r2 configuration file*<br>
* Calling 'ip r` to check the route table on both routers<br>
![ip r on r1](screen/5.29.png)<br>*ip r on r1*<br>
![ip r on r2](screen/5.30.png)<br>*ip r on r2*<br>
* Run the commands `ip r list 10.10.0.0/18` and `ip r list 0.0.0.0/0' on ws1<br>
![ip r commands on ws11](screen/5.31.png)<br>*ip r commands on ws11*<br>
* The first IP address and mask correspond to the route set in the network plan (10.10.0.0 /18), and the other does not (one of the things that does not fit the rule is 0.0.0.0/0 is outside the set mask), so it follows the default route<br>

### 5.5. Making a router list
* Run the dump command `tcpdump -tnv -i enp0s3` on r1<br>
* Using the traceroute utility, we build a list of routers on the path from ws11 to ws21 with the command `traceroute 10.20.0.10 -n`<br>
![traceroute on ws11](screen/5.32.png)<br>*traceroute on ws11*<br>
![tcpdump on r1](screen/5.33.png)<br>*tcpdump on r1*<br>
* How traceroute works:<br>
* To identify intermediate routers, traceroute sends a series of data packets to the target node, each time increasing by 1 the value of the TTL ("lifetime") field. This usually results in a large number of routers that can be traversed by the packet. The first packet is sent with TTL, high speed 1, so the first response returns an ICMP message indicating that data cannot be delivered. Traceroute records the address of the router, as well as the time between sending a packet and receiving a response (you use this information on your computer monitor). Then traceroute repeats sending the packet, but with a TTL of size 2, which allows the first router to skip the packet further.<br>
* The process is repeated until, when the TTL value is insufficient, the packet is absorbed by the target packet. Upon receiving a response from this packet, the tracing process is completed.<br>

### 5.6. Using the ICMP protocol for routing
* Run on r1 interception of network traffic passing through eth0 using the command `tcpdump -n -i eth0 icmp`<br>
* Ping a non-existent IP from ws11 using the command `ping -c 1 10.30.0.111`<br>
![Ping on ws11](screen/5.34.png)<br>*Ping on ws11*<br>
![tcpdump on r1](screen/5.35.png)<br>*tcpdump on r1*<br>
* We make a dump for each machine

## Part 6. Dynamic IP configuration using DHCP
* For r2, configure the configuration of the DHCP service in the file `/etc/dhcp/dhcpd.conf'<br>
![dhcpd.conf on r2](screen/6.1.png)<br>*dhcpd.conf on r2*<br>
* In the resolv.conf file we write `nameserver 8.8.8.8`<br>
![resolv.conf on r2](screen/6.2.png)<br>*resolv.conf on r2*<br>
* Restart the DHCP service with the command `systemctl restart isc-dhcp-server`<br>
![DHCP reboot on r2](screen/6.3.png)<br>*Rebooting DHCP on r2*<br>
* Reboot the ws21 machine using `reboot` and check via `ip a` that it has received the address<br>
![Address on ws21](screen/6.4.png)<br>*Address on ws21*<br>
* Ping ws22 with ws21<br>
![Ping on ws21](screen/6.5.png)<br>*Ping on ws21*<br>
* Specify the MAC address of ws11, for this in etc/netplan/00-installer-config.yaml should add the lines: `macaddress: 10:10:10:10:10: BA`, `dhcp4: true`<br>
![Added MAC address to ws11](screen/6.6.png)<br>*Added MAC address to ws11*<br>
* For r1, we configure it similarly to r2, but we issue addresses with a tight binding to the MAC address (ws11). We also do similar tests<br>
![dhcpd.conf on r1](screen/6.7.png)<br>*dhcpd.conf on r1*<br>
![resolv.conf on r1 and DHCP reboot](screen/6.8.png)<br>*resolv.conf on r1 and DHCP reboot*<br>
![Address on ws11](screen/6.9.png)<br>*Address on ws11*<br>
![Ping on ws11](screen/6.10.png)<br>*Ping on ws11*<br>
* Calling 'ip a` on ws21, then requesting an ip update and calling `ip a' again<br>
![IP update to ws21](screen/6.11.png)<br>*IP update on ws21*<br>
* dhcilent uses the dynamic host configuration protocol to dynamically configure the network parameters of the network interface<br>
* The name of the network interface that dhclient should try to configure can be specified on the command line. If the interface name is not specified on the command line, dhclient usually identifies all network interfaces, removes non-broadcast interfaces if possible, and tries to configure each interface<br>

## Part 7. NAT
* Install Apache2 with the command `sudo apt-install apache2'<br>
* In the file `/etc/apache2/ports.conf` on ws22 and r1, we change the line Listen 80 to Listen 0.0.0.0:80, that is, we make the Apache2 server public<br>
![Modified ports.conf](screen/7.1.png)<br>*Changed ports.conf*<br>
* Launch the Apache web server with the command `service apache2 start` on ws22 and r1<br>
![Running Apache on ws22](screen/7.2.png)<br>*Running Apache on ws22*<br>
![Running Apache on r1](screen/7.3.png)<br>*Running Apache on r1*<br>
* Add the following rules to the firewall created by analogy with the firewall from Part 4 on r2:<br>
* 1) Deleting rules in the filter - iptables -F table<br>
* 2) Deleting rules in the "NAT" table - iptables -F -t nat<br>
* 3) Drop all routed packets - iptables --policy FORWARD DROP<br>
![firewall.sh on r2](screen/7.4.png)<br>*firewall.sh on r2*<br>
* Run the file the same way as in Part 4<br>
![Launch firewall.sh on r2](screen/7.5.png)<br>*Launch firewall.sh on r2*<br>
* Check the connection between ws22 and r1 with the `ping` command. When running a file with these rules, ws22 should not "ping" with r1<br>
![Ping with r1](screen/7.6.png)<br>*Ping with r1*<br>
![Ping with ws22](screen/7.7.png)<br>*Ping with ws22*<br>
* We allow routing of all ICMP protocol packets<br>
![firewall.sh](screen/7.8.png)<br>*firewall.sh*<br>
* Check the connection between ws22 and r1 with the `ping` command. When running a file with these rules, ws22 should "ping" with r1<br>
![Ping with ws22](screen/7.9.png)<br>*Ping with ws22*<br>
![Ping with r1](screen/7.10.png)<br>*Ping with r1*<br>
* Add two more rules to the file:<br>
5) Enable SNAT, namely masking of all local ip from the local network located behind r2 (according to the designations from Part 5 - network 10.20.0.0)<br>
6) Enable DNAT on the 8080 port of the r2 machine and add access from outside the network to the Apache web server running on ws22
![firewall.sh](screen/7.11.png)<br>*firewall.sh*<br>
* We check the TCP connection for SNAT, for this we connect to the Apache server on r1 from ws22 with the command: `telnet [address] [port]`<br>
![telnet on ws22](screen/7.12.png)<br>*telnet on ws22*<br>
* Check the TCP connection for DNAT, for this we connect to the Apache server on ws22 with the command `telnet`<br> from r1
![telnet on r1](screen/7.13.png)<br>*telnet on r1*<br>

## Part 8. Bonus. Introduction to SSH Tunnels
* Launch a firewall on r2 with the rules from Part 7<br>
* Running Apache web server on ws 22 only on localhost<br>
![web server](screen/8.1.png)<br>*web server*<br>
* Use Local TCP forwarding from ws21 to ws22 to access the web server on ws22 from ws21
![local TCP](screen/8.2.png)<br>
![local TCP](screen/8.3.png)<br>*Local TCP*<br>
* Use Remote TCP forwarding from ws11 to ws22 to access the web server on ws22 from ws11<br>
* Start with ws11 ssh -R remote_port:local_ip:local_port user@hostname<br>
![Remote TCP](screen/8.4.png)<br>*Remote TCP*<br>
* Let's check if the connection worked<br>
![Telnet](screen/8.5.png)<br>
![Telnet](screen/8.6.png)<br>*Telnet*<br>
