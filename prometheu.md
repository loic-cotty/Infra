# Prometheus

Le principe rapide : 

1. Des exporters à installer sur chaque serveur à monitorer. 

    De multiple exporter existent: 
   - node_exporter, 
   - node_apache, 
   - node_nginx, 
   - node_docker, etc. 

    Chacun de ces "exporter" extrait les metrics du service visé.


2. Une application prometheus, qui va ensuite récupérer les metrics de tout les exporter en activité. 

## Exporter

### Installation générale

Télécharger l'archive de l'exporter souhaité, la décompresser et la copier dans /usr/local/bin/
```
wget https://github.com/prometheus/XXX_exporter/releases/download/v1.11.1/XXX_exporter-1.11.1.linux-amd64.tar.gz
tar xzf XXX_exporter-1.11.1.linux-amd64.tar.gz
sudo cp XXX_exporter-1.11.1.linux-amd64/XXX_exporter /usr/local/bin/XXX_exporter-1.11.1
sudo ln -s /usr/local/bin/XXX_exporter-1.11.1 /usr/local/bin/node_exporter
```

Créer un service par exporter et un utilisateur par service.
```
sudo useradd --system --no-create-home --shell /usr/sbin/nologin XXX_exporter
```

Créer un fichier systemd
```
sudo vi /etc/systemd/system/XXX_exporter.service
```

**Contenu général, à personnaliser pour chaque exporter**
```
[Unit]
Description=Prometheus XXX Exporter

[Service]
User=nXXX_exporter
Group=XXX_exporter
ExecStart=/usr/local/bin/XXX_exporter

[Install]
WantedBy=multi-user.target
```

**Reload deamon**
```
sudo systemctl daemon-reload
```

**Démarrer le service**
```
sudo systemctl start node_exporter
```

**Demarrer automatiquement au reboot du serveur**
```
sudo systemctl enable node_exporter
```

**Check service**
```
sudo systemctl status node_exporter
```

**Stop and restart**
```
sudo systemctl stop node_exporter
sudo systemctl restart node_exporter
```

**Log**
```
sudo journalctl -u node_exporter
```

**Test installation**
```
curl http://localhost:9100/metrics
```



### node_exporter

Archive : https://github.com/prometheus/node_exporter/releases/download/v1.11.1/node_exporter-1.11.1.linux-amd64.tar.gz

**Fichier systemd**
```
[Unit]
Description=Prometheus Node Exporter

[Service]
User=node_exporter
Group=node_exporter
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
```

### apache_exporter

Archive : https://github.com/Lusitaniae/apache_exporter/releases/download/v1.1.1/apache_exporter-1.1.1.linux-amd64.tar.gz

**Fichier systemd**
```
[Unit]
Description=Prometheus Apache Exporter
After=network.target

[Service]
User=apache_exporter
Group=apache_exporter
ExecStart=/usr/local/bin/apache_exporter --scrape_uri=http://127.0.0.1/server-status?auto

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

## Prometheus

### Installation

Pour simplifier les mises à jour et évolution, docker s'impose.

Un fichier docker-compose.yml, et au même niveau, un dossier prométheus, grafana, alertmanager

**docker-compose.yml** 
```
services:
  prometheus:
    image: prom/prometheus:v3.6.0
    container_name: prometheus
    restart: unless-stopped
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.enable-lifecycle'
    ports:
      - "9090:9090"
  grafana:
    image: grafana/grafana:12.0.0
    container_name: grafana
    restart: unless-stopped
    volumes:
      - grafana_data:/var/lib/grafana
    ports:
      - "3000:3000"
  alertmanager:
    image: prom/alertmanager:v0.28.1
    container_name: alertmanager
    restart: unless-stopped
    volumes:
      - ./alertmanager/alertmanager.yml:/etc/alertmanager/alertmanager.yml
    command:
      - '--config.file=/etc/alertmanager/alertmanager.yml'
    ports:
      - "9093:9093"

volumes:
  prometheus_data:
  grafana_data:

```

**prometheus.yml**
```
global:
  scrape_interval: 15s
  external_labels:
    monitor: app-moba
    environment: production
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - alertmanager:9093
rule_files:
  # - alerts.yml
scrape_configs:
  # Prometheus lui-même
  - job_name: prometheus
    static_configs:
      - targets:
          - prometheus:9090
  # node Exporter
  - job_name: node
    static_configs:
      - targets:
          - server1
        labels:
          server: server1
          environment: prod
```

### Commandes 

**Check config**
```
docker exec -it prometheus promtool check config /etc/prometheus/prometheus.yml
```

**Redémarrer le service** 
```
docker compose restart prometheus
curl -X POST http://localhost:9090/-/reload # La commande --web.enable-lifecycle doit etre activée dans le docker-compose
```

