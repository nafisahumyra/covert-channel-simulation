# covert-channel
The simulation demonstrates the use of a covert channel to avoid automated detection of specific keywords.  Blocking packets based on the presence of certain keywords is a common practice in many countries that enforce online censorship

The infrastrucutre required for the simulation uses 3 virtual machines
1. sender (192.168.1.243)
2. router with snort to detect keywords (192.168.1.241, and 10.8.9.149
3. receiver (10.8.9.152)

Packets are sent from 192.168.1.243 using client.py (not covert) or covert_client.py (covert)
The sender and receiver are configured to route packets through the router to the receiver.


Routing
========
route command to run on the receiver, 192.168.1.243
sudo ip route add 10.8.9.152/32 via 192.168.1.241

route command to run on the sender, 10.8.9.152
sudo ip route add 192.168.1.243/32 via 10.8.9.149

Capture Packets WITHOUT using a covert channel
===============================================
1. Command capture packets on the router 192.168.1.241/10.8.9.149
   sudo snort -Q -A console -q -k none -c /etc/snort/snort.conf -i ens3:ens9

**Note:  This simulation assumes snort has been installed on the router (192.168.1.241/10.8.9.149)

2. Start server on 10.8.9.152
   sudo python3 server.py

3. Send Message with keyword "bomb" from 192.168.1.243
   sudo python3 client.py      

Capture Packets USING a covert channel
===============================================

**Note:  scapy must be installed on the sender and receiver.  To install scapy on the sender and receiver type:
sudo pip3 install scapy

1. Command capture packets on the router 192.168.1.241/10.8.9.149
   sudo snort -Q -A console -q -k none -c /etc/snort/snort.conf -i ens3:ens9

**Note:  This simulation assumes snort has been installed on the router (192.168.1.241/10.8.9.149)

2. Start server on 10.8.9.152
   sudo python3 covert_server.py

3. Send Message with keyword "bomb" from 192.168.1.243
   sudo python3 covert_client.py



