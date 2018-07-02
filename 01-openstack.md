# Openstack

## Cloud

Openstack is a cloud system. The term *Cloud* is used for systems which try to extract computers or servers
into abtract objects. In the old world we used to talk about servers or virtual machines. With virtual machines
we try to mimic a server or a computer. In a Cloud we talk about *instances* which is a combination of
* virtual cpu's
* network interfaces
* memory
* root storage
* (disk) volumes
* security templates (firewall)
Scripting is imported in cloud systems. In general instances are fully configured by script. If a instance has a problem,
normally a new instance is created and the defect is deleted since creating a new one is fast.

## Openstack Web interface (Horizon)

<img align="left" src="images/2018-06-29-140834_221x690_scrot.png" size="75%"><img/>

The webaddress is https://stack.naturalis.nl. You can log in with your provided credentails for openstack. On the left are the elements
of openstack. You have the global groups: 
* Project 
* Admin 
* Identity 
* Applications

For most users only the group **Project** is used.
The Project group has: 
* Compute
* Network
* Orchestration
* Data Processing 
* Object store. 

Here only **Compute** is really important.

### Compute

* Overview: You can find a global overview of your resources
* Instances: A list of all your running instances
* Volumes: A list of all your volumes
* Images: A list of all your images of instances
* Access & Security: Here you can configure firewall and access keys

<br></br>
<br></br>

## Create access credentials with Putty

Access of to a linux server is done by a *ssh keypair*. In on mac and linux systems this is native. For windows
you have to install Putty. You also need puttygen.

Putty: https://the.earth.li/~sgtatham/putty/latest/w64/putty.exe

Puttygen: https://the.earth.li/~sgtatham/putty/latest/w64/puttygen.exe

### Generate ssh keypair in puttygen

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/-92wEg68SKQ/0.jpg)](https://www.youtube.com/watch?v=-92wEg68SKQ)

Now we are going to import the public key to your openstack environment. Go to **Access & Security** and then to 
**Key Pairs** and then go for **Import Keypair** and copy your public key in the box. Give it a name (something with
our name in it is handy).

We are going to use this later

## Setup firewall (security groups)

Openstack comes with its own firewall. Each firewall rule is called a **security rule** and they are grouped
together in a **security group**. A security group is then applied to an instance.

We are going to create a firewall rule which allows access to the instance via SSH.
Go to **Access & Security** and click on **Create Security Group**
![overview-a_and_s](images/2018-06-29-113713_1691x285_scrot.png)
Give it a name (for example, access for `<your name>` development-server)
![create-new-group](images/2018-06-29-113725_721x334_scrot.png)

Now go for **Add Rule**
![add-new-rule](images/2018-06-29-113743_1669x208_scrot.png)

At **Rule** go for **SSH**. And at **CIDR** go for `0.0.0.0/0`
![create-rule](images/2018-06-29-113753_731x585_scrot.png)

### CIDR
You can also write a custom rule. You can then define a port (or port-range) which is allowed and also specifiy the CIDR. 
CDIR defines a set of ip addresses which the rule applies to. Above we used `0.0.0.0/0` which means *the whole world*. If you
only want to allow 1 ipaddress (for example, your home computer) you can set `83.37.128.94/32` where `/32` relates to 1 ip address.
If you want to give access to the whole silvius building, you can set a bigger range: `192.168.201.0/24`.
More info can be found here: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing

## Create Instance
Now we have all requirements set to create a instance.

Go to **Instances** and click (right side) on **Launch Instance**
![create-instance-1](images/2018-06-29-112746_1915x252_scrot.png)
![create-instance-2](images/2018-06-29-112814_953x506_scrot.png)
At sources with find images of operating systems or snapshots of instances. For now
select `Ubuntu 16.04 LTS`
![create-instance-3](images/2018-06-29-112830_951x1274_scrot.png)
Flavor relates to the size of the instance. There are predefined sets of flavors. The flavor
names are in the following format: `hpc.<number of cpu>c.<gb of ram>c.<gb of localdisk>h`. For
this workshop only `hpc.2c.2r.20h` is used. 
![create-instance-4](images/2018-06-29-112840_949x564_scrot.png)
At networks select the network with name `network`
![create-instance-5](images/2018-06-29-112848_948x564_scrot.png)
At security groups, select the security group you created.
![create-instance-6](images/2018-06-29-112916_933x502_scrot.png)
At keypair select your keypair.
![create-instance-7](images/2018-06-29-112927_947x665_scrot.png)
At configuration you can add a script which will launch first time the server boots. For now
leave empty
![create-instance-8](images/2018-06-29-113639_948x540_scrot.png)
click on **Launch Instance**

## Add floating IP address

<img align="right" src="images/2018-07-02-111918_186x192_scrot.png" width="20%" height="20%"><img/>
By default the ip address of an instance is unreachable. It is only reachable from inside it's
own network. A floating IP address is an ip address which connects to the ip address of the instance 
making it reachable from the outside. Naturalis has a set of public ip addresses. Each project in openstack is allowed a set ip addresses.

Click on **Associate Floating IP**
<img align="left" src="images/2018-07-02-112521_715x329_scrot.png" width="40%" height="40%"><img/>
Now it shows *No floating ip addresses allocated*. This means that there are no ip addresses reserved 
for your project. Click on the **+** sign to allocate a floating ip address to your project. Select 
at pool **external** and click on **Allocate IP**. Now you see an ip address which start 
with `145.136.24(0/1/2/3)`. Click on **Associate**. 

The result will be:
![new-instance](images/2018-07-02-113321_1351x120_scrot.png)

## Connecting via SSH to the instance

Start up putty and enter at `Host Name` ubuntu@your-floating-ip-address. Also give your `session` a name 
at `Saved Sessions`.

![putty-1](images/2018-07-02-115301_639x603_scrot.png)

Now go to `Connection->SSH->Auth` and at `Private key file for authentication` click on `browse` and select your **private key**

![putty-1](images/2018-07-02-115809_639x603_scrot.png)

Go back to `Session` and click on `Save` and then on `Open`. You are now connected to your instance.



**end of part 1**

