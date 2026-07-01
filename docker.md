# Docker

## Installation

Sur un serveur Ubuntu:

### Dépendances 
```
sudo apt update
sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg
```

### Clé GPG Docker
```
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

### Dépot Docker
```
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

### Docker

```
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
docker compose
```

### Droits
On ajoute l'utilisateur au groupe docker
```
sudo usermod -aG docker <mon_user>
groups  # Pour vérifier si le groupe docker existe
```
Il est nécessaire de quitter la session et de se reconnecter pour voir le groupe apparaitre.

### installation du projet
```
docker composer up -d
```


