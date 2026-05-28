# ZEN0SEC — Workstation OS Setup Guide

**Configuration:** Ubuntu 24.04 LTS + Parrot OS 7.2 Security Edition (Dual Boot)  
**Hardware:** Workstation Tower  
**Boot Manager:** GRUB2

---

## OS Role Assignment

| OS | Role | Use Cases |
|---|---|---|
| **Ubuntu 24.04 LTS** | Primary — daily driver | Dev, scripting, ZPAK work, Python, AI/ML, general productivity |
| **Parrot OS 7.2 Security** | Secondary — offensive research | Red team ops, lab exercises, tool testing, security research |

---

## Installation Order

> **Critical:** Always install Ubuntu FIRST, then Parrot. Ubuntu's GRUB will be overwritten by Parrot's GRUB, which is the desired outcome since Parrot's GRUB will detect both.

### Step 1: Install Ubuntu 24.04 LTS

1. Download Ubuntu 24.04 LTS ISO: https://ubuntu.com/download/desktop
2. Flash to USB with Balena Etcher or `dd`
3. Install Ubuntu to first partition (recommend 200-300GB minimum)
4. Allow Ubuntu to install GRUB to MBR/EFI
5. Boot into Ubuntu, verify everything works
6. Note the partition where Ubuntu is installed (`/dev/sdaX` or NVMe equivalent)

### Step 2: Install Parrot OS 7.2 Security

1. Download Parrot OS 7.2 Security Edition: https://parrotsec.org/download/
   - Choose: **Security Edition** (NOT Home, NOT HTB, NOT Architect)
   - Desktop: MATE (recommended for performance) or KDE
2. Flash to USB
3. Boot from USB
4. Install to second partition (recommend 150-200GB minimum)
5. **When GRUB install prompt appears:** install GRUB to the MBR/EFI (same as Ubuntu did)
6. Parrot's GRUB will auto-detect Ubuntu and add it to the boot menu

### Step 3: Verify Dual Boot

1. Reboot — GRUB menu should show both:
   - `Parrot GNU/Linux`
   - `Ubuntu 24.04 LTS`
2. Test boot into each OS

### Step 4: Configure GRUB Default (Optional)

```bash
# Set Ubuntu as default (boots Ubuntu unless you select otherwise)
sudo nano /etc/default/grub
# Change: GRUB_DEFAULT=0  (0 = first entry, usually Parrot)
# To set Ubuntu as default, find its position in 'grub-mkconfig' output
# or use: GRUB_DEFAULT="Ubuntu"

sudo update-grub
```

---

## Ubuntu 24.04 — Post-Install Hardening

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install USG (Ubuntu Security Guide) for CIS compliance
sudo apt install ubuntu-advantage-tools
sudo pro enable usg
sudo apt install usg

# Run CIS Level 1 audit
sudo usg audit cis_level1_server

# Generate tailored CIS profile
sudo usg generate-tailoring cis_level1_server usg-tailoring.xml

# Apply OpenSCAP OVAL vulnerability scan
sudo apt install libopenscap8 openscap-scanner
# Download Canonical OVAL content:
wget https://security-metadata.canonical.com/oval/com.ubuntu.noble.usn.oval.xml.bz2
bzip2 -d com.ubuntu.noble.usn.oval.xml.bz2
sudo oscap oval eval --report report.html com.ubuntu.noble.usn.oval.xml
```

---

## Parrot OS 7.2 — Post-Install Setup

```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Verify key tooling is present
which nmap metasploit-framework burpsuite wireshark aircrack-ng

# Install any missing tools
sudo apt install bloodhound neo4j impacket-scripts crackmapexec

# Set up ZEN0SEC scripts lib
git clone https://github.com/zenShaunD/scripts-lib ~/zpak/scripts-lib
cd ~/zpak/scripts-lib && chmod +x zen0

# Clone this HQ repo
git clone https://github.com/zenShaunD/Resources-zen0sec ~/zpak/zen0sec-hq
```

---

## Hypervisor Setup (for Lab VMs)

### Option A: VirtualBox (Free, cross-OS)
```bash
# Install on Ubuntu
sudo apt install virtualbox virtualbox-ext-pack

# Create host-only networks for lab segments
# VirtualBox → File → Host Network Manager:
# vboxnet0: 10.10.10.1/24 (Attacker segment)
# vboxnet1: 10.10.20.1/24 (AD segment)
# vboxnet2: 10.10.30.1/24 (DMZ segment)
# vboxnet3: 10.10.40.1/24 (Management segment)
# DISABLE DHCP on all — use static IPs per lab design
```

### Option B: KVM/libvirt (Better performance, Linux-native)
```bash
# Install on Ubuntu
sudo apt install qemu-kvm libvirt-daemon-system virt-manager bridge-utils
sudo usermod -aG libvirt,kvm $USER

# Create isolated networks via virt-manager or virsh
# Use 'isolated' network type (no forwarding to host interface)
```

---

## Partition Layout Reference

```
/dev/sda (or nvme0n1)
├── /dev/sda1   EFI System Partition     512MB
├── /dev/sda2   Ubuntu 24.04 /           250GB ext4
├── /dev/sda3   Parrot OS 7.2 /          200GB ext4
└── /dev/sda4   Shared Data (optional)   Remaining  ext4 or exfat
```

> Swap: use swap files (not partitions) on modern systems — one per OS in its own root.

---

## Troubleshooting

| Issue | Fix |
|---|---|
| GRUB only shows Parrot | Run `sudo update-grub` from Parrot — it should detect Ubuntu |
| GRUB only shows Ubuntu | Boot Parrot USB, reinstall GRUB from Parrot chroot |
| Secure Boot blocking Parrot | Disable Secure Boot in BIOS/UEFI |
| VMs have internet in lab | Set VM adapters to Host-Only, remove NAT adapter |
