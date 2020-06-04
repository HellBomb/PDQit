[![Generic badge](https://img.shields.io/badge/Maintained-Yes-Green.svg)](#) [![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)](http://perso.crans.org/besson/LICENSE.html) [![Github all releases](https://img.shields.io/github/downloads/HellBomb/PDQit/total.svg)](https://GitHub.com/HellBomb/PDQit/releases/) [![GitHub release](https://img.shields.io/github/release/HellBomb/PDQit.svg)](https://GitHub.com/HellBomb/PDQit/releases/)

# PDQ IT
I have been using PDQ for about 2 years now but it took only days to fall in love with it. Its easy to use interface and powerful functionality makes it hands down one of the best tools in any windows administrators toolbox for system administrators of all skill levels. We have been able to combine PDQ Deploy, PDQ Inventory, and Active Directory to completely automate application installations based on Active Directory Security groups.

## Before We Get Started
I will be keeping this repo up to date with the latest versions of my PDQ Packagages but those may not represent the latest versions of software. With how easily it is to download the latest version of software and point it to the latest file I will leave making those updates to you.

## Creating Queries

## Setting Up The Framework


### Create Active Directory Groups
Individual security groups need to be made for every application package you want automatically deployed, then simply add each of the computers to that group. 

Sources:
     (1) How to create a group account: https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-a-group-account-in-active-directory
     (1) Understanding different AD groups: https://docs.microsoft.com/en-us/windows/security/identity-protection/access-control/active-directory-security-groups

### Create Package Schedules
Once all packages have been created or imported a schedule needs to be created and attached to automate the deployment. Create a new schedule, set the desired trigger schedule, then for the target LINK to the PDQ Inventory Collection for the package application. 

My Preferred Settings

&ensp;Triggers:__

&ensp;&ensp;Interval (Every 1 hour starting at 1/1/2000 12:00 AM)\

&ensp;Options:\

&ensp;&ensp;Schedule Enabled: Yes (Checked)  

&ensp;&ensp;Credentials: (Default)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Use PDQ Inventory Scan User credentials first, when available: No (Unchecked)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Run As: (use package settings)(varies)
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Notifications: 
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Stop deploying to targets once they succeed: Yes (Checked)
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Stop deploying to targets if they fail xx time(s): Yes (Checked), 1 time [1]
    
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Stop deploying to remaining queued targets after xx minutes: yes (Checked), 30 [2]
    

  Offline Settings
    Offline Status
      Use settings from package(s): Yes (Checked)
      Ping before deployment: Yes (Checked)
      Send Wake-on-LAN and attempt deployment: Yes (Checked)
    Retry Queue
      Use settings from package(s): Yes (Checked)
      Put offline targets in Retry Queue: No (Uncecked) [3]

Reasoning & Justification
  [1] A schedule will not trigger again until it has completed successfully so if a few computers are failing over and over again new computers that need the application will not get it until the schedule completes.
  [2][3] Essentially the same as [1], you just want to keep the schedule triggering as desired.
  
Sources:
     (1) Create Schedules: https://documentation.pdq.com/PDQDeploy/13.0.3.0/index.html?manage-schedules.htm
