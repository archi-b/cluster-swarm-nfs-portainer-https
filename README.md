# cluster-swarm-nfs-portainer-https
Cluster Swarm with Portainer, Traefik HTTP/HTTPS and a NFS Server

# First see how to prepare infrastruture
### See https://github.com/archi-b/cluster-swarm.git

## Start a Vagrant Nodes
`vagrant up`
### To view nodes
`vagrant global-status`

## Inside a VM node
### Choice a vm to be a NFS Server (Setting to NFS_SERVER variable to use in Portainer Stack)
`vagrant ssh node-04`
### Install NFS Server
`sudo yum -y install nfs-utils nfs-utils-lib`
`sudo chkconfig nfs on`
`sudo service rpcbind start`
`sudo service nfs start`
`sudo service nfslock start`
`sudo service rpcbind start`
### View NFS after install
`service nfs status`
### Create path to Portainer volume
`sudo mkdir nfs-data && cd nfs-data`
`sudo mkdir portainer && cd ..`
`sudo chmod -R 777 .`
### Create a map to volume /nfs-data in NFS
`echo "/nfs-data *(rw,sync,no_root_squash,no_subtree_check,insecure)" > /etc/exports`

## Cluster Swarm
### Choice a vm to beginning config
`vagrant ssh node-01`
`git clone https://github.com/archi-b/cluster-swarm-nfs-portainer-https.git`
`cd cluster-swarm-nfs-portainer-https/`

### Config a first Swarm node
`sudo docker swarm init --advertise-addr 192.168.56.10`
### Create a network "router-net"
`docker network create -d overlay --subnet 10.1.0.0/16 router-net`
### Deploy Stack "Traefik"
`DNS=traefik.vm.com.br USER=admin HASHED_PASSWORD=$(openssl passwd -apr1 admin123) docker stack deploy -c docker-compose-traefik.yml traefik`
### Deploy Stack "Portainer-ce"
`DNS=portainer.vm.com.br NFS_SERVER=192.168.56.13 docker stack deploy -c docker-compose-portainer.yml portainer-ce`

# Configure /etc/hosts in all nodes
`echo "127.0.0.1       portainer.vm.com.br" >> /etc/hosts`
`echo "127.0.0.1       traefik.vm.com.br" >> /etc/hosts`