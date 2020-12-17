# Instalação do OpenSource OSM  :cloud:

### OSM Release: 8.0
[OSM](https://osm.etsi.org/docs/user-guide/01-quickstart.html#installing-osm)

## Requisitos mínimos de hardware
- 2 processadores
- 4Gb de RAM
- 20 Gb disco

## Sistema Operacional
- Ubuntu18.04 (64-bit variant required)


wget https://osm-download.etsi.org/ftp/osm-8.0-eight/install_osm.sh
chmod +x install_osm.sh
./install_osm.sh 2>&1 | tee osm_install_log.txt


docker stack ps osm |grep -i running
docker service ls

newgrp docker

Para reiniciar tudo
docker stack rm osm
docker stack deploy -c /etc/osm/docker/docker-compose.yaml osm

