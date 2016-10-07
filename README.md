# Create self signed certs

mkdir ./certs
chcon -t svirt_sandbox_file_t ./certs
chgrp -R docker ./certs
docker run --rm -v $(pwd)/certs:/certs ehazlett/certm -d /certs bundle generate -o=local --host localhost --host 127.0.0.1 --host host.domain.tld
mv server-key.pem  host.domain.tld.key
mv server.pem host.domain.tld.crt

# Configure and edit environment

cp env.example .env
nano -w .env # or your favourite editor

# Create a network for nginx-proxy 

docker network create nginx-proxy

# Start services

cd /path/to/nginx-proxy
docker-compose up -d

# Stop services

cd /path/to/nginx-proxy
docker-compose down

# Clean up

docker-compose down -v
docker network rm nginx-proxy
