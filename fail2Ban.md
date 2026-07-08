# Fail2ban

### Logs
```
sudo tail -f /var/log/fail2ban.log
```

### Status
```
sudo fail2ban-client status
sudo fail2ban-client status sshd
```

### Unban
```
sudo fail2ban-client set sshd unbanip <ip>
```

### WHiteList an IP
La configuration de fail2ban se trouve dans 
```
sudo vi /etc/fail2ban/jail.conf
```

Si jail.local existe, l'éditer :
```
sudo vi /etc/fail2ban/jail.local
```
et modifier la section 
```
[DEFAULT]
ignoreip = 127.0.0.1/8 ::1 TON_IP_PUBLIQUE
```
Si jail.local n'existe pas, créer un fichier dans /etc/fail2ban/jail.d/ en ajoutant uniquement les surchages désirées
```
sudo vi /etc/fail2ban/jail.d/local.conf
```

restart service
```
sudo systemctl restart fail2ban
```


### Vérifier les ip ignorées
```
sudo fail2ban-client get sshd ignoreip
sudo fail2ban-client get apache-auth ignoreip
```