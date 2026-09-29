Create internal Parallax Network

`docker network create parallax-net`

Create .env file

Copy the existing .env.example and using openssl generate random passwords. The .env will be used by docker-compose to set the environment variables. Since we know we will be using Redis and Jupyter, we will also generate random passwords for those services.

```bash
cat > .env <<EOF
POSTGRES_USER=parallax_admin
POSTGRES_PASSWORD=$(openssl rand -hex 24)
POSTGRES_DB=parallax_admin
REDIS_PASSWORD=$(openssl rand -hex 24)
JUPYTER_TOKEN=$(openssl rand -hex 24)
EOF
```

You can copy the .env.example to provide custom values.

PG Quickstart

```bash
docker run -d --name parallax-pg \
  --network parallax-net \
  --env-file .env \
  -v parallax_pgdata:/var/lib/postgresql \
  -p 127.0.0.1:5432:5432 \
  --restart unless-stopped \
  postgres:18.6-trixie
```

Redis Quickstart

```bash
docker run -d --name parallax-redis \
  --network parallax-net \
  --env-file .env \
  -p 127.0.0.1:6379:6379 \
  --restart unless-stopped \
  redis:8 sh -c 'exec redis-server --requirepass "$REDIS_PASSWORD"'
```

Jupyter Quickstart

It's safer to bind to localhost, but I will be the only user.

```bash
docker run -d --name parallax-jupyter \
  --network parallax-net \
  --env-file .env \
  -v ~/notes:/home/jovyan/work \
  -p 8888:8888 \
  --restart unless-stopped \
  quay.io/jupyter/base-notebook
  ```