# How To Deploy A Shiny App Onto OpenStack

## Step 1: Request Access to OpenStack

Email [E-Research](mailto:e-research@kcl.ac.uk) requesting an OpenStack account and access to the Openstack dashboard

## Step 2: Set Up VPN

Set up your OpenStack VPN following these instructions(https://github.kcl.ac.uk/e-research/rosalind-openvpn)

You will need to sign in using your KCL username and password.

## Step 3: Connect to the VPN

ADD PICTURES

## Step 4: Deploy a Virtual Machine

Access the Openstack dashboard on the following link(https://openstack.rosalind.kcl.ac.uk/dashboard/auth/login/?next=/dashboard/project/).

Make sure to use the latest version of ubuntu.

Depoy a virtual machine following these instructions(https://legacy.rosalind.kcl.ac.uk/doku.php?id=openstack:start)

ADD MORE STEPS

## Step 5: Add a Security Rule for Port 3838

ADD PICTURES

## Step 6: SSH to Your Virtual Machine

## Step 7: Install R on the Virtual Machine

Follow these instructions(https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/)

## Step 8: Install Shiny Server on the Virtual Machine

Follow these instructions([https://linuxize.com/post/how-to-install-r-on-ubuntu-20-04/)

## Step 9: Install the Neccessary Modules on the Virtual Machine

Run this command:
```
sudo apt install build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev net-tools libssl-dev
```

## Step 10: Installing R Packages

It is important to become a super user before installing R packages so that the shiny server can also access the packages:
```
sudo su
sudo -i R
install.packages(c(“shiny”,   "rmarkdown"))
q()
exit
```

## Step 11: Installing R Packages

Restart the shiny server
```
sudo systemctl restart shiny-server
```

Check the default shiny app is running by visiting <Virtual Machine IP Address>:3838
