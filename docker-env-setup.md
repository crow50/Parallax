Create .env file

Copy the existing .env.example and using openssl generate random passwords. The .env will be used by docker-compose to set the environment variables.

```bash
cat > .env <<EOF
POSTGRES_USER=parallax
POSTGRES_PASSWORD=$(openssl rand -hex 24)
POSTGRES_DB=parallax_db
EOF
```

You can copy the .env.example to provide custom values.

## Database Commands

**1. Open a `psql` session as the admin user:**
```bash
docker compose exec parallax-db psql -U parallax -d parallax_db
```

**2. Inside `psql`, create the role, password, and create the database:**
```sql
CREATE ROLE n8n WITH LOGIN;
\password n8n
CREATE DATABASE n8n OWNER n8n;
REVOKE CONNECT ON DATABASE n8n FROM PUBLIC;
```

**3. Verify:**
```sql
\l
\du
\q # quits
```

**4. Test the new login works.** 

Test using localhost wih password:
```bash
docker compose exec parallax-db psql -U n8n -d n8n -h 127.0.0.1 -c '\conninfo'
```
It should prompt for the password you just set and print the connection details.
