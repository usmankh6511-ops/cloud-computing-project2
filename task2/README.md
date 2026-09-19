# Project 2: The Server Commander
### Cloud Computing (AWS/Azure) — DecodeLabs Internship — Batch 2026

## Overview
A virtual server (EC2 instance) was provisioned on AWS, secured with a
custom Security Group, accessed remotely via SSH, and configured to
host a live web page using Nginx.

## Steps Performed
1. Launched an EC2 instance (Amazon Linux 2023, t2.micro — Free Tier)
2. Created a new SSH key pair for secure access
3. Configured a Security Group:
   - SSH (port 22) restricted to my IP only
   - HTTP (port 80) open to everyone
4. Connected to the instance via SSH
5. Installed and started the Nginx web server
6. Replaced the default Nginx page with a custom "Welcome to DecodeLabs" page
7. Verified the live site by visiting the instance's public IP in a browser

## Tools Used
- AWS EC2
- SSH (OpenSSH / PuTTY)
- Nginx
- Amazon Linux 2023

## Files in this Repository
- `index.html` — the custom welcome page hosted on the server
- `commands.txt` — every command used, in order
- `Project2_ServerCommander_Guide.docx` — full step-by-step guide

## Live Result
Visiting `http://<public-ip>` shows:
> Welcome to DecodeLabs: Mission Accomplished

## Author
Usman — Computer Science, University of Central Punjab (UCP)
