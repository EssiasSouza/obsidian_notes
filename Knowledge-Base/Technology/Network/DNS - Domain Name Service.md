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

---
# How does DNS work?

All machines in the internet are identified by a number of identification. This number is called IP address, i.e. Internet Protocol address.

Numbers are so difficult to remember and to solve this, the Domain Name System (DNS) was created, that solves names as IP numbers.

For example:

When you type on your browser essias.com.br your computer looks in the local cache if you accessed it before. If you accessed this site before, your computer already has the IP number saved in the cache. So, your computer already knows the IP number that it needs to access.

Let's imagine if that is not the scenario. Your computer is trying to access a website never accessed before.

Your computer knows the Name Server that is capable to solve the name, so the name server answer it with the IP number. Then your computer saves it in its cache for the next access.

In Linux systems is necessary to configure the name servers in the file `/etc/resolv.conf`