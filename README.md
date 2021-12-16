# Netfilter
Nftables learning


#4.3.1 - counter.conf
```bash
#counter for incoming
counter

#counts and drops any traffic not covered by an earlier rule
counter drop
```

#4.3.2 icmp.conf
```bash
#gdzie 56 to numer grupy
ip saddr 192.168.56.0/24 icmp type echo-request counter accept	
```

#4.3.3 netcat.conf
```bash
tcp dport { 80,443 } ct state new counter accept 
```

#4.3.4 output.conf
```
    chain outgoing {
        type filter hook output priority 0; policy accept;
        counter
    }
```

#4.3.5 external.conf
```bash
 ip daddr eti.pg.edu.pl tcp dport { 80,443 } counter reject 
```

#5.1.1 nat.conf
```bash
 chain postrouting {
        type nat hook postrouting priority 100; 
        #172.17.0.0/24 - network of docker machine 
        #enp0s3 - internet interface
        #10.0.2.15 - ip of computer that have connection to internet
        ip saddr 172.17.0.0/24 oif enp0s3 snat 10.0.2.15 
    }
```

#5.1.2 Ipv6 nat.conf
```bash
 #12 - group #enp0s3 - existing interface of internet, ~ eth0
ip6 saddr FD00:12::/64 oif enp0s3 snat 2001:4070:11:204::11:12
```

#5.2.1
```bash
  chain forwarding {
        type filter hook forward priority 0; policy drop;
  }
```

#5.2.2 
```bash
    chain incoming {
        type nat hook input priority 0; policy drop
        icmpv6 type { nd-neighbor-solicit, nd-router-advert } counter accept
    }

   chain forwarding {
        type filter hook forward priority 0; policy drop;
         
        ct state { established,related } accept
        #1172.17.0 docker group
        ip saddr 172.17.0.0/24 accept

        #12 - group
        ip6 saddr FD00:12::/64 accept
    }
```