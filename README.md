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
# account create <user> <password>
# account set gmlevel <user> 3 -1
# puis Ctrl+p puis ctrl+q pour revenir à l'hôte

# Mettre à jour realmlist en DB
docker compose exec ac-database mysql -uroot -pazerothroot -e "USE acore_auth; UPDATE realmlist SET address = '192.168.1.119', localAddress = '192.168.1.119' WHERE id = 1;"

# verifier que la requete SQL a bien fonctionné
docker compose exec ac-database mysql -uroot -pazerothroot -e "USE acore_auth; SELECT id, name, address, port, localAddress, localSubnetMask FROM realmlist;"

# Modifier realmlist du client
# in World of Warcraft/Data/frFR/realmlist.wtf:
# set realmlist 192.168.1.119

```

## Configuring AzerothCore in Containers

```yaml
# docker-compose.yml
services:
  ac-worldserver:
    environment:
      AC_ALLOW_TWO_SIDE_INTERACTION_CALENDAR: "1" # AllowTwoSide.Interaction.Calendar
```

Figuring out the environment variable name of a configuration parameter can be a bit difficult. The general rules for this:

- The entire parameter is prefixed with `AC_`
- Periods (`.`) become underscores (`_`)
- A sequence with a lowercase letter and then an uppercase letter inserts an underscore (`_`) between them
- The entire parameter is uppercased (so `foo` becomes `FOO`)

A few examples:

- `foo.bar_baz` => `AC_FOO_BAR_BAZ`
- `MaxPrimaryTradeSkill` => `AC_MAX_PRIMARY_TRADE_SKILL`
- `AllowTwoSide.Interaction.Calendar` => `AC_ALLOW_TWO_SIDE_INTERACTION_CALENDAR`


## Sources

Doc officiel https://github.com/azerothcore/wiki/blob/master/docs/install-with-docker.md

Liste des commandes GM : https://www.azerothcore.org/wiki/gm-commands
