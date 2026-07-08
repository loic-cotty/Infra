# Apache


## Essentials Commands

### start/stop/restart
```
sudo systemctl start apache2
sudo systemctl stop apache2
sudo systemctl restart apache2
```

### reload
```
sudo systemctl reload apache2
sudo apachectl -k graceful
```

### status
```
sudo systemctl status apache2
```

### Check config (before restart, very usefull)
```
sudo apache2ctl configtest
```

### active / Deactive vhost
```
sudo a2ensite <vhost_conf_file>
sudo a2dissite <vhost_conf_file>
```

### Module List
```
apache2ctl -M
```

### active / deactive module
```
sudo a2enmod rewrite
sudo a2dismod rewrite
```

### Logs
```
/var/log/apache2/access.log
/var/log/apache2/error.log
```

### Vhost loaded
```
apache2ctl -S
```

### Check port listened by apache
```
sudo ss -lntp | grep apache
sudo netstat -tulpn | grep apache
```

### SSL let's encrypt location
```
/etc/letsencrypt/live/
```





