![GHOSTS Logo](https://github.com/cmu-sei/GHOSTS/blob/master/img/ghosts-logo.jpg)

Copyright 2017 Carnegie Mellon University. All Rights Reserved. See LICENSE.md file for terms.

# GHOSTS NPC AUTOMATION

GHOSTS is a framework for highly-complex, realistic non-player character (NPC) orchestration. It essentially realistically mimics the behavior of the different types of people you might encounter on any array of different typical office or enterprise networks. The system makes it possible for cybersecurity experts to test their skills and realistically train to defend real networks with real NPC players operating on those networks doing the things we might expect them to do: Create documents, access systems, browse the web, click, run commands, and so on.

As a result of the system checks required in order for NPCs to be situationally aware, GHOSTS also does health reporting for all configured clients on a given instance.

## Key Links

[Installation and configuration information is maintained on our wiki](https://github.com/cmu-sei/GHOSTS/wiki)

[Don't hesitate to submit issues and feature requests here](https://github.com/cmu-sei/GHOSTS/issues)

## Platform Components

### Ghosts.Client (Windows)
.NET Console app (but built as forms app so that it is hidden) - requires .NET framework v4.6.1 or higher. Client works on both Windows 7 and Windows 10.

### Ghosts.Client (Linux)
dotnetcore app built to run silently. Client tested on centos, alpine and kali distributions. We typically use this for red teaming and "outside" traffic generation or administration simulation.

### Ghosts.Api
.NET Core 2.0 MVC Code-First containing both the api calls for the client (and corresponding api calls the ui needs) in one. 

Uses postgres on the backend because there is not much that postgres can't do.

## LEGAL

[DISTRIBUTION STATEMENT A] This material has been approved for public release and unlimited distribution.

I have forked this from https://github.com/cmu-sei/GHOSTS. 

I take no credit for anything except trying to dumb down the instructions so more people can enjoy this program. If you are like me, and are intrested in this project but are not a devoloper and have much expierence with things like docker or github. This may be the place for you. 


I have learned a lot on this journey. Thank you for the opportunity and for taking the time to respond. I have spent the last few days putting the pieces together, hopefully I am not to far off. I was able to get the clients to talk to the API.

Yes, that helped me out a lot. I hope you don't mind but I took the liberty of trying to pay it forward. please feel free to delete this if it is not acceptable or not the right place. I most certainly hope some one will correct me if i made any mistakes. That being said here we go, this is what i was able to put together from the different pieces and resources provided on this site (here and there).

Range Staging Environment
I take no credit for this I am simply attempting to repay my gratitude in having the chance to work with this wonderful set of tools. All credit goes to sei-dupdyke aka Dustin Updyke.

First steps

Ready the range development resources
Build the 3 Vms 1 Windows 7 client, 1 Windows 10 client , 1 Ubuntu 16.04 Desktop API (minumum 1 windows box and one Ubuntu box)
Work in the clones keep the Vanilla pure for later use. (you will thank me later if you decide to add more clients, simply clone the vanilla VM's and modify the clones as it takes for ever for the windows updates to install).
Installing the API from Linux to Ubuntu Desktop 16.04

See here for the original instructions https://github.com/cmu-sei/GHOSTS/wiki/Installation-from-distribution-binaries

Getting the environment ready.

Make sure your Ubuntu Desktop 16.04 is updated, open up a teminal
sudo apt-get update && sudo apt-get upgrade -y

Install git
sudo apt install git -y

Install curl
sudo apt-get install curl -y

Install docker (Step 1 only)
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04

Install docker-compose version 1.21.2, build a133471 (again you only should need step one)
https://www.digitalocean.com/community/tutorials/how-to-install-docker-compose-on-ubuntu-18-04

Installing the API

Open up a terminal and navigate to your desktop (cd Desktop)
$ sudo git clone https://github.com/cmu-sei/GHOSTS.git
$ sudo chown -R ghosts:ghosts GHOSTS/ (this assumes your username is ghost)
$ sudo chmod -R 777 GHOSTS
$ cd GHOSTS
$ cd Ghosts.Api
$ sudo docker-compose up -d
go to http://localhost:5000/api/home to verify the API is up and running (take note of the API, you will be looking for a change later, the default state of 3 NPC’s, 0 Activities, 0 Polling, and 0 Health.
Next step is to configure the Grafana
https://github.com/cmu-sei/GHOSTS/wiki/Configuring-Grafana-Reporting

l. Log into Grafana at http://localhost:3000
2. login – username admin password admin
3. You will be prompted to change your password

First step is to setup a datasource named "ghosts" to the ghosts postgres database.

Click data source
Scroll down and click PostgreSQL
Leave the default database name or change it (doesn't matter but remeber it for trouble shooting )
In the host field enter :5432 (for this demo it was the first docker NIC 172.19.0.1)
In the database field enter ghosts
In the User field enter - "ghosts" with the Password - "scotty@1"
Disable ssl
Scroll down and click save and test
Assuming no errors click the icon in the top left to proceed to the next step
Now import the Grafana json file in this repository. It creates the default GHOSTS dashboard.

Hover over the plus sign + and select import
Click Upload .json file
Navigate to /home/ghosts/Desktop/GHOSTS and select GHOSTS-default Grafana dashboard.json*
On the Unique identifier (uid) field select check the box to enable a UID
On the field ghosts click the drop down and select PostgreSQL (this is the name of the database you selected earlier)
Press select and your API should be up and running, you should see that beautiful dashboard
Next move on to the clients, this guide will cover the Windows machines first

Choose either Windows 7 or Windows 10 as that is all this guide has tested and they both have the same install process.
Install ALL updates (the specific one thats is needed is listed on Github)
Install Microsoft office for the NPC’s to interject with
Your client Windows machine will need to have at least Microsoft DotNet 4.7.x runtime installed. Note that you only need the runtime, not the full SDK.
Click here to download the current client zip file. Unzip to your client machine in a directory such as c:\exercise\ghosts. You will need to adjust configuration in ./config/application.json in order for your client to talk to your previously installed API server above. (for this to work the string in the ./config/application.json file needed to be edited, you can use notepadd++, from "IdUrl": "http://telemetry.exercise.mil/api/clientid", to "IdUrl": "http://:5000/api/clientid", it also seems that while using vbox the networks on the VM's required to be in bridged mode.)
Navigate to the folder you just downloaded and installed, locate the ghosts.exe and run it
Verify the API shows a change from the default state of 3 NPC’s, 0 Activities, 0 Polling, and 0 Health.
I hope this helps, good luck and good hunting.
