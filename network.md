# Network


## Interroger un DNS

### Domain Information Groper
```
dig next3.app-moba.com [MX|NS|TXT|]
dig +short next3.app-moba.com // retourne uniquement l'adresse IP
```


## Info sur l'ip
```
ip addr
curl ifconfig.me
curl -4 ifconfig.me
```


## Ecouter le traffic entrant
```
sudo tcpdump -ni any port 3000
```
