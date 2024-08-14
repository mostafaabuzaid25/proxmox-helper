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
deb http://security.debian.org/debian-security bookworm-security main contrib
deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription
deb http://download.proxmox.com/debian/ceph-reef bookworm no-subscription
EOF

# Backup existing ceph.list
rm /etc/apt/sources.list.d/ceph.list



# Update package lists
apt-get update

# Upgrade packages
apt-get upgrade -y

# Clear the browser cache note (add instructions for user to follow)
echo "Please clear your browser cache. Depending on the browser, you may need to open a new tab or restart the browser."
# Restart the Proxmox web service
systemctl restart pveproxy.service
