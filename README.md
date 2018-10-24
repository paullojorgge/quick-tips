# Quick tips

1. [Get public IP address](#get-public-ip-address)<br />
2. [Change IP address from command line Linux](#change-ip-address-from-command-line-linux)<br />
3. [Change default gateway](#change-default-gateway)<br />
4. [Display routing table](#display-routing-table)<br />
5. [OpenSSL](#openssl)<br />
5.1 [Verify that a private key matches a certificate](#verify-that-a-private-key-matches-a-certificate)<br />
5.2 [How verify if TLS1.2 is supported on a remote server?](#how-verify-if-TLS1.2-is-supported-on-a-remote-server?)<br />


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


### How verify if TLS1.2 is supported on a remote server?

```
$ openssl s_client -connect <host>:<port> -tls1_2
```

* Example:
    ```
	$ openssl s_client -connect google.com:443 -tls1_2
    ```

If we get the certificate chain and the handshake we know the system in question supports TLS 1.2.
NOTE: we can also test for TLS1 or TLS1.1 with `-tls` or `-tls1_1` respectively!

OR we can also do it using `nmap`:
```
$ nmap --script ssl-enum-ciphers -p <port> <host>
```

* Example: 
    ```
    12:58 $ sudo nmap --script ssl-enum-ciphers -p 443 google.com

	Starting Nmap 7.60 ( https://nmap.org ) at 2018-10-24 13:00 BST
	Nmap scan report for google.com (216.58.206.46)
	Host is up (0.0018s latency).
	Other addresses for google.com (not scanned): 2a00:1450:4009:805::200e
	rDNS record for 216.58.206.46: lhr35s10-in-f14.1e100.net

	PORT    STATE SERVICE
	443/tcp open  https
	| ssl-enum-ciphers: 
	|   TLSv1.0: 
	|     ciphers: 
	|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (prime256v1) - A
	|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (prime256v1) - A
	|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
	|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
	|     compressors: 
	|       NULL
	|     cipher preference: server
	|     warnings: 
	|       64-bit block cipher 3DES vulnerable to SWEET32 attack
	|   TLSv1.1: 
	|     ciphers: 
	|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (prime256v1) - A
	|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (prime256v1) - A
	|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
	|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
	|     compressors: 
	|       NULL
	|     cipher preference: server
	|     warnings: 
	|       64-bit block cipher 3DES vulnerable to SWEET32 attack
	|   TLSv1.2: 
	|     ciphers: 
	|       TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 (prime256v1) - A
	|       TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256 (prime256v1) - A
	|       TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 (prime256v1) - A
	|       TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA (prime256v1) - A
	|       TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA (prime256v1) - A
	|       TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (rsa 2048) - A
	|       TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256 (rsa 2048) - A
	|       TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384 (rsa 2048) - A
	|       TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
	|       TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_AES_128_GCM_SHA256 (rsa 2048) - A
	|       TLS_RSA_WITH_AES_256_GCM_SHA384 (rsa 2048) - A
	|       TLS_RSA_WITH_AES_128_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_AES_256_CBC_SHA (rsa 2048) - A
	|       TLS_RSA_WITH_3DES_EDE_CBC_SHA (rsa 2048) - C
	|     compressors: 
	|       NULL
	|     cipher preference: server
	|     warnings: 
	|       64-bit block cipher 3DES vulnerable to SWEET32 attack
	|_  least strength: C

	Nmap done: 1 IP address (1 host up) scanned in 2.16 seconds
    ```

NOTE: if port is filtered or the previous command doesn't work just try to use the following argument: `-sV`
* Example:
    ```
    12:58 $ sudo nmap -sV --script ssl-enum-ciphers -p 443 google.com	
    ```

