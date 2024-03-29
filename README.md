# How To Deploy A Shiny App Onto OpenStack

## Step 1: Request Access to OpenStack

Email [E-Research](mailto:e-research@kcl.ac.uk) requesting an OpenStack account and access to the Openstack dashboard

## Step 2: Set Up VPN

Set up your OpenStack VPN following these [instructions](https://docs.er.kcl.ac.uk/CREATE/tools/openvpn/)

You will need to sign in using your KCL username and password.

## Step 3: Connect to the VPN

## Step 4: Create a new Security Group that allows internet traffic

Access the Openstack dashboard on the following [link](https://cloud.er.kcl.ac.uk/project/).

You will need to sign in using your KCL username and password.

Add a security group with these rules:

<img width="842" alt="image" src="https://user-images.githubusercontent.com/68480973/232744822-d495ba7f-c568-49c3-847f-2e33dad88ae7.png">

Details on how to do this can be found [here](https://github.kcl.ac.uk/pages/maudsley-brc-cti/drive-health-hpc-training/cloud/exercises/)

## Step 5: Deploy a Virtual Machine

Make sure to use the latest version of ubuntu.

Depoy a virtual machine following these [instructions](https://docs.er.kcl.ac.uk/CREATE/cloud/launch_instance/)

Associate it with the external network:

<img width="719" alt="image" src="https://user-images.githubusercontent.com/68480973/229762396-287554b5-af79-4744-b818-2d32349f51f7.png">

Only add the security group you created. 

Documentation can be found [here](https://github.kcl.ac.uk/pages/maudsley-brc-cti/drive-health-hpc-training/cloud/exercises/)

Additional CREATE OpenStack documentation can be found [here](https://docs.er.kcl.ac.uk/CREATE/cloud/web_interface/)

## Step 6: SSH to Your Virtual Machine

Change directory to the directory containing the key e.g.:
```cd C:\Users\guypw\OneDrive\Documents\createOpenStack```

ssh into the virtual machine e.g.:
```ssh -i createOpenStackKey.pem ubuntu@10.211.114.16```

## Step 7: Install R on the Virtual Machine

Follow these [instructions](https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/)

Make sure you get the latest version.

```sudo apt-get update```

## Step 8: Install Shiny Server on the Virtual Machine

Follow these [instructions](https://www.rstudio.com/products/shiny/download-server/ubuntu/) from Step 3 onwards. 

## Step 9: Install the Neccessary Modules on the Virtual Machine

Run this command:
```
sudo apt-get install build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev net-tools libssl-dev libz-dev gfortran liblapack-dev libopenblas-dev libpng-dev libjpeg-dev cmake liblzma-dev python-dev libbz2-dev libclang-dev libfontconfig1-dev libharfbuzz-dev libfribidi-dev libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev
```

## Step 10: Installing R Packages

It is important to become a super user before installing R packages so that the shiny server can also access the packages:
```
sudo su
R
install.packages(c("shiny",   "rmarkdown"))
q()
exit
```

Restart the shiny server
```
sudo systemctl restart shiny-server
```

Check the default shiny app is running by visiting "Virtual Machine IP Address":3838

e.g. http://10.211.116.107:3838/

## Step 11: Install Your Shiny App
Create a new folder called "app" in the users home directory (/home/ubuntu/).

Upload your code to the /home/ubuntu/app directory

Update the directories permissions:

```
sudo chmod 7777 -R /home/ubuntu/app/
```
  
You can either use a text editor to create an app.R file and upload your code or upload your code via a file sharing system.

Install your packages as a super user e.g.

```
sudo su
R
install.packages(c('shiny','ggplot2','shinydashboard','knitr','rmarkdown','ggthemes','plotly','DT','data.table','dplyr','shinythemes','RColorBrewer','markdown','shinycssloaders','MASS','stringr','ggpubr','rlang','shinybusy'))
q()
exit
```

Update the shiny config file permissions to allow you to edit it.

```
sudo chmod 7777 -R /etc/shiny-server
```

Update the shiny config file (/etc/shiny-server/shiny-server.conf) to your apps location.

```vim /etc/shiny-server/shiny-server.conf```

You can replace the contents of the config file with this:
```
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
```

  
After uploading your code ensure all the neccessary R packages are installed (as a super user) and then restart the shiny server.

```
sudo systemctl restart shiny-server
sudo systemctl status shiny-server
```
 
Check your shiny app is running by visiting "Virtual Machine IP Address":3838

## Step 12: Optionally you can set up a script to restart the shiny server everyday

Create a folder for your shell script in your app folder e.g. 

```
mkdir /home/ubuntu/app/shellScripts
```

Create a shell script:

```
cd /home/ubuntu/app/shellScripts
vim dailyProcess.sh
```

Add the following contents to the shell script file:

```
#!/bin/sh

sudo systemctl restart shiny-server
echo "Shiny restart complete"
```

Create a new daily job:

```
crontab -e
```

Add the following line, this will set the shell script to run every day at 23:55:

```
55 23 * * * /home/ubuntu/app/shellScripts/dailyProcess.sh
```

You can test your shellscript with this command:

```
sh /home/ubuntu/app/shellScripts/dailyProcess.sh
```

You can test the cron job with this command:
```
run-parts /etc/cron.daily
```

## Step 13: Requst a public facing URL from the CREATE admins

Email [E-Research](mailto:e-research@kcl.ac.uk) requesting a public facing URL with the details in the Excel below (the Excel contains several examples):

[openStackMigrationDetails.xlsx](https://github.com/guypwhunt/deployShinyAppOnOpenstack/files/11271192/openStackMigrationDetails.xlsx)
