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

