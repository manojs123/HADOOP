# ﻿STEPS TO SET UP A MULTI NODE  CLOUDERA CLUSTER IN CLOUD (Google Cloud):


### STEP 1 : 
Go to `Google Console`  and log in with your user credentials and fill out all the required details. /* It ask for your payment details, don’t panic it won’t charge without your permission*/ (Check Screenshot 1 ) 


### STEP 2 :
Once you have created your account , click  `COMPUTE ENGINE` which you can find on the left hand side (Screenshot 2 ). 
It shows you a Dialog box saying `VM Instances` (Screenshot 3 & 4).
Click on create, it takes sometime.


### STEP 3 :
Now you can create your instances `(Cluster nodes)` (Screenshot 5).
Give name for your instance (I gave like Node1), Zone, Machine Type like number of CPU like 1 vCPU or  2 vCPU or 4 vCPU or 8 vCPU (I took 4 vCPU), Boot Disk (Type of OS, Screenshot 6 ) & give the size ( I gave like 50GB). 


Once you have done it creates your first instance (Screenshot 7 & Screenshot 8).


Likewise create as number of instances as you ( I have created like 3 instances with names node1,node2,node3) ( Screenshot 9 and Screenshot 10).


### STEP 4 :
After you have created all the instances (nodes) you want. Click on the `SSH` (Screenshot 11 & 12).
Once you have click on that SSH, you can see terminals for all the instances you have created (Screenshot 13 for my first instance i.e,. node1).


### STEP 5 :
Now generate Public Keys from `root (“sudo su”)` for all the instances using ``` vi ssh-keygen``` (Screenshot 14).
Your generated public key is located : (Screenshot 15).


### STEP 6 :
Now create password less SSH for all the instances.
Place your public key of node1 in remaining nodes and vice versa.
Run the following command in your master node (First Instance i.e node1):
 ``` vi 
cat .ssh/id_rsa.pub/ssh yournodename ‘cat >> ~/.ssh/authorized_keys’
```
`For My node Example`
``` vi 
(cat .ssh/id_rsa.pub/ssh node2 ‘cat>>~/.ssh/authorized_keys’)
(cat .ssh/id_rsa.pub/ssh node3 ‘cat>>~/.ssh/authorized_keys’)```


The above commands place node1 public keys in node2 and node .


### STEP 7 :
If you get error like “Permission Denied” or “Failure”  in STEP 6 
You need to change the Permission as Google Cloud doesn’t allow to grant access from root. 
``` vi 
Go to : vi /etc/ssh/sshd_config
```  and make following changes :
Change `PasswordAuthentication` to Yes and `PermitRootLogin` to Yes 
(Screenshot 16 and Screenshot 16(a)).


As you have changed properties you need to restart your service.
Run : service sshd restart (Screenshot 17).


Now repeat STEP 6, bingo! we could successfully create passwordless SSH (Screenshot 18).


### STEP 8 :
Now , install HTTPD from your master node for all your remaining nodes.

``` vi 
        Yum -y install httpd 
```

For remaining nodes run : ssh nodename `yum -y install httpd`  (As we have passwordless SSH, we can run it from our main/master node).



### STEP 9 :
After successfully installing HTTPD in STEP 8 in all nodes , we need to switch off `FIREWALL SERVICE` using :
ssh nodename 
``` vi 
service firewalld stop && chkconfig firewalld off
``` 
`Ex : ssh node1 “service firewalld stop && chkconfig firewalld off” `
Do this for all your nodes (Screenshot 19).
You can check whether Firewall is stopped or not using : service firewalld status (Screenshot 20 & 20(a)).


### STEP 10 :
After switching off firewall, start HTTPD, using :

``` vi
service httpd start 
chkconfig httpd on
```

Do this for all remaining nodes from master/main node :


`ssh nodename “service httpd start && chkconfig httpd on”&&`
EX : `ssh node2 “service httpd start && chkconfig httpd on”&&`


(& : Making that process work background so that we can continue our process).




### STEP 11 :
Now disable `SELINUX.`
To disable SELINUX, go to →` vi /etc/sysconfig/selinux` (Screenshot 21 & 21(a)).


Now `REBOOT` your master node. (Just type reboot and press enter).




### STEP 12 :


After completion of step 11, now go back to your Google Cloud Platform i.e., google console. 
Go to `NETWORKINGS` (Screenshot 22).
Go to `Firewall Rules` , under Networking (LEFT HAND SIDE) (Screenshot 23).
Now create a Firewall Rule (Screenshot 23 & 24).
Give firewall name, and under “Allowed Protocols & ports” dialog box open all TCP ports by “tcp:0-65000” (Screenshot 24 & 24(a)).
STEP 13 :
Now install wget on master node by using : `yum -y install wget`.


### STEP 14 :
Now Follow the remaining steps from following link :


CLOUDERA INSTALLATION FURTHER STEPS


 i.e,. :


Download the installer by (from Master Node):
``` vi
wget https://archive.cloudera.com/cm5/installer/latest/cloudera-manager-installer.bin 
```

Now run the following command : 
``` vi
chmod u+x cloudera-manager-installer.bin
```
A dialog box is popped up asking for confirmation accept the agreement and press next (Screenshot 25)
Cloudera is installing successfully on your machine (Screenshot 26).


## FINAL STEPS :
Once the installation is finished, go to google tab and give you master node external IP (Screenshot 27).
Now a screen showing with the details of your respective OS. As i took REHL I got details corresponding to it (Screenshot 28 & 28(a)).
Now enter your external ip : 7180 and press enter (Screenshot 29 ).


Congrats ! Cloudera is Successfully installed on your machine. You can see Cloudera Login page (Screenshot 30 ).
Give username and Password as admin, admin and press enter. You can see next screen installing your nodes (Screenshot 31 & 31(a)).
Follow the further steps i.e,. Accept license and press continue. 
“MULTI NODE CLOUDERA CLUSTER IS INSTALLED”
