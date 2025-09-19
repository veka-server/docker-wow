# docker-wow
server wow azerothcore docker

```bash

# Clone
git clone https://github.com/veka-server/docker-wow.git
cd docker-wow

# Creer le fichier .env
cp .env-dist .env

# Editer le fichier de config
nano .env

# corriger les droits sur les dossiers si neccesaire
# chmod -R 777 ...

# Démarrer les services
docker compose up -d

# attendre que le ac-worldserver soit pret

# Se connecter au worldserver en interactif pour créer l'admin
docker attach ac-worldserver
# (dans la console AC> taper :)
# account create user password
# account set gmlevel user 3 -1
# puis Ctrl+p puis ctrl+q pour revenir à l'hôte

# Mettre à jour realmlist en DB
docker compose exec ac-database mysql -uroot -pazerothroot -e "USE acore_auth; UPDATE realmlist SET address = '192.168.1.119', localAddress = '192.168.1.119' WHERE id = 1;"

# verifier que la requete SQL a bien fonctionné
docker compose exec ac-database mysql -uroot -pazerothroot -e "USE acore_auth; SELECT id, name, address, port, localAddress, localSubnetMask FROM realmlist;"

# Modifier realmlist du client
# in World of Warcraft/Data/frFR/realmlist.wtf:
# set realmlist 192.168.1.119

```
