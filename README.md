# docker-home-server
Docker containers made for the Raspberry Pi.

Containers:
```
docker-home-server
├── dns
│   ├── pihole (dns sinkhole - adblocker)
│   └── unbound (recursive dns server)
└── monitoring
    ├── grafana (dashboards)
    ├── prometheus (database)
    ├── node_exporter (exporter - host data)
    ├── cadvisor (exporter - docker data)
    └── speedtest-exporter (exporter - network speed data)
```

## Install
Configure Raspberry Pi:
```bash
sudo apt update
sudo apt upgrade
sudo sed -r -i.orig 's/#?DNSStubListener=yes/DNSStubListener=no/g' /etc/systemd/resolved.conf
sudo rm /etc/resolv.conf && sudo ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf
sudo systemctl restart systemd-resolved
sudo nano /etc/netplan/static_ip.yaml
sudo netplan apply
sudo apt install openssh-server
sudo apt install git
sudo apt install docker-compose
<ME OWN TERMINAL CONFIGS>
reboot
```

static_ip.yaml for netplan:
```yaml
network:
    version: 2
    renderer: networkd
    ethernets:
        <NETWORK_ADAPTER>:
            addresses:
                - <IP>/<SUBNETMASK>
            routes:
                - to: default
                  via: <GATEWAY_IP>
            nameservers:
                addresses:
                    - 127.0.0.1
```


Clone this repository:
```bash
git clone https://github.com/fjfinch/docker-home-server.git
```

Configure dns stack:
```bash
sudo docker volume create volume_pihole
sudo nano .env
sudo docker-compose up -d
sudo docker exec -it pihole sudo pihole -a -p

(GUI) add block lists
    https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt                  - Advertising
    https://v.firebog.net/hosts/AdguardDNS.txt                                                  - Advertising
    https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt     - Tracking & Telemetry
    https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts             - Tracking & Telemetry
    https://v.firebog.net/hosts/Easyprivacy.txt                                                 - Tracking & Telemetry
(GUI) add whitelist domains
    edge.activity.windows.com
    s.shopify.com
    self.events.data.microsoft.com

sudo docker exec -it pihole pihole updateGravity
```

Configure monitoring stack:
```bash
sudo docker volume create volume_grafana
sudo docker volume create volume_prometheus
sudo docker-compose up -d
```

Configure portainer:
```bash
sudo docker volume create volume_portainer
sudo docker-compose up -d
```


## Update
```bash
sudo docker pull <IMAGE>
#sudo docker rm -f <CONTAINER>
sudo docker-compose up -d
sudo docker image prune -a
```
