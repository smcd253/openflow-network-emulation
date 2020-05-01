# EE555 - Broadband Network Architecture - Final Project
**An implementation of Openflow using Mininet and Pox**

Authors: Guozhou Wu and Spencer McDonough

## Setup
Follow [these](https://github.com/mininet/openflow-tutorial/wiki/Installing-Required-Software) instructions to install the required software.

[Install VirtualBox](https://www.virtualbox.org/wiki/Downloads) or your favorite VM software
NOTE: Linux users can run `sudo apt-get install virtualbox` because Linux rules.

Follow [these](https://github.com/mininet/openflow-tutorial/wiki/Set-up-Virtual-Machine) instructions to configure VM manager such that you can communicate with your Mininet VM.
NOTE: Linux users will find they can run this application on their host machine. We don't recommend this, as isolating experimental environments from your own is always good practice. But if you insist (and know what you are doing) then you can find the instructions to do this in the tutorials above.

## Connecting to OpenFlow VM
**Step 1:** Start your Mininet Ubuntu VM and log in
*Login: mininet*
*Password: mininet*
    * Step 1a: Run `ifconfig` to get your URL for SSHing into the VM from your host machine.
    * Step 1b: Run `sudo dhclient` to enable the NAT forwarding port on your VM to connect to the internet.

**Step 2:** Establish 2 separate connections to the VM 
```bash
# Connection 1: Mininet Console
ssh -X mininet@<your VM url>
```
```bash
# Connection 2: Controller Console
ssh mininet@<your VM url>
sudo dhclient # enables DHCP through the NAT connection you have provided your VM
```

## Replace Mininet and Pox
From either terminal you have used to connect to the VM (or from the VM itself)
```bash
cd ~/mininet
git remote set-url origin https://github.com/smcd253/mininet.git
git checkout master
git pull
```
```bash
cd ~/pox
git remote set-url origin https://github.com/smcd253/pox.git
git checkout dev
git pull
```

## Scenario 1
### Compilation and Run Instructions
**Step 1 (Controller Console):** Run The controller script in the controller console
```bash
cd pox
./pox.py log.level --DEBUG misc.controller_1
```

**Step 2 (Mininet Console):** Run mininet in the mininet console
```bash
sudo mn --custom ~/mininet/custom/topology_1.py  --topo mytopo --mac --controller remote
```

**Step 3 (Mininet Console):** Evaluate Ping and Iperf
```bash
# ping other hosts
h1 ping h2
pingall
# test tcp throughput
iperf h1 h2
```


## Scenario 2
### Compilation and Run Instructions
**Step 1 (Controller Console):** Run The controller script in the controller console
```bash
cd pox
./pox.py log.level --DEBUG misc.controller_2
```

**Step 2 (Mininet Console):** Run mininet in the mininet console
```bash
sudo mn --custom ~/mininet/custom/topology_2.py --topo mytopo --mac --controller remote
```

**Step 3 (Mininet Console):** Evaluate Ping and Iperf
```bash
# ping other hosts
h1 ping h2
pingall
# ping interfaces
h1 ping 10.0.0.1
h1 ping 20.0.0.1
h1 ping 30.0.0.1
# ping invalid IPs (destination unreachable)
h1 ping 8.8.8.8
# test tcp throughput
iperf h1 h2
```

## Scenario 3
### Compilation and Run Instructions
* Step 1 (Controller Console): Run The controller script in the controller console
```bash
cd pox
./pox.py log.level --DEBUG misc.controller_3
```

* Step 2 (Mininet Console): Run mininet in the mininet console
```bash
sudo mn --custom ~/mininet/custom/topology_3.py --topo mytopo --mac --controller remote
```

* Step 3 (Mininet Console): Evaluate Ping and Iperf
```bash
# ping other hosts
h9 ping h10
h9 ping h12
h9 ping h15
h9 ping h18
h9 ping h21
pingall
# ping interfaces
h9 ping 172.17.16.1
h9 ping 10.0.0.1
h9 ping 10.0.0.129
h9 ping 20.0.0.1
h9 ping 20.0.0.129
# ping invalid IPs (destination unreachable)
h9 ping 8.8.8.8
h12 ping 8.8.8.8
h21 ping 8.8.8.8
# test tcp throughput
iperf h9 h12
iperf h9 h15
iperf h9 h18
iperf h9 h21
```

## Custom Framework
We created the following files for this framework:
[topology_1.py](mininet/custom/topology_1.py)
[topology_2.py](mininet/custom/topology_2.py)
[topology_3.py](mininet/custom/topology_3.py)
[controller_1.py](pox/pox/misc/controller_1.py)
[controller_2.py](pox/pox/misc/controller_2.py)
[controller_3.py](pox/pox/misc/controller_3.py)
[switch.py](pox/pox/misc/switch.py)
[router.py](pox/pox/misc/switch.py)