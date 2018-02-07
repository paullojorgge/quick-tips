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

### OpenSSL

#### Verify that a private key matches a certificate
In order to process with this action we need to i) check the consistency of the
private key and ii) compare the modulus of the public key in the certificate against the
modulus of the private key:

1. To verify the consistency: `openssl rsa -modulus -noout -in my.key | openssl md5`
2. To view the modulus: `openssl x509 -modulus -noout -in my.crt | openssl md5`

If the first command show any errors, or if the modulus of the public key in the
certificate and the modulus of the private key do not match, then it means that we
are not using the correct private key.
