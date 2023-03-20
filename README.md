# eShopOnSysbox
Complete .net 7 microservices architecture example using a sysbox master container acting as a VM including systemd functionality to run docker within docker securely. 

# pre-req
Setup sysbox-

install with yay on arch distros - refer to sysbox documentation for others (https://github.com/nestybox/sysbox)
yay -S sysbox-ce-bin

enable and start sysbox
-----------------------
sudo systemctl enable sysbox
sudo systemctl start sysbox

make sure this entry is in your docker deamon.json file (/etc/docker/daemon.json)
{
    "runtimes": {
        "sysbox-runc": {
            "path": "/usr/bin/sysbox-runc"
        }
    }
}

sudo systemctl daemon-reload 
sudo systemctl restart docker

check that docker can see the sysbox runtime
--------------------------------------------
docker info | grep -i runtime
Runtimes: sysbox-runc io.containerd.runc.v2 runc

# docker image
docker pull karlpothast/eshoponssybox:1.0.0

# docker run
docker run --runtime=sysbox-runc -itd -p 5100:5100 -p 5104:5104 -p 5107:5107 --name eshop2 eshop-on-sysbox:0.0.1

You should be able to see the catalog, SPA app and health checker with the following URLs :
http://localhost:5100
http://localhost:5104
http://localhost:5107

# references
.net7 eShopOnContainers microservices architecture
https://github.com/dotnet-architecture/eShopOnContainers

Sysbox
https://github.com/nestybox/sysbox

Ubuntu 22.04 with systemd :
[jrei/systemd-ubuntu](https://hub.docker.com/r/jrei/systemd-ubuntu)
*this worked easier than nestybox's ubuntu 22.04 iamge

# to do
add docker-compose functionality to always get the latest version
