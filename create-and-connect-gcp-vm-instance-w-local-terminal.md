# A.1. Create a Virtual Machine in Google Cloud Platform (_no GPU_)

_If you're using pay-as-you-go GPU instances, you can skip this section and continue with B.1._

This guide explains how to create a virtual environment on Google Cloud Platform (GCP) to install the Hailo Software Suite (SW Suite) witout GPU. While it's possible to use only a CPU instance to get familiar with the software, using a GCP instance (option B) with a GPU provides good performance.

#### Prerequisits

- Google account
- Access to [Google Cloud Platform](https://console.cloud.google.com) (Free trail)

## Steps
1. Create a VM instance
2. Generate public- and private key
3. Upload public key to CGP
4. Use private key to connect VM with local terminal

## 1. Create a VM instance
This guide provides a basic configuration. You might need to adjust settings based on your specific needs.

1. **Create or Select a Project**: Creat a `NEW PROJECT` or use an excisting project in Google Cloud Platform (cloud.google.com)
2. **Create VM instance**: In the Navigation Menu (top left) navigate to `Compute Engine` > `VM instances` > click `CREATE INSTANCE` (top middel)
3. **Configure the VM instance**: 
**Name**: choose a descriptive name for your VM.

**Region/Zone**: Select a region based on your location or target audience (affects latency). Choose a zone within the region for redundancy (availability). 

**Machine configuration**: select `E2`, select `e2-standard-32gb` (16gb to save costs), available policies (optional): GCP offers Spot VMs for lower costs. However, these VMs can be interrupted if needed by GCP, but 60-90% discount. 

**Boot disk**: Operating system: select `x86/64 Ubuntu 20.04`. Optional: Boot disk type: select `Standard persistent disk` to save costs. Size (GB): `200 GB` and resize upwars later if needed. Downgrading size is not possible. 

**Firewall**: activate `allow HTTP traffic` (needed for Jupyter notebook on local browser).
5. Click `CREATE`

## 2. Generate public- and private key

In your local terminal generate a public and private key with the following command below. Replace PATH with a secure location on your local drive where you store your keys (for example ~/.ssh/). Replace KEYNAME with a descriptive name (for example CGPkey). Replace USERNAME with your username in GCP. Optional: a strong passphrase for added security.
```sh
ssh-keygen -t rsa -b 4096 -f PATH/KEYNAME -C USERNAME
```

## 3. Upload public key to CGP

1. In GCP Navigation Menu navigate to `Compute Engine` > `Metadata` > `SSH KEYS` > `EDIT` > `+ADD ITEM`.
2. Open your public key in your local terminal (for example with nano GCPkey.pub).
3. Copy this key.
4. In GCP click `ADD SSH KEY`.
5. Paste the key in SSH key.
6. Click `SAVE`.

## 4. Use private key to connect VM with local terminal

1. **Obtain VM External IP**: In the GCP Navigation Menu, navigate to `Compute Engine` > `VM instances`. Ensure the VM instance is active. If not, click Start/resume in the instance's menu. Copy External IP address of the instance. (Right side of the menu bar)
2. **Establish SSH Connection**: Open a terminal in your local machine. Use the following command, replacing ~/.ssh with the path you use to store public- and private keys. Replace USERNAME with your GCP username and EXTERNALIP with the copied IP, confirm  the connection when prompted.
```sh
ssh -i ~/.ssh/CGPkey USERNAME@EXTERNALIP
```
	
For the next time you SSH you VM instance, this External IP will change. Remember to copy-paste it in the command.

Continue with [Docker Install of the Hailo Software Suite](https://github.com/marcory-hub/hailo/blob/main/install-hailo-software-suite-on-google-cloud-VM-instance.md).

## Troubleshooting 

### General
**Copy hailo zip to VM**: Ensure you are in you local terminal (and not the VM terminal).

### Connection refused
**Firewall**: Ensure SSH port (22 by default) is open in the VM's firewall rules.

**SSH service**: Verify SSH service is running on the VM using `sudo systemctl status sshd`. If not, start it with `sudo systemctl start sshd`.

### Permission denied (publickey)
**Public Key**: make sure you have copied the whole pub key. The key starts with ssh-rsa and ends with your ==USERNAME. Usually your key is located at ~/.ssh/.

**Authorized_keys**: Verify your public key is added to metadata the authorized_keys file on the VM.

**SSH KEYS**: Make sure you added the key under `Compute Engine` -> `Settings`-`Metadata`-> `SSH KEYS` (and not under METADATA)

### Connection timed out
**Network connectivity**: Check your internet connection and network configuration.

**VM status**: Ensure the VM is running and accessible.
