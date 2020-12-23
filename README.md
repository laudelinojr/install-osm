# Instalação do OpenSource OSM  :orchestrator:

### OSM Release: 9.0
[OSM](https://osm.etsi.org/docs/user-guide/01-quickstart.html)

## Requisitos mínimos de hardware
- 2 processadores
- 8 Gb de RAM
- 60 Gb disco

## Sistema Operacional
- Ubuntu18.04 (64-bit variant required)

## Passo a Passo para isntalação

1) Sugiro criar na instalaçao do Ubuntu o usuário chamado mano.

2)
Para instalar o OSM e monitoramento do kubernetes cluster
wget https://osm-download.etsi.org/ftp/osm-9.0-nine/install_osm.sh
chmod +x install_osm.sh
./install_osm.sh --k8s_monitor 2>&1 | tee osm_install_log.txt

3)
Ao ser questionado sobre prosseguir com a instalação, digite "Y".

4)
Será solicitado a senha do sudo para o usuário mano.

5)



docker stack ps osm |grep -i running
docker service ls

newgrp docker

Para reiniciar tudo
docker stack rm osm
docker stack deploy -c /etc/osm/docker/docker-compose.yaml osm

