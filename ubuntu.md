# Commandes de base Ubuntu

## Utilisateur

### Créer un utilisateur
```
sudo adduser <monuser>
```

### Ajouter un utilisateur dans un groupe
```
sudo usermod -aG sudo <monuser>
```

### Vérifier l'utilisateur courant
```
whoami
sudo whoami
```

## Se connecter avec le compte d’un autre user
```
sudo su other_user // saisir mdp de other_user
sudo su - other_user //(uniquement si on est sudo, saisir son propre mdp)
sudo -i -u other_user // idem que précédemment
```
## Désactiver un utilisateur
```
sudo usermod -L ubuntu   # bloque le mot de passe
sudo usermod -s /usr/sbin/nologin ubuntu  # empêche la connexion interactive
```


### Modifier son mot de passe
```
sudo passwd <monuser>
sudo passwd # to change root password
sudo passwd -l root # lock root acount
sudo passwd -S root # Visualize root account state : root L 2026-06-30 0 99999 7 -1 => L = locked, P = password set
```

### Vérifier un utilisateur
```
id <monuser>
```

### Lister les utilisateurs
```
cat /etc/passwd
cut -d: -f1 /etc/passwd
```

## Groupe

### Créer un groupe
```
sudo groupadd nom_du_groupe
```

### Modifier un groupe
```
sudo groupmod -n <nouveau_nom> <ancien_nom>
```

### Lister les groupes existants
```
cat /etc/group
```

### Vérifier les groups d’un utilisateur
```
groups <monuser>
```


## Gestion des paquets


## Liste des paquets
```
dpkg --get-selections | grep -w install
apt list --installed
```

## Installer un paquet
```
sudo apt install <nom-du-paquet> #sudo apt install apache2 -y
```

## Supprimer un paquet
```
sudo apt remove <nom-du-paquet>
```

## Nettoyer les paquets inutiles
```
sudo apt autoremove -y
sudo apt clean
```


## Mettre à jour la liste des paquets
```
sudo apt update
```

## Mettre à jour les paquets installés
```
sudo apt upgrade -y
```

## Chercher un paquet
```
dpkg -l | grep nginx
``

