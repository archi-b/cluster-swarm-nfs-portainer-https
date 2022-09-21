
# Start a Cluster Swarm
### $ docker swarm init

# Create a network "router-net"
### $ docker network create -d overlay --subnet 10.1.0.0/16 router-net

# Deploy Stack "Traefik"
### $ DNS=traefik.dns.com.br USER=admin HASHED_PASSWORD=$(openssl passwd -apr1 admin123) docker stack deploy -c docker-compose.yml traefik

# Deploy Stack "Portainer-ce"
### $ DNS=portainer.dns.com.br docker stack deploy -c docker-compose.yml portainer-ce

# Configure /etc/hosts in your host
### # echo "127.0.0.1       portainer.dns.com.br" >> /etc/hosts
### # echo "127.0.0.1       traefik.local.btfinanceira.com.br" >> /etc/hosts
