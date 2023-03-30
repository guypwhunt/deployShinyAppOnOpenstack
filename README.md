# How To Deploy A Shiny App Onto OpenStack

## Step 1: Request Access to OpenStack

Email [E-Research](mailto:e-research@kcl.ac.uk) requesting an OpenStack account and access to the Openstack dashboard

## Step 2: Set Up VPN

Set up your OpenStack VPN following these [instructions](https://docs.er.kcl.ac.uk/CREATE/tools/openvpn/)

You will need to sign in using your KCL username and password.

## Step 3: Connect to the VPN

## Step 4: Set Up the Network Infastructure

Create a new network.

Create a router connecting the external network to your new network.

Details on how to do this can be found [here](https://github.kcl.ac.uk/pages/maudsley-brc-cti/drive-health-hpc-training/cloud/exercises/)

## Step 5: Deploy a Virtual Machine

Access the Openstack dashboard on the following [link](https://cloud.er.kcl.ac.uk/project/).

You will need to sign in using your KCL username and password.

Make sure to use the latest version of ubuntu.

Depoy a virtual machine following these [instructions](https://docs.er.kcl.ac.uk/CREATE/cloud/launch_instance/)

Allocate a floating IP address.

Additional CREATE OpenStack documentation can be found [here](https://docs.er.kcl.ac.uk/CREATE/cloud/web_interface/)

## Step 6: Add a Security Rule for Port 3838

## Step 7: SSH to Your Virtual Machine

## Step 8: Install R on the Virtual Machine

Follow these [instructions](https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/)

## Step 9: Install Shiny Server on the Virtual Machine

Follow these [instructions](https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/)

## Step 10: Install the Neccessary Modules on the Virtual Machine

Run this command:
```
sudo apt install build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev net-tools libssl-dev
```

## Step 11: Installing R Packages

It is important to become a super user before installing R packages so that the shiny server can also access the packages:
```
sudo su
sudo -i R
install.packages(c(“shiny”,   "rmarkdown"))
q()
exit
```

## Step 12: Installing R Packages

Restart the shiny server
```
sudo systemctl restart shiny-server
```

Check the default shiny app is running by visiting <Virtual Machine IP Address>:3838
  

## Step 13: Install Your Shiny App
Change Directory, become a super user, delete the content of the folder and change folder permissions.

```
cd /srv/shiny-server
sudo su
rm /srv/shiny-server/*
chmod 7777 -R /srv/shiny-server
```
   
You can either use a text editor to create an app.R file and upload your code or upload your code via a file sharing system.
  
After uploading your code ensure all the neccessary R packages are installed (as a super user) and then restart the shiny server.

```
sudo systemctl restart shiny-server
```
 
Check your shiny app is running by visiting <Virtual Machine IP Address>:3838
  
## Step 14: Optional Step - Increase the Shiny Server Time Out
  
Open the shiny-server.conf file.
  
```
sudo vi /etc/shiny-server/shiny-server.conf
```

Add the following lines directly below under “run_as shiny”:
  
```
http_keepalive_timeout 300;
sockjs_heartbeat_delay 10;
sockjs_disconnect_delay 300;
disable_websockets TRUE;
app_init_timeout 300;
app_idle_timeout 300;
```
Save the file and restart the shiny server
```
sudo systemctl restart shiny-server
```
