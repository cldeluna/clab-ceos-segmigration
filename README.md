# Containerlabs for Segmentation Migration Training

Tips:
Learn some basic linux!!
- ls
- cd
- mkdir
- pwd
- sudo
- curl
- ip add

Installing
Note: this is specific to the Linux distribution 
Ubuntu/Debian

```bash
sudo apt update && sudo apt upgrade -y
```

2. Install Required Packages

```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

```bash
sudo apt install -y iproute2 iputils-ping
```

Add Docker's Official GPG Key and Repository

Add Docker’s GPG key:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Uninstall all conflicting packages:

```bash
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
```



Add the Docker repository:

```console
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/debian/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```



Install Docker Packages

```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

```console
 sudo systemctl status docker
```

Some systems may have this behavior disabled and will require a manual start:

```console
$ sudo systemctl start docke
```

Verify that the installation is successful by running the `hello-world` image:

```console
$ sudo docker run hello-world
```



### [Manage Docker as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user)

https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user



### Command line Editor

- vi

Learn & Practice!
https://vimschool.netlify.app/introduction/vimtutor/

## Installing Containerlab

[Quick Setup](https://containerlab.dev/install)

The easiest way to get started with containerlab is to use the [quick setup script](https://github.com/srl-labs/containerlab/blob/main/utils/quick-setup.sh) that installs all of the following components in one go (or allows to install them separately):

- docker (docker-ce), docker compose
- Containerlab (using the package repository)
- [`gh`](https://cli.github.com/) CLI tool

The script has been tested on the following OSes:

- Ubuntu 20.04, 22.04, 23.10, 24.04

To install all components at once, run the following command on any of the supported OSes:

```console
curl -sL https://containerlab.dev/setup | sudo -E bash -s "all"
```

### Loading docker images

Download the latest or required cEOS image from arista.com (You will need a free Arista account using a buisness login email)

Get the image to your Ubuntu server.

```bash
# rename the tag as you like
docker import cEOS64-lab-4.28.xF.tar.xz ceos:4.28.xF
docker images | egrep "REPOSITORY|ceos"

```

I recommend always setting the "latest" tag.

```bash
docker import cEOS64-lab-4.28.xF.tar.xz ceos:latest
```



### Containerlab Topology for practicing Segmentation activities

cds-seg.clab.yml

This topology models a standard 3-tier core/distribution/access network and includes one "user device"
so we can understand the "user" end of things.

We will use this lab to 
1. review the work needed to provision a new subnet/vlan and 
2. to review the steps needed to move a user device to a different subnet/vlan.

### Getting the lab to your Virtual Machine

Using the git clone command:
`git clone https://github.com/cldeluna/clab-ceos-segmigration.git`

will create a new directory with the repository name and place the contents of the repository in that directory.


```bash
claudia@ubuntu:~/containerlabs$ git clone https://github.com/cldeluna/clab-ceos-segmigration.git
Cloning into 'clab-ceos-segmigration'...
remote: Enumerating objects: 17, done.
remote: Counting objects: 100% (17/17), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 17 (delta 6), reused 15 (delta 4), pack-reused 0
Receiving objects: 100% (17/17), 31.76 KiB | 813.00 KiB/s, done.
Resolving deltas: 100% (6/6), done.
claudia@ubuntu:~/containerlabs$ ls -al
total 44
drwxrwxr-x 11 claudia claudia 4096 Jul 28 17:09 .
drwxr-x---  8 claudia claudia 4096 Jul 28 15:17 ..
drwxrwxr-x  5 claudia claudia 4096 Jul 28 14:28 cds-dhcp
drwxrwxr-x  3 claudia claudia 4096 Jul 28 16:47 cds-seg
drwxrwxr-x  3 claudia claudia 4096 Jul 28 17:09 clab-ceos-segmigration  # <---New Directory
drwxrwxr-x  3 claudia claudia 4096 May 14 18:40 clab-quickstart
drwxrwxr-x  3 claudia claudia 4096 Jul 27 21:50 images
drwxrwxr-x  3 claudia claudia 4096 May 16 17:36 new_l2vlan_lab
drwxrwxr-x  4 claudia claudia 4096 Jul 25 12:45 seg-mig-nodummy
drwxrwxr-x  3 claudia claudia 4096 Jul 25 12:14 seg-migration
claudia@ubuntu:~/containerlabs$

```

Once you have cloned the lab repository, move into the new directory and deploy the lab.
Note that you will have to specify which lab you want because there are several lab YAML files 
in the repository


```bash
claudia@ubuntu:~/containerlabs$ pwd
/home/claudia/containerlabs
claudia@ubuntu:~/containerlabs$ ls -al
total 32
drwxrwxr-x 8 claudia claudia 4096 Jul 28 17:27 .
drwxr-x--- 8 claudia claudia 4096 Jul 28 15:17 ..
drwxrwxr-x 5 claudia claudia 4096 Jul 28 14:28 cds-dhcp
drwxrwxr-x 3 claudia claudia 4096 Jul 28 16:47 cds-seg
drwxrwxr-x 3 claudia claudia 4096 May 14 18:40 clab-quickstart
drwxrwxr-x 3 claudia claudia 4096 Jul 27 21:50 images
drwxrwxr-x 3 claudia claudia 4096 May 16 17:36 new_l2vlan_lab
drwxrwxr-x 3 claudia claudia 4096 Jul 25 12:14 seg-migration
claudia@ubuntu:~/containerlabs$ git clone https://github.com/cldeluna/clab-ceos-segmigration.git
Cloning into 'clab-ceos-segmigration'...
remote: Enumerating objects: 17, done.
remote: Counting objects: 100% (17/17), done.
remote: Compressing objects: 100% (11/11), done.
remote: Total 17 (delta 6), reused 15 (delta 4), pack-reused 0
Receiving objects: 100% (17/17), 31.76 KiB | 722.00 KiB/s, done.
Resolving deltas: 100% (6/6), done.
claudia@ubuntu:~/containerlabs$ ls -al
total 36
drwxrwxr-x 9 claudia claudia 4096 Jul 28 17:28 .
drwxr-x--- 8 claudia claudia 4096 Jul 28 15:17 ..
drwxrwxr-x 5 claudia claudia 4096 Jul 28 14:28 cds-dhcp
drwxrwxr-x 3 claudia claudia 4096 Jul 28 16:47 cds-seg
drwxrwxr-x 3 claudia claudia 4096 Jul 28 17:28 clab-ceos-segmigration
drwxrwxr-x 3 claudia claudia 4096 May 14 18:40 clab-quickstart
drwxrwxr-x 3 claudia claudia 4096 Jul 27 21:50 images
drwxrwxr-x 3 claudia claudia 4096 May 16 17:36 new_l2vlan_lab
drwxrwxr-x 3 claudia claudia 4096 Jul 25 12:14 seg-migration
claudia@ubuntu:~/containerlabs$ cd clab-ceos-segmigration/
claudia@ubuntu:~/containerlabs/clab-ceos-segmigration$ ls
README.md  cds-seg.clab.drawio.png  cds-seg.clab.yml  ceos-lab.clab.yml  na-us-0000-test-as01.cfg  na-us-0000-test-as02.cfg  na-us-0000-test-as03.cfg  na-us-0000-test-cs01.cfg  na-us-0000-test-ds01.cfg
claudia@ubuntu:~/containerlabs/clab-ceos-segmigration$ sudo clab deploy -t cds-seg.clab.yml
```


`sudo clab deploy -t cds-seg.clab.yml`

```bash
claudia@ubuntu:~/containerlabs/clab-ceos-segmigration$ sudo clab deploy -t cds-seg.clab.yml
INFO[0000] Containerlab v0.56.0 started
INFO[0000] Parsing & checking topology file: cds-seg.clab.yml
INFO[0000] Creating docker network: Name="clab", IPv4Subnet="172.20.20.0/24", IPv6Subnet="2001:172:20:20::/64", MTU=1500
INFO[0000] Creating lab directory: /home/claudia/containerlabs/clab-ceos-segmigration/clab-cds-seg
INFO[0000] Creating container: "na-us-0000-test-as02"
INFO[0000] Creating container: "na-us-0000-test-cs01"
INFO[0000] Creating container: "na-us-0000-test-ds01"
INFO[0000] Creating container: "na-us-0000-test-as01"
INFO[0000] Creating container: "na-us-0000-test-as03"
INFO[0002] Creating container: "desktop"
INFO[0004] Created link: na-us-0000-test-cs01:eth1 <--> na-us-0000-test-ds01:eth1
INFO[0004] Created link: na-us-0000-test-cs01:eth2 <--> na-us-0000-test-ds01:eth2
INFO[0004] Running postdeploy actions for Arista cEOS 'na-us-0000-test-as02' node
INFO[0005] Created link: na-us-0000-test-cs01:eth3 <--> na-us-0000-test-ds01:eth3
INFO[0005] Created link: na-us-0000-test-ds01:eth5 <--> na-us-0000-test-as01:eth1
INFO[0005] Created link: na-us-0000-test-cs01:eth4 <--> na-us-0000-test-ds01:eth4
INFO[0005] Running postdeploy actions for Arista cEOS 'na-us-0000-test-cs01' node
INFO[0005] Created link: desktop:eth1 <--> na-us-0000-test-as01:eth3
INFO[0005] Created link: na-us-0000-test-ds01:eth6 <--> na-us-0000-test-as01:eth2
INFO[0006] Created link: na-us-0000-test-ds01:eth7 <--> na-us-0000-test-as02:eth1
INFO[0006] Created link: na-us-0000-test-ds01:eth8 <--> na-us-0000-test-as02:eth2
INFO[0007] Created link: na-us-0000-test-ds01:eth9 <--> na-us-0000-test-as03:eth1
INFO[0007] Created link: na-us-0000-test-ds01:eth10 <--> na-us-0000-test-as03:eth2
INFO[0007] Running postdeploy actions for Arista cEOS 'na-us-0000-test-ds01' node
INFO[0007] Running postdeploy actions for Arista cEOS 'na-us-0000-test-as01' node
INFO[0007] Running postdeploy actions for Arista cEOS 'na-us-0000-test-as03' node
WARN[0075] Cannot parse export template /etc/containerlab/templates/export/auto.tmpl: template: auto.tmpl:49:18: executing "auto.tmpl" at <index $eps 1>: error calling index: reflect: slice index out of range
INFO[0075] Adding containerlab host entries to /etc/hosts file
INFO[0075] Adding ssh config for containerlab nodes
+---+-----------------------------------+--------------+---------------+-------+---------+-----------------+----------------------+
| # |               Name                | Container ID |     Image     | Kind  |  State  |  IPv4 Address   |     IPv6 Address     |
+---+-----------------------------------+--------------+---------------+-------+---------+-----------------+----------------------+
| 1 | clab-cds-seg-desktop              | c08bb91c8423 | ubuntu:latest | linux | running | 172.20.20.2/24  | 2001:172:20:20::7/64 |
| 2 | clab-cds-seg-na-us-0000-test-as01 | 92a4f5d3cdcf | ceos:4.32.1F  | ceos  | running | 172.20.20.31/24 | 2001:172:20:20::3/64 |
| 3 | clab-cds-seg-na-us-0000-test-as02 | 551c47850120 | ceos:4.32.1F  | ceos  | running | 172.20.20.32/24 | 2001:172:20:20::2/64 |
| 4 | clab-cds-seg-na-us-0000-test-as03 | 96c4473726fe | ceos:4.32.1F  | ceos  | running | 172.20.20.33/24 | 2001:172:20:20::5/64 |
| 5 | clab-cds-seg-na-us-0000-test-cs01 | 959123e6f3fa | ceos:4.32.1F  | ceos  | running | 172.20.20.10/24 | 2001:172:20:20::4/64 |
| 6 | clab-cds-seg-na-us-0000-test-ds01 | 1070af10995b | ceos:4.32.1F  | ceos  | running | 172.20.20.20/24 | 2001:172:20:20::6/64 |
+---+-----------------------------------+--------------+---------------+-------+---------+-----------------+----------------------+
claudia@ubuntu:~/containerlabs/clab-ceos-segmigration$
```

### Topology

![cds-seg.clab.drawio.png](cds-seg.clab.drawio.png)

How to read the topology file:

First, a default image of Arista ceos:4.32.1F is defined.  
If a node section does not specify an image then that Arista image will be used to spin up that device.

In addition to the 5 Arista EOS switches, the lab includes one "desktop" node which will use
the ubuntu:latest image ()

```yaml
name: cds-seg

topology:
  defaults:
    kind: ceos
    image: ceos:4.32.1F
  nodes:
    desktop:
      kind: linux
      image: ubuntu:latest
      cmd: sleep infinity
    na-us-0000-test-cs01:
      kind: ceos
      mgmt-ipv4: 172.20.20.10
      startup-config: ./na-us-0000-test-cs01.cfg
    na-us-0000-test-ds01:
      kind: ceos
      mgmt-ipv4: 172.20.20.20
      startup-config: ./na-us-0000-test-ds01.cfg
    na-us-0000-test-as01:
      kind: ceos
      mgmt-ipv4: 172.20.20.31
      startup-config: ./na-us-0000-test-as01.cfg
    na-us-0000-test-as02:
      kind: ceos
      mgmt-ipv4: 172.20.20.32
      startup-config: ./na-us-0000-test-as02.cfg
    na-us-0000-test-as03:
      kind: ceos
      mgmt-ipv4: 172.20.20.33
      startup-config: ./na-us-0000-test-as03.cfg

  links:
    - endpoints: ["na-us-0000-test-cs01:eth1", "na-us-0000-test-ds01:eth1"]
    - endpoints: ["na-us-0000-test-cs01:eth2", "na-us-0000-test-ds01:eth2"]
    - endpoints: ["na-us-0000-test-cs01:eth3", "na-us-0000-test-ds01:eth3"]
    - endpoints: ["na-us-0000-test-cs01:eth4", "na-us-0000-test-ds01:eth4"]
    - endpoints: ["na-us-0000-test-ds01:eth5", "na-us-0000-test-as01:eth1"]
    - endpoints: ["na-us-0000-test-ds01:eth6", "na-us-0000-test-as01:eth2"]
    - endpoints: ["na-us-0000-test-ds01:eth7", "na-us-0000-test-as02:eth1"]
    - endpoints: ["na-us-0000-test-ds01:eth8", "na-us-0000-test-as02:eth2"]
    - endpoints: ["na-us-0000-test-ds01:eth9", "na-us-0000-test-as03:eth1"]
    - endpoints: ["na-us-0000-test-ds01:eth10", "na-us-0000-test-as03:eth2"]
    - endpoints: ["desktop:eth1", "na-us-0000-test-as01:eth3"]


    - type: dummy
      endpoint:
        node: na-us-0000-test-as01
        interface: eth4
... <snip>
```



#### Credentials
ceos
admin/admin
ubuntu desktop

```bash
claudia@ubuntu:~/containerlabs/cds-seg$ docker exec -it clab-cds-seg-desktop bash
root@desktop:/# # You are now in the Ubuntu container
root@desktop:/# apt update

```

```bash
# You wil be logged in as root so you don't need sudo to elevate your privileges
# are root 
apt update
apt install -y iproute2 iputils-ping

ip addr add 192.168.100.33/24 dev eth1
ip link set eth1 up
```




On the switch side, you'll need to configure the corresponding interface. 
You can access the switch CLI and configure it as needed:

```bash
docker exec -it clab-<lab-name>-switch Cli
# or SSH
ssh admin@clab-<lab-name>-switch

```

```
root@desktop:/# history
    1  apt update
    2  apt install -y iproute2 iputils-ping
    3  ip addr add 192.168.100.33/24 dev eth1
    4  ip link set eth1 up
    5  ip add
    6  ping 192.168.100.5
    7  ping 192.168.100.1

```



``` bash
claudia@ubuntu:~/containerlabs/clab-ceos-segmigration$ sudo clab inspect
[sudo] password for claudia:
INFO[0000] Parsing & checking topology file: cds-seg.clab.yml
+---+-----------------------------------+--------------+---------------+-------+---------+-----------------+----------------------+
| # |               Name                | Container ID |     Image     | Kind  |  State  |  IPv4 Address   |     IPv6 Address     |
+---+-----------------------------------+--------------+---------------+-------+---------+-----------------+----------------------+
| 1 | clab-cds-seg-desktop              | 2120629b0541 | ubuntu:latest | linux | running | 172.20.20.2/24  | 2001:172:20:20::7/64 |
| 2 | clab-cds-seg-na-us-0000-test-as01 | f8cd269a6877 | ceos:4.32.1F  | ceos  | running | 172.20.20.31/24 | 2001:172:20:20::3/64 |
| 3 | clab-cds-seg-na-us-0000-test-as02 | d34416e9a14f | ceos:4.32.1F  | ceos  | running | 172.20.20.32/24 | 2001:172:20:20::2/64 |
| 4 | clab-cds-seg-na-us-0000-test-as03 | 77408daa8610 | ceos:4.32.1F  | ceos  | running | 172.20.20.33/24 | 2001:172:20:20::4/64 |
| 5 | clab-cds-seg-na-us-0000-test-cs01 | 1c8f30bee2f7 | ceos:4.32.1F  | ceos  | running | 172.20.20.10/24 | 2001:172:20:20::5/64 |
| 6 | clab-cds-seg-na-us-0000-test-ds01 | c460894d8a6d | ceos:4.32.1F  | ceos  | running | 172.20.20.20/24 | 2001:172:20:20::6/64 |
+---+-----------------------------------+--------------+---------------+-------+---------+-----------------+----------------------+
claudia@ubuntu:~/containerlabs/clab-ceos-segmigration$
```





# **Exercise 1 - Gateway and L2 down to Users Desktop**

Now that most everyone has their Containerlab topology up and running, lets work on an exercise.

Intranet Data Vlan

​	1.	Create a new Vlan 100 for subnet [192.168.100.0/24](https://192.168.100.0/24) with the gateway on the core

​	2.	Establish layer 2 down through the distribution switch down to as01 on a single link and keeping ds01 at layer 2.

​	3.	Configure as01 Eth3 as an access port on vlan 100.

​	4.	Configure IP [192.168.100.33/24](https://192.168.100.33/24) on the user desktop.

​	5.	Ping the gateway from the desktop and share a screen cap in this thread.

Tip:

The commands below will help you configure the IP address on the desktop.

```bash
# if you are logged in as root you do not need to start each command with sudo

ip addr add 192.168.100.33/24 dev eth1 #you should confirm the interface with `ip add` command

ip link set dev eth1 up   # make sure the interface is "up"

ip add # verify your IP address 
```

You may need to install some packages if the ip add command does not work (is not found)

```bash
# if you are logged in as root you do not need to start each command with sudo

sudo apt update

sudo apt install -y net-tools iproute2
```

Default Gateway
```bash
ip route change default via 192.168.100.1 dev eth1
```

```bash
claudia@ubuntu:~/containerlabs/test/clab-ceos-segmigration$ docker exec -it clab-cds-seg-fwl-desktop1 bash
root@desktop1:/# history
    1  apt update
    2  apt install -y iproute2 iputils-ping
    3  ip addr add 192.168.100.33/24 dev eth1
    4  ping 192.168.100.1
    5  ping 192.168.0.1
    6  exit
    7  history
    8  ping 192.168.100.1
    9  exit
   10  ip route change default via 192.168.100.1 dev eth1
   11  ping 192.168.0.1
   12  exit
   13  history

```


# **Exercise 2 - Link Aggregation**

**Goal**

Put all links into port channels first without LACP and then for the final configuration using LACP.

​	1.	Place the 4 links between cs an ds into a port channel numbered 10. Arista and other vendors call this LAG (Link Aggregation Groups) but its pretty common to just say port-channel.

​	2.	Place the two links between ds and as01 into a port channel numbered 100

​	3.	Place the two links between ds and as02 into a port channel numbered 200

​	4.	Place the two links between ds and as03 into a port channel numbered 300

https://arista.my.site.com/AristaCommunity/s/article/how-to-configure-link-aggregation-groups-in-eos

**Tip:**

when using LACP, the upstream switch should be the active member.

**Results**

provide the show port-channel output for each port channel configured from each device participating in the port channel. The first set without LACP and the second set with the port channels configured with LACP.

**Extra credit**

Pick a port channel

using LACP, configure both ends in passive mode - what happens?

using LACP, configure both ends in active mode - what happens?

**Tip:** 

what should you do with any existing configuration on an interface?? (that is what should you do with the switch interface configurations from Exercise 1?).

**Extra Extra credit.** 

Fix your port channel configurations so that you can still ping your desktop from the core and the gateway on the core from the desktop. (think port channel rather than interface)

# **Exercise 3 - Routing**



**Goal**
Configure dynamic routing using OSPF routing between core and distro, Move the 192.168.100.0/24 SVI from core to distro and make sure that the core has a layer 3 or routed path to the desktop.

1. Configure port channel 10 interface as a Layer 3 link (means that rather than having a switchport trunk interface with vlans you will have a routed point to point link on a specific subnet).  Use subnet 192.168.10.0/29.  Use the first available IP for the core and the last valid available IP for the ds.

From the core, ping the 102.168.10 ds Po10 interface IP (save screen shot)
From the distro, ping the 102.168.10 cs Po10 interface IP (save screen shot)

4. Configure loopback0 interfaces:
cs 192.168.0.1/32
ds 192.168.0.2/32

5. Configure OSPF area 10 and advertise the loopbacks.
https://arista.my.site.com/AristaCommunity/s/article/a-simple-ospf-configuration

**Extra credit:** Don't advertise everything.  Be deterministic about every route you are advertising.  

6. Move the vlan 100 SVI from the core to the distribution and route the subnet.  The cs should have a routing table which includes subnet 192.168.100.0/24 learned from the distro.

**Tip:**  Make sure routing is enabled on your core and distro

---

please share the following answers and results

**Questions:**

Why are loopbacks a best practice?

What special property do loopback interfaces have?

Why is it a best practice to be specific about which routes to advertise into your routing protocol?

**Results:**
Po10 interface ping screen shots form step 1
Share the routing table on both core and distro.
From the Core, try to ping the desktop 192.168.100.33. share screen shot
From the desktop, try to ping the vlan 100 gateway, the cs loopback and the ds loopback and share screen shots
From the core, trace to the desktop and share screen shot

Some of the above will fail.   Why might that be?
