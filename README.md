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
Serão citadas as instações da versão 8 com docker e versão 9 com kubernetes

## Versão 8 com docker

1) Sugiro criar na instalaçao do Ubuntu o usuário chamado mano.

2) Para instalar o OSM
```bash
wget https://osm-download.etsi.org/ftp/osm-8.0-eight/install_osm.sh
chmod +x install_osm.sh
./install_osm.sh 2>&1 | tee osm_install_log.txt
```
%comment na versao 9 ./install_osm.sh -c swarm%

3) Verificar se todos os containers subiram e estão em estado de running, e verificar as portas utilizadas
```bash
docker stack ps osm |grep -i running
docker service ls
```

4) Se os comandos já citados não funcionarem, será necessário efetuar rodar o comando abaixo para adicionar o usuário em questão no grupo docker
```bash
newgrp docker
```

5) A qualquer tempo, é possível fazer re deploy
```bash
docker stack rm osm && sleep 60
docker stack deploy -c /etc/osm/docker/docker-compose.yaml osm
```

6) Para verificar os logs
```bash
docker service logs osm_lcm     # exibe logs de todos os containers (inclusive os "dead") associados ao osm_lcm.
docker logs $(docker ps -aqf "name=osm_lcm" -n 1)  # exibe os logs do último container osm_lcm
```

7)
8)
9)
10)

## Versão 9 com kubernetes

1) Sugiro criar na instalaçao do Ubuntu o usuário chamado mano.

2) Desativar serviço
```bash
sudo systemctl stop ufw
```

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
kubectl get all -n monitoring


10) Para verificar as portas relacinadas a aplicacoes do OSM e aplicacoes correlatas
```bash
kubectl get pods -A -o wide
```


### Comum às versões 8 e 9

11) Digite usuário e senha admin opara acessar a interface web
http://endereco_ip

12) Para acessar o Grafana (dashboards) e Prometheus (coletor de dados do VIM)
- Grafana (http://endereco_ip:3000)
- Prometheus (http://endereco_ip:9091)

13) para adicionar um VIM OpenStack, neste caso, o Victoria
```bash
osm vim-create --name openstack1 --user admin --password keystoneadmin --auth_url http://endereco_ip:5000/v3 --tenant admin --account_type openstack --config='{security_groups: sg_admin, keypair: }'
```
Obs.: No exemplo acima foi criado um security group denominado sg_admin, para evitar utilizar o default. Neste grupo foi permitido acesso ssh e entrada e saída de pacotes icmp.


14) Ao acessar o grafana pela primeira vez, utilize a senha default:
usuario: admin
senha: admin
Obs.: Ao ser questionado para renomear a senha, clique em Skip.

15) Ao acessar a interface do prometheus, será possível visualizar as métricas que começam com o nome osm_.

16) Adiconando VNF e NS
O OSM cria as instancias e parametros de rede no VIM baseado em arquivos descriptors, que estão no formato YAM.


