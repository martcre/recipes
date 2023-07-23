# How to attach container to the host external lan
In this sample, the server has a second NIC `eno2`, which will be used as the network bridge br0:
In this case, eno2 is a USB-to-Ethernet device, which requires a special mode to be set:
```
usb_modeswitch -v 0bda -p 8151 -V 0bda -P 8152 -M 555342430860d9a9c0000000800006e0000000000000000000000000000000
```

Create a Network Bridge:
```
ip link set eno2 up
ip link set eno2 master br0
ip link show master br0
```

Create a Docker Bridge Network using br0
```
docker network create --driver=bridge --ip-range=192.168.178.128/25 --subnet=192.168.178.128/25 -o "com.docker.network.bridge.name=br0" br0
```
Sample of a container attached through the bridge:
```
sudo docker run -dit --name alpine1 --network br0 alpine ash
```
