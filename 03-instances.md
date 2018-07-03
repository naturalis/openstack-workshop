# Instances

## Some Linux basic

#### change dir
```
cd <directoryname>
```
examples
```
cd /data
cd /home/ubuntu
cd ~ (where does this go to?)
```

#### create directory
```
mkdir <directoryname>
```
examples
```
mkdir a-dir
mkdir /data/new-dir
mkdir -p /data/a/b/c (what's happening?)
```

#### edit a file
using *nano*
```
nano /path/to/file
```

#### installing/remove packages

first always update the online available package list
```
sudo apt update
```

install a package
```
sudo apt install htop
sudo apt install sysstat iftop
sudo apt install -y python3 ipython (what happened?)
```
to remove
```
sudo apt remove sysstat
```

update all packages
```
sudo apt upgrade
```
It **recommended** to update your full machine regularly. It is really usefull
to install the `unattended-upgrades` packages. This will install security updates
and notify you (if you login) if your system should be rebooted.

```
sudo apt install unattended-upgrades
```

#### moving / removing files

moving
```
mv old-name new-name
```
removeing
```
rm <file>
rm -r <directory>
```

#### Permission/Ownership of a file

Linux has three groups which can have permision. 
1. Owner
2. Group
3. Everybody

If you run
```
ubuntu@a-nice-instance:~$ ls -lh
total 27M
-rw-rw-r-- 1 ubuntu ubuntu 4.3K Jul  2 12:49 generate_cert.go
-rwxrwxr-x 1 ubuntu ubuntu  27M Jun 29 02:11 minio
```
In the first row you see the persissions. In the third and fourth row use see the 
owner en the groupowner.
The first character in the permissions notes if it is a directory or file (- = file / d = directory)
The next 3 are the permissions for the owner, then the next 3 are for the group owership and the last 3 are for everybody ownership.
* r -> read permissions
* w -> write permissions
* x -> execute permissions
* \- -> permission not granted
To change permission of a file for a user run
```
chmod u+x <filename> (this will add execute permissions)
chmod u-w <filename> (this will remove write permissions)
```
for group use `g` instead of `u` and for everybody use `o`

Add `-R` to the command to set the permissions of all files in a directory

You can also change the ownership
```
chown ubuntu:ubuntu <filename>
```
This will change the ownership to user `ubuntu` and group `ubuntu`

If you (as ubuntu user) don't own the file and want to change the permission/ownership use `sudo` in front.


## Snapshots

You can make a snapshot of a instance. This will save the current state of the instance and save it
in Ceph.

Go to the list of instances and hit **snapshot** and this will make a snapshot of your instance. 
You can start an instance of a snapshot.

> **Assignment**
>
> Create a snapshot
>
> Delete your instance
>
> Rebuild your instance with a snapshot

#### backup
Please not that snapshots are saved in our Ceph cluster. If this cluster somehow fails (which is unlikely) the data can be lost. So use drive or copy files to our fileservers to back your most important files.


## Screen

Screen is a program you can use to save your sessions in linux. It also can be used to have multiple sessions in 1 window. The default config is not very good, so install a better screen config by:
```
wget -O ~/.screenrc     http://git.grml.org/f/grml-etc-core/etc/grml/screenrc_generic
```
to start screen run
```
screen -DR <session-name>
```
now type
```
top
```
and type `ctrl + A` and then `d`. Then run `screen -ls` which shows the current running sessions.
To attach again run
```
screen -DR <session-name>
```
To create a new tab run `ctrl + A` and then `c`. To switch to this tab run `crtl +A` and then `n`.

> **Assignment**
>
> Close your terminal window with the X sign (Alt + F4)
>
> Reconnect to your instance
>
> attach to your screen session

To see the help of screen run `ctrl + A` and then `?`


## Scripts
For linux you can create simple scripts. Here is a simple script which installs some packages and 
creates some directories

```
#!/bin/bash

# This is a comment
# a bash script always starts with #!/bin/bash

# make directories in your home dir
mkdir -p /home/ubuntu/nice-dir
apt install git
```

to run it run `bash <scriptname>`

> **Assignment**
>
> Design a script which you can use to deploy an instance which
>
> installs htop, git and python-minimal
>
> Make a directory in your homefolder (note that the installer runs as root user, so change permissions)
>
> Delete your instance
>
> Create an instance and use the script


