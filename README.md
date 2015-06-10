# Docker-in-Docker + OVS + ryu + pipework Superpack

This container helps you connect [Docker-in-Docker](https://github.com/jpetazzo/dind) to [Open vSwitch](http://openvswitch.org/) (-in-Docker, with [ryu](http://osrg.github.io/ryu/)) via [pipework](https://github.com/jpetazzo/pipework).

Available on Docker Hub: https://registry.hub.docker.com/u/akihirosuda/dind-ovs-superpack/

## Usage

    $ sudo modprobe openvswitch
    $ docker run --privileged -t -i -e LOG=file -h dind-superpack --rm akihirosuda/dind-ovs-superpack
     * /etc/openvswitch/conf.db does not exist
     * Creating empty database /etc/openvswitch/conf.db
     * Starting ovsdb-server
     * Configuring Open vSwitch system IDs
     * Starting ovs-vswitchd
     * Enabling remote OVSDB managers
    Assigned 192.168.42.254 to ovsbr0
    root@dind-superpack:/# docker run -itd -h dind1 --name dind1 busybox
	root@dind-superpack:/# pipework ovsbr0 dind1 192.168.42.101/24
	root@dind-superpack:/# docker run -itd -h dind2 --name dind2 busybox
	root@dind-superpack:/# pipework ovsbr0 dind2 192.168.42.102/24
	root@dind-superpack:/# docker exec -it dind1 sh
	/ # ping -c 3 192.168.42.102
	PING 192.168.42.102 (192.168.42.102): 56 data bytes
	64 bytes from 192.168.42.102: seq=0 ttl=64 time=0.201 ms
	64 bytes from 192.168.42.102: seq=1 ttl=64 time=0.056 ms
	64 bytes from 192.168.42.102: seq=2 ttl=64 time=0.094 ms
	
	--- 192.168.42.102 ping statistics ---
	3 packets transmitted, 3 packets received, 0% packet loss
	round-trip min/avg/max = 0.056/0.117/0.201 ms
	/ # 


## Variables
You can add a few optional variables to `docker` like `docker -e VAR_NAME=VAR_VALUE`.

 * `OVSBR0_IP`: IPv4 address assigned to `ovsbr0` bridge. default="192.168.42.254".
 * `OVSBR0_NETMASK`: netmask of `ovsbr0`. default="24".

