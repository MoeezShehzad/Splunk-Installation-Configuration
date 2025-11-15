# Splunk-Installation-Configuration
The objective of this task is to assist everyone in installing and configuring Splunk on an Ubuntu machine. By completing this task, students will have Splunk up and running, enabling them to collect and analyze security logs.
# Lab Setup
#Requirements
  System: Ubuntu 22.04 / 20.04 (Server or Desktop)
  Tools Required:
  Splunk Enterprise (Free version for local setup)
  Terminal (Command Line Access)

# Steps to Install and Configure Splunk on Ubuntu
      1. Open Terminal and download Splunk using wget:
      wget -O splunk-9.3.0-51ccf43db5bd-linux-2.6-amd64.deb "https://download.splunk.com/products/splunk/releases/9.3.0/linux/splunk-9.3.0-51ccf43db5bd-linux-2.6-amd64.deb" 
  # Once downloaded, install Splunk:
      sudo dpkg -i splunk-ubuntu.deb
      
# Step 2: Enable Splunk as a Service
      Move to the Splunk installation directory:
      cd /opt/splunk/bin
# Accept the license agreement and enable Splunk at boot:
      sudo ./splunk enable boot-start --accept-license
# Start Splunk:
      sudo ./splunk start
# When prompted, set up an admin username and password.
# Step 3: Access Splunk Web Interface
     Open a web browser and go to:
     http://<your-server-ip>:8000

# Log in with the admin credentials created earlier.

# Splunk Universal forwarder 
  Only Splunk is not enough if you want to get real-time threats from the operating systems you want to monitor; then you have to install Splunk Universal Forwarder on the endpoint systems.
  
# For Debian OS's 
wget -O splunkforwarder-9.3.0.deb "https://download.splunk.com/products/universalforwarder/releases/9.3.0/linux/splunkforwarder-9.3.0-51ccf43db5bd-linux-2.6-amd64.deb"

You can download it from here for Debian.

# Step 2:
 Then run the following command
 sudo dpkg -i splunkforwarder.deb
 
# In case there are dependency errors, run the following Command
    sudo apt --fix-broken install -y

# Step 3: Start it and accept Liscence
    sudo /opt/splunkforwarder/bin/splunk start --accept-license

# Step 4: Enable Boot-Start
    sudo /opt/splunkforwarder/bin/splunk enable boot-start

# Step 5: Configure forwarding to your Splunk Enterprise indexer (replace INDEXER_IP and port 9997):
    sudo /opt/splunkforwarder/bin/splunk add forward-server INDEXER_IP:9997 -auth admin:YourAdminPass

# Step 6: Add data inputs (for example monitor logs) by editing /opt/splunkforwarder/etc/system/local/inputs.conf:

    [monitor:///var/log/syslog]
    disabled = false
    sourcetype = linux:syslog
    index = platform_logs

# Step 7: Restart the forwarder
    sudo /opt/splunkforwarder/bin/splunk restart

# Final: On Splunk Enterprise server: confirm that it’s receiving data (Settings → Forwarding & Receiving → Received data).
