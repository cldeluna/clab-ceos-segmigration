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
- sudo apt update
- sudo apt install xxxxx
- sudo apt install -y iproute2 iputils-ping

Command line Editor
- vi

Learn & Practice!
https://vimschool.netlify.app/introduction/vimtutor/

### Containerlab Topology for practicing Segmentation activities
cds-seg.clab.yml

![cds-seg.clab.drawio.png](cds-seg.clab.drawio.png)

How to read the topology file:

First, a default image of Arista ceos:4.32.1F is defined.  If a node section does not specify an impage then that Arista image 

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

