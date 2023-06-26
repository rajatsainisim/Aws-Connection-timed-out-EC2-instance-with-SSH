# Aws-Connection-timed-out-EC2-instance-with-SSH
I'm receiving "Connection refused" or "Connection timed out" errors when trying to connect to my EC2 instance with SSH. How do I resolve this?


Follow these steps to configure user-data for the instance:

1.    Open the Amazon EC2 console.

2.    Choose Instances from the navigation pane, and then select the instance that you plan to connect to.

3.    Stop the instance.

4.    Choose Actions, Instance Settings, Edit User Data.

5.    Copy the following user data script into the Edit User Data dialog box, and then choose Save.


Content-Type: multipart/mixed; boundary="//"
MIME-Version: 1.0
 
--//
Content-Type: text/cloud-config; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="cloud-config.txt"
 
#cloud-config
cloud_final_modules:
- [scripts-user, always]
 
--//
Content-Type:
    text/x-shellscript; charset="us-ascii"
MIME-Version: 1.0
Content-Transfer-Encoding: 7bit
Content-Disposition: attachment; filename="userdata.txt"
 
#!/bin/bash
iptables -P INPUT ACCEPT
iptables -F
systemctl restart sshd.service || service sshd restart
if [[ $( cat /etc/hosts.[ad]* | grep -vE '^#' | awk 'NF' | wc -l) -ne 0 ]];\
then sudo sed -i '1i sshd2 sshd : ALL: allow' /etc/hosts.allow; fi
--//

6.  Start the Instance
7.  Connect to the instance using SSH.
