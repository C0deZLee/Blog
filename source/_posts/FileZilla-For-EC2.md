title: 'Use FileZilla to connect to Amazon EC2 instance'
categories:
- Guide
date: 2015-10-18 01:15:08
tags:
- Cloud
- AWS
---
The FileZilla is a useful tool to upload/download files to/from Amazon EC2, and here is a brief guide of how to use FileZilla.

## Download
[Download Link](https://filezilla-project.org/download.php?type=client)
You can download the FileZilla from the link above, FileZilla provides Windows, Mac and Linux versions.
<!--more-->
## Setup
#### Step 1
EC2 instances use **.pem** files to connect instead of username/password, so the first step is import your **.pem** to FileZilla.
Open Settings -> SFTP -> Add key file, import your **.pem** file.
![](/images/filezilla/step1.png)
#### Step 2
Open File -> Site Manager.
Enter the parameters as followings:
**Host**: EC2 instance public DNS
**Protocol**: SFTP
**Logon Type**: Normal
**User**: ec2-user
![](/images/filezilla/step2.png)
Then click connect
#### Step 3
Now we have connected to EC2, to upload files to your remote server, just drag files to desired directory.
![](/images/filezilla/step3.png)
