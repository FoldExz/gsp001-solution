# Create a Virtual Machine

**Lab:** GSP001 – Create a Virtual Machine  
**Completed on:** July 23, 2025 at 00:21 (Asia/Jakarta)  
**Environment:** Google Cloud Shell

## Overview

This lab demonstrates how to create and manage virtual machines using Google Cloud Compute Engine via both Cloud Console and the `gcloud` CLI.

### Tasks:
1. Create a VM instance using the Cloud Console (via `gcloud` CLI).
2. Install and verify an NGINX web server.
3. Create another VM instance using `gcloud`.
4. Complete a short knowledge check.

---

## Task 1: Create a New VM Instance

```bash
gcloud config set compute/region us-east1
export REGION=us-east1
export ZONE=us-east1-b
```

```bash
gcloud compute instances create gcelab \
  --zone=us-east1-b \
  --machine-type=e2-medium \
  --image-family=debian-11 \
  --image-project=debian-cloud \
  --boot-disk-type=pd-balanced \
  --boot-disk-size=10GB \
  --tags=http-server \
  --metadata=startup-script='#! /bin/bash' \
  --quiet
```

### Allow HTTP traffic:
```bash
gcloud compute firewall-rules create allow-http \
  --allow tcp:80 \
  --target-tags=http-server \
  --direction=INGRESS \
  --priority=1000 \
  --network=default
```

### SSH into the instance:
```bash
gcloud compute ssh gcelab --zone=us-east1-b
```

---

## Task 2: Install NGINX Web Server

Inside the `gcelab` VM:

```bash
sudo apt-get update
sudo apt-get install -y nginx
ps auwx | grep nginx
```

Optional: View the web server via  
`http://[EXTERNAL_IP]` (check using `gcloud compute instances list`)

---

## Task 3: Create Another VM via `gcloud`

```bash
gcloud config set compute/zone us-east1-b
gcloud compute instances create gcelab2 --machine-type e2-medium --zone=$ZONE
```

Optional: Verify or connect:

```bash
gcloud compute instances list
gcloud compute ssh gcelab2 --zone=us-east1-b
```

---

## Task 4: Test Your Knowledge

Ways to create a VM instance in Compute Engine:  
- ✅ The gcloud command line tool  
- ✅ The Cloud Console

---

*This README documents the core steps and commands for completing the "Create a Virtual Machine" lab using Google Cloud Shell.*  
