# Port Scanning with Nmap in a Dockerized Kali Linux Environment

A hands-on lab where I set up a Kali Linux environment from scratch using Docker, installed Nmap, and ran a series of port scans against an authorized test target — including comparing scan behavior between root and non-root users.

## Overview
Rather than using a pre-built Kali VM, this lab builds a Kali Linux container from a Dockerfile and runs it interactively. From there, it walks through installing Nmap and using it to explore SYN scans, privilege requirements, and packet-level tracing.

## What I did

### 1. Built and ran a Kali Linux Docker container
Pulled a Dockerfile for a Kali Linux image, built it locally, and launched it as an interactive container — a clean, disposable environment for the rest of the lab.

### 2. Installed Nmap and ran a baseline scan
Installed Nmap via apt, then ran a default scan against `scanme.nmap.org` (the Nmap project's official, publicly authorized test target).

### 3. Ran a targeted SYN scan as root
Scanned three specific ports (22, 113, 139) using Nmap's default SYN scan — a stealth technique that can check thousands of ports quickly without completing a full TCP handshake.

### 4. Tested the same scan as a non-root user
Created a new unprivileged user, switched to it, and tried the same SYN scan. It failed — SYN scans need raw socket access, which requires root/admin privileges. Explicitly passing `-sS` as a non-root user still didn't bypass the restriction.

### 5. Inspected scan behavior at the packet level
Switched back to root and re-ran the scan with `--packet-trace` (and a higher debug level, `-d5`) to see exactly what Nmap was sending and receiving at the packet level, rather than just the summarized results.

## Key takeaways
- SYN scans are fast and relatively stealthy because they never complete the TCP handshake, but that same behavior is exactly why they require elevated privileges to execute
- Privilege separation isn't just theoretical — watching a regular user get explicitly blocked from a raw-socket scan makes the OS-level enforcement concrete
- Packet tracing turns Nmap from a black box into something you can actually verify — useful for understanding what's really happening on the wire, not just trusting the summary output

## Tools
Docker, Kali Linux, Nmap

---
*Completed as a hands-on Skills Network lab.*
