# How To Deploy A Shiny App Onto OpenStack

## Step 1: Request Access to OpenStack

Email [E-Research](mailto:e-research@kcl.ac.uk) requesting an OpenStack account and access to the Openstack dashboard

## Step 2: Set Up VPN

Set up your OpenStack VPN following these [instructions](https://docs.er.kcl.ac.uk/CREATE/tools/openvpn/)

You will need to sign in using your KCL username and password.

## Step 3: Connect to the VPN

## Step 4: Create a new Security Group that allows internet traffic

Add a security group with these rules:

<img width="768" alt="image" src="https://user-images.githubusercontent.com/68480973/229761921-9457f6dc-bbdf-4dca-ab0a-c08f8f8da700.png">

Details on how to do this can be found [here](https://github.kcl.ac.uk/pages/maudsley-brc-cti/drive-health-hpc-training/cloud/exercises/)

## Step 5: Deploy a Virtual Machine

Access the Openstack dashboard on the following [link](https://cloud.er.kcl.ac.uk/project/).

You will need to sign in using your KCL username and password.

Make sure to use the latest version of ubuntu.

Depoy a virtual machine following these [instructions](https://docs.er.kcl.ac.uk/CREATE/cloud/launch_instance/)

Associate it with the external network:
<img width="719" alt="image" src="https://user-images.githubusercontent.com/68480973/229762396-287554b5-af79-4744-b818-2d32349f51f7.png">

Only add the security group you created (the default one will prevent all traffic). 

Documentation can be found [here](https://github.kcl.ac.uk/pages/maudsley-brc-cti/drive-health-hpc-training/cloud/exercises/)

Additional CREATE OpenStack documentation can be found [here](https://docs.er.kcl.ac.uk/CREATE/cloud/web_interface/)

## Step 7: SSH to Your Virtual Machine

Change directory to the directory containing the key e.g.:
```cd C:\Users\guypw\OneDrive\Documents\createOpenStack```

ssh into the virtual machine e.g.:
```ssh -i createOpenStackKey.pem ubuntu@10.211.114.16```

## Step 8: Install R on the Virtual Machine

Follow these [instructions](https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/)

Make sure you get the latest version.

```sudo apt-get update```
```sudo apt-get upgrade```

## Step 9: Install Shiny Server on the Virtual Machine

Follow these [instructions](https://www.rstudio.com/products/shiny/download-server/ubuntu/)

## Step 10: Install the Neccessary Modules on the Virtual Machine

Run this command:
```
sudo apt-get install build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev net-tools libssl-dev libz-dev gfortran liblapack-dev libopenblas-dev libpng-dev libjpeg-dev
```

## Step 11: Installing R Packages

It is important to become a super user before installing R packages so that the shiny server can also access the packages:
```
sudo su
sudo -i R
install.packages(c("shiny",   "rmarkdown"))
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
Create a new folder called "app" in the users home directory (/home/ubuntu/).

Upload your code to the /home/ubuntu/app directory
  
You can either use a text editor to create an app.R file and upload your code or upload your code via a file sharing system.

Update the shiny config file permissions to allow you to edit it.

```
sudo chmod 7777 -R /etc/shiny-server
```

Update the shiny config file (/etc/shiny-server/shiny-server.conf) to your apps location.

```vim /etc/shiny-server/shiny-server.conf```

You can replace the contents of the config file with this:
run_as shiny;
http_keepalive_timeout 300;
sockjs_heartbeat_delay 10;
sockjs_disconnect_delay 300;
disable_websockets TRUE;
app_init_timeout 300;
app_idle_timeout 300;

# Define a server that listens on port 3838
server {
  listen 3838;

  # Define a location at the base URL
  location / {

    # Host the directory of Shiny Apps stored in this directory
    site_dir /home/ubuntu/app;

    # Log all Shiny output to files in this directory
    log_dir /var/log/shiny-server;

    # When a user visits the base URL rather than a particular application,
    # an index of the applications available in this directory will be shown.
    directory_index on;
  }
}

  
After uploading your code ensure all the neccessary R packages are installed (as a super user) and then restart the shiny server.

'''
sudo systemctl restart shiny-server
'''
 
Check your shiny app is running by visiting <Virtual Machine IP Address>:3838
