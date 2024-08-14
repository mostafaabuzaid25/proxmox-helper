```bash
#!/bin/bash

# Change to working directory
cd /usr/share/javascript/proxmox-widget-toolkit

# Make a backup
cp proxmoxlib.js proxmoxlib.js.bak

# Edit the file proxmoxlib.js
# Use sed to locate and insert the required code
sed -i '/checked_command: function(orig_cmd) {/a \    orig_cmd();\n    return;' proxmoxlib.js


# Backup existing sources.list
cp /etc/apt/sources.list /etc/apt/sources.list.bak

# Write new sources.list
cat <<EOF > /etc/apt/sources.list
deb http://ftp.us.debian.org/debian bookworm main contrib
deb http://ftp.us.debian.org/debian bookworm-updates main contrib
deb http://security.debian.org bookworm-security main contrib
deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription
deb http://archive.ubuntu.com/ubuntu/ focal main restricted
deb-src http://archive.ubuntu.com/ubuntu/ focal main restricted
deb http://archive.ubuntu.com/ubuntu/ focal-updates main restricted
deb-src http://archive.ubuntu.com/ubuntu/ focal-updates main restricted
deb http://archive.ubuntu.com/ubuntu/ focal universe
deb-src http://archive.ubuntu.com/ubuntu/ focal universe
deb http://archive.ubuntu.com/ubuntu/ focal-updates universe
deb-src http://archive.ubuntu.com/ubuntu/ focal-updates universe
deb http://archive.ubuntu.com/ubuntu/ focal multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-updates multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-updates multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb-src http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://archive.canonical.com/ubuntu focal partner
deb-src http://archive.canonical.com/ubuntu focal partner
deb http://security.ubuntu.com/ubuntu focal-security main restricted
deb-src http://security.ubuntu.com/ubuntu focal-security main restricted
deb http://security.ubuntu.com/ubuntu focal-security universe
deb-src http://security.ubuntu.com/ubuntu focal-security universe
deb http://security.ubuntu.com/ubuntu focal-security multiverse
deb-src http://security.ubuntu.com/ubuntu focal-security multiverse

EOF

# Backup existing ceph.list
cp /etc/apt/sources.list.d/ceph.list /etc/apt/sources.list.d/ceph.list.bak

# Write new ceph.list
cat <<EOF > /etc/apt/sources.list.d/ceph.list
deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription
deb http://download.proxmox.com/debian/ceph-reef bookworm no-subscription
EOF

# Backup existing pve-enterprise.list
cp /etc/apt/sources.list.d/pve-enterprise.list /etc/apt/sources.list.d/pve-enterprise.list.bak

# Write new pve-enterprise.list
cat <<EOF > /etc/apt/sources.list.d/pve-enterprise.list
deb http://security.debian.org/debian-security bookworm-security main contrib
EOF

# Update package lists
apt-get update

# Upgrade packages
apt-get upgrade -y
# Restart the Proxmox web service
systemctl restart pveproxy.service

# Clear the browser cache note (add instructions for user to follow)
echo "Please clear your browser cache. Depending on the browser, you may need to open a new tab or restart the browser."

```
