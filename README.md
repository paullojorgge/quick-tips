# Quick tips

---

### Get public IP address
1. Using `curl`:
```
curl remote.io |sed 's/<[^>]*>//g' | head -n 1
curl ifconfig.io
curl icanhazip.com
```

2. From `dig` command:
```
dig +short myip.opendns.com @resolver1.opendns.com
NOTE: we can use four resolvers: resolver[1-4].opendns.com
```

3. From web browsers:
  * [Google](https://www.google.com/search?q=what%20is%20my%20IP%20address)
  * [DuckDuckGo](https://duckduckgo.com/?q=ip&ia=answer)
  * [WolframAlpha](https://www.wolframalpha.com/input/?i=what+is+my+ip+address)


---


### Change IP address from command line Linux
```
sudo ifconfig <interface> <ip> netmask <netmask>
sudo ifconfig wlan0 192.168.0.1 netmask 255.255.255.0
```

### Change default gateway
```
sudo route add default gw 192.168.0.253 wlan0
```

### Display routing table
```
route -n
```


---
