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
Clone this repository:
```bash
git clone https://github.com/fjfinch/docker-home-server.git
```

Configure pihole:
```bash
sudo docker volume create volume_pihole
sudo docker exec -it pihole sudo pihole -a -p
(GUI) add block lists
        https://raw.githubusercontent.com/anudeepND/blacklist/master/adservers.txt                      Advertising
        https://v.firebog.net/hosts/AdguardDNS.txt                                                      Advertising
        https://raw.githubusercontent.com/crazy-max/WindowsSpyBlocker/master/data/hosts/spy.txt         Tracking & Telemetry
        https://raw.githubusercontent.com/FadeMind/hosts.extras/master/add.2o7Net/hosts                 Tracking & Telemetry
        https://v.firebog.net/hosts/Easyprivacy.txt                                                     Tracking & Telemetry
        https://raw.githubusercontent.com/StevenBlack/hosts/master/alternates/porn/hosts                Porn
(GUI) add whitelist domains
        edge.activity.windows.com
        s.shopify.com
        self.events.data.microsoft.com
sudo docker exec -it pihole pihole updateGravity
```

Configure grafana:
```bash
sudo docker volume create volume_grafana
```

Configure prometheus:
```bash
sudo docker volume create volume_prometheus
```

Configure portainer:
```bash
sudo docker volume create volume_portainer
```
