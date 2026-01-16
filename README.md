# firewallsetup
## Fast Firewall Setup

This is a script for setting up a firewall with settings for tarpitting ssh and basic protections that everyone needs.

## Download the rules to /etc/
```
git clone https://github.com/sanjay556/firewall.git
````
## Make the Rules Permanent
### Debian-based Distributions
```
sudo apt install iptables-persistent
sudo /etc/init.d/netfilter-persistent save
```
### Arch Linux Distributions
*Use iptable-save which is pre-installed*
```
sudo iptables-save > /etc/iptables/iptables.rules
```
### RHEL / CentOS Distributions
*This is by far the simpliest way to save rules and check them # chkconfig --list | grep iptables*

*Note: chkconfig iptables on only needs to be run once to turn the service on*
```
sudo chkconfig iptables on
sudo service iptables save
```



## Using Port Redirection (iptables)
You can use the Linux firewall to redirect incoming traffic from port 80 to a port your user can access (like 8080).

    Redirect incoming traffic: 
    sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j REDIRECT --to-port 8080

    Redirect local traffic (for testing on the same machine): 
    sudo iptables -t nat -A OUTPUT -o lo -p tcp --dport 80 -j REDIRECT --to-port 8080



* Exposing port 14001 and having external IP and natting in Firewall-  from your server

        iptables -t nat -A PREROUTING -p tcp --dport 14001 -j DNAT --to-destination <remoteServer>:14001
        iptables -t nat -A POSTROUTING -p tcp -d <remoteServer> --dport 14001 -j MASQUERADE
        iptables -A FORWARD -p tcp -d <remoteServer> --dport 14001 -m state --state NEW,ESTABLISHED,RELATED -j ACCEPT
        iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

        OS,Save Command
        Debian/Ubuntu,sudo /etc/init.d/netfilter-persistent save
        Arch Linux,sudo iptables-save > /etc/iptables/iptables.rules
        RHEL/CentOS,sudo service iptables save

