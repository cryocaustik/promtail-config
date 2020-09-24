# configuring promtail as remote agent

Instructions on how to configure promtail remote agents (on remote servers)

- [configuring promtail as remote agent](#configuring-promtail-as-remote-agent)
  - [Setup](#setup)
    - [install unzip](#install-unzip)
    - [setup dir](#setup-dir)
    - [download & install promtail](#download--install-promtail)
    - [create config](#create-config)
    - [create promtail user & permissions](#create-promtail-user--permissions)
    - [create promtail service](#create-promtail-service)
    - [start promtail and verify it is stable](#start-promtail-and-verify-it-is-stable)
    - [enable auto start on reboot](#enable-auto-start-on-reboot)
  - [Links](#links)

## Setup

setup instructions

### install unzip

```bash
apt install -y unzip
```

### setup dir

make dir to house promtail and it's config

```bash
mkdir /usr/local/bin/promtail
```

### download & install promtail

find latest (stable) download link on the [releases page](https://github.com/grafana/loki/releases) and copy the url for the relevant version, then download and unzip it.

e.g. Ubuntu 18.04 we use `promtail-linux-amd64/zip`

```bash
cd /usr/local/bin/promtail
wget https://github.com/grafana/loki/releases/download/v1.6.1/promtail-linux-amd64.zip
unzip promtail-linux-amd64.zip
mv promtail-linux-amd64 promtail
rm promtail-linux-amd64.zip
chmod a+x promtail
```

### create config 

see reference [config-promtail.yml](./config-promtail.yml) and create similar config

```bash
vi config-promtail.yml
```

### create promtail user & permissions

create the promtail user and add it to the relevant groups (e.g. groups where the promtail agent will read from)

```bash
useradd --system promtail
usermod -aG systemd-journal,adm,jitsi promtail
```

### create promtail service

see reference [promtail.service](promtail.service) and create similar config

```bash
vi /etc/systemd/system/promtail.service
```

### start promtail and verify it is stable

```bash
service promtail restart
service promtail status
```

successful response:

```bash
:root: [20-09-24 7:34:32] ➜  ~ service promtail status
● promtail.service - Promtail service
   Loaded: loaded (/etc/systemd/system/promtail.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-09-24 07:20:19 UTC; 14min ago
 Main PID: 6333 (promtail)
    Tasks: 10 (limit: 4680)
   CGroup: /system.slice/promtail.service
           └─6333 /usr/local/bin/promtail/promtail -config.file /usr/local/bin/promtail/config-promtail.yml
```

### enable auto start on reboot

```bash
systemctl enable promtail.service
```

## Links

- [promtail setup](https://grafana.com/docs/loki/latest/clients/promtail/installation/)
- [promtail scrap configs](https://grafana.com/docs/loki/latest/clients/promtail/configuration/)
