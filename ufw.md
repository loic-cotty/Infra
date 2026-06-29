# Commande de base pour UFW

## Documentation

[ufw](https://doc.ubuntu-fr.org/ufw)


## Status
```
sudo ufw status [numbered]
```

## Ajouter une règle
```
sudo ufw allow ssh
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow from 192.168.1.0/24 # Autoriser un réseau entier
sudo ufw allow from 192.168.1.15 to any port 9100 proto tcp # Autoriser une IP spécifique, le port 9100 port par défaut de prometheus
```

## Supprimer une règle 
```
sudo ufw delete allow 9100/tcp
sudo ufw delete 4
```

## Blocage 
```
sudo ufw deny from 1.2.3.4 # Blocage IP
sudo ufw deny 9100/tcp # Blocage port
```


## Activer les logs
```
sudo ufw logging on
sudo ufw logging medium # Niveau détaillé
```

## Voir les logs 
```
sudo tail -f /var/log/ufw.log
sudo journalctl -f
```

## Activer/ Desactiver le pare feu
```
sudo ufw enable # Prudence est mère de sûreté, ne pas l'activer trop vite, risque de blocage réel 😉
sudo ufw disable
```

## Recharger ufw après modification
```
sudo ufw reload
```


## Limiter les tentatives ssh
```
sudo ufw limit ssh
```


## Réinit total
```
sudo ufw reset
```
