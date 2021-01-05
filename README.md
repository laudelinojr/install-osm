# Instalação do OpenSource OSM  :cloud:

### OSM Release: 9.0
[OSM](https://osm.etsi.org/docs/user-guide/01-quickstart.html)

## Requisitos mínimos de hardware
- 2 processadores
- 8 Gb de RAM
- 60 Gb disco

## Sistema Operacional
- Ubuntu18.04 (64-bit variant required)

## Passo a Passo para instalação

1) Sugiro criar na instalaçao do Ubuntu o usuário chamado mano.

2) Desativar serviço
sudo systemctl stop ufw

3) Para instalar o OSM e monitoramento do kubernetes cluster
```bash
wget https://osm-download.etsi.org/ftp/osm-9.0-nine/install_osm.sh
chmod +x install_osm.sh
./install_osm.sh --k8s_monitor 2>&1 | tee osm_install_log.txt
```

4) Ao ser questionado sobre prosseguir com a instalação, digite "Y".

5) Será solicitado a senha do sudo para o usuário mano.

6) Depois de tudo instalado, digite no browser:
http://endereco_ip

7) Execute o comando abaixo e observe se o status é "RUNNING"
```bash
kubectl get all -n osm
```

8) Para verificar o log por container
```bash
kubectl logs -n osm deployments/lcm           # for LCM
kubectl logs -n osm deployments/light-ui      # for LW-UI
kubectl logs -n osm deployments/mon           # for MON
kubectl logs -n osm deployments/nbi           # for NBI
kubectl logs -n osm deployments/pol           # for POL
kubectl logs -n osm deployments/ro            # for RO
kubectl logs -n osm deployments/keystone      # for Keystone
kubectl logs -n osm statefulset/kafka         # for Kafka
kubectl logs -n osm statefulset/mongo         # for Mongo
kubectl logs -n osm statefulset/mysql         # for Mysql
kubectl logs -n osm statefulset/prometheus    # for Prometheus
kubectl logs -n osm statefulset/zookeeper     # for Zookeeper
```

9) Verifique o status 


10) Digite usuário e senha admin opara acessar a interface web

11)
- OSM (http://endereco_ip)
- Grafana (http://endereco_ip:3000)
- Prometheus (http://endereco_ip:9091)

12) para adicionar um VIM OpenStack, neste caso, o Victoria
```bash
osm vim-create --name openstack1 --user admin --password keystoneadmin --auth_url http://endereco_ip:5000/v3 --tenant admin --account_type openstack --config='{security_groups: sg_admin, keypair: }'
```
Obs.: No exemplo acima foi criado um security group denominado sg_admin, para evitar utilizar o default.


13) Ao acessar o grafana pela primeira vez, utilize a senha default:
usuario: admin
senha: admin
Obs.: Ao ser questionado para renomear a senha, clique em Skip.




Antigo...
docker stack ps osm |grep -i running
docker service ls

newgrp docker

Para reiniciar tudo
docker stack rm osm
docker stack deploy -c /etc/osm/docker/docker-compose.yaml osm

