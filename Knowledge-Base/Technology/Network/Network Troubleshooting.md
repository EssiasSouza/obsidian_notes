Source: #source/internet_resources 
Project: #project/personal 
Areas: #area/work
Subject: #subject/professional 
Type: #type/pdp
Learning priority: #priority/P0 
Status: #status/in_process
Time Line: #time_line/june_10
Related: [[PDP]]
[[Network Roadmap]]
[[phase1-2026-mon1-june]]
[[English]]

# Hands-On Lab (60 min)

## Linux + Networking Lab

### Connectivity

```
ping google.comping 8.8.8.8
```

Question:

- Why can one fail while the other works?

---

### DNS Inspection

dig installation on WSL

```
sudo apt install dnsutils
```

```
dig google.com
nslookup google.com
cat /etc/resolv.conf
```

Document:

- nameserver
- response
- TTL
- authority section

---

### HTTP Visibility

```
curl -I https://google.comcurl -v https://google.com
```

Observe:

- TLS negotiation
- headers
- redirects

---

### Socket Visibility

```
ss -tulnpnetstat -tulnp
```

Question:

- What is listening on your machine?

---

### Route Visibility

```
traceroute google.com
```

Question:

- How many hops?
- Where does latency increase?