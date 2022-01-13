---  

layout: post 
title:  "ubuntu快照工具之timeshift" 
categories: [linux] 
tags: [ubuntu]  

---

ubuntu快照工具之timeshift
A simple way to use this tool:

Install the timeshift
sudo add-apt-repository -y ppa:teejee2008/timeshift
sudo apt-get update
sudo apt-get install timeshift
 

Create a snapshot
sudo timeshift --create
------------------------------------------------------------------------------
Creating new snapshot...(RSYNC)
Saving to device: /dev/sda1, mounted at path: /
Linking from snapshot: 2021-05-21_15-00-01
Synching files with rsync...
Created control file: /.../code/timeshift/snapshots/2021-05-24_18-20-45/info.json
RSYNC Snapshot saved successfully (16s)
Tagged snapshot '2021-05-24_18-20-45': ondemand
------------------------------------------------------------------------------
List snapshots
sudo timeshift --list
Device : /dev/sda1
UUID   : a63e7cf3-f4b2-409e-9e6e-2e75d94b266f
Path   : /home/beyond/backup
Mode   : RSYNC
Status : OK
4 snapshots, 673.8 GB free
 
Num     Name                 Tags  Description
------------------------------------------------------------------------------
0    >  2021-04-09_15-00-01  M
1    >  2021-04-23_15-00-02  W
2    >  2021-05-21_15-00-01  W
3    >  2021-05-24_18-20-45  O
Restore OS with a snapshot
sudo timeshift --restore
 
Select snapshot:
 
Num     Name                 Tags  Description
------------------------------------------------------------------------------
0    >  2021-04-09_15-00-01  M
1    >  2021-04-23_15-00-02  W
2    >  2021-05-21_15-00-01  W
3    >  2021-05-24_18-20-45  O
 
Enter snapshot number (a=Abort, p=Previous, n=Next): 3
 
 
 
******************************************************************************
To restore with default options, press the ENTER key for all prompts!
******************************************************************************
 
Press ENTER to continue...
For more details, check https://github.com/teejee2008/timeshift