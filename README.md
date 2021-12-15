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