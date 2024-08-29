We download and install the Virtual Box program
Download and install Vagrant

Open a terminal and make sure Vagrant is running: Powershell can be used as a terminal.
C:\> vagrant -v

We create a "Vagrant" folder on the C:\ drive

Then we search for the image of the machine we want to download and run in the portal of vagrant and execute it in the CLI
C:\> vagrant init [ geerlingguy/centos7 ]

After that, a .vagrantfile will be created in the "Vagrant" folder on the C:\ drive.

You open the .vagrantfile and change the necessary locations

C:\> vagrant up
This command automatically creates and configures a virtual machine if it does not already exist, or simply starts it if it already exists.












