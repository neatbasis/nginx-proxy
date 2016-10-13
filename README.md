an [nginx-proxy] environment

# Ensure a container can access docker's socket on a SELinux enabled system

See https://github.com/dpw/selinux-dockersock

# Create self signed certs

```bash
mkdir ./certs
chcon -t svirt_sandbox_file_t ./certs # if your system uses SELinux
chgrp -R docker ./certs
docker run --rm -v $(pwd)/certs:/certs ehazlett/certm -d /certs bundle generate -o=local --host localhost --host 127.0.0.1 --host host.domain.tld
mv server-key.pem  host.domain.tld.key
mv server.pem host.domain.tld.crt
```

# Configure and edit environment

```bash
cp env.example .env
nano -w .env # or your favourite editor
```

# Create a network for nginx-proxy 

```bash
docker network create nginx-proxy
```

# Start services

```bash
cd /path/to/nginx-proxy
docker-compose up -d
```

# Stop services

```bash
cd /path/to/nginx-proxy
docker-compose down
```

# Clean up

```bash
docker-compose down -v
docker network rm nginx-proxy
```

[nginx-proxy]: https://github.com/jwilder/nginx-proxy
