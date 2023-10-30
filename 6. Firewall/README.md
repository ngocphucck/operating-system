1. Check iptables: ```$ iptables --list -n -v```
2. Check filter tables: ```$ iptables -t filter --list -n -v```
3. Check NAT tables: ```$ iptables -t nat --list -n -v```
4. Open port 443: ```$ iptables -A INPUT -p tcp --dport 443 -j ACCEPT```
5. Close port 80: ```$ iptables -A INPUT -p tcp --dport 80 -j DROP```
6. Open port 22 for SSH: ```$ iptables   -A  INPUT  -p tcp  --dport  22  -j  ACCEPT```
```$ iptables  -A  INPUT  -m state --state RELATED,ESTABLISHED  -j  ACCEPT```
7. Close all ports without accept ports: ```$ iptables  -P   INPUT  DROP```
8. Return to accept status: ```$ iptables  -P  INPUT  ACCEPT```