# PostgreSQL

This folder contains instructions to set up a **PostgreSQL database environment**, including a custom configuration.

## Usage

### Starting the Container

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

```bash
docker compose up -d
  ```

**Explanation**: This command will download the PostgreSQL image, start the container, and configure the database environment.

---

### Accessing PostgreSQL

* **Host**: `localhost:5432`
* **Credentials**:

  * **User**: `postgres`
  * **Password**: `postgres`
  * **Database**: `postgres`

You can connect using any PostgreSQL client, such as **pgAdmin**, **DBeaver**, or the command line (`psql`).

For IDE integration:

* **IntelliJ**: use **Database Navigator** plugin
* **Visual Studio**: use **JDBC Client**

## Custom Docker Configuration

### Dockerfile Instructions

* `FROM postgres:latest` → Uses the latest official PostgreSQL image as the base.
* `COPY pg_hba.conf /custom-config/pg_hba.conf` → Copies your custom authentication configuration into the container.
* `COPY init.sh /docker-entrypoint-initdb.d/init.sh` → Adds an initialization script that runs when the container starts.
* `RUN chmod +x /docker-entrypoint-initdb.d/init.sh` → Makes the script executable.
* `ENV PGDATA=/var/lib/postgresql/data/pgdata` → Defines the directory where PostgreSQL stores its data.

### init.sh Script

This script executes during container startup:

```bash
#!/bin/bash
set -e

if [ -f "$PGDATA/pg_hba.conf" ]; then
    mv "$PGDATA/pg_hba.conf" "$PGDATA/pg_hba_old.conf"
    cp /custom-config/pg_hba.conf "$PGDATA/pg_hba.conf"
fi

exec "$@"
```

**Explanation**:

* Checks if a `pg_hba.conf` file exists in the data directory.
* If it exists, renames the old file and replaces it with the custom one.
* Executes the default PostgreSQL entrypoint.

---

### pg\_hba.conf Overview

`pg_hba.conf` controls **client authentication** in PostgreSQL. Key points:

* Defines **who can connect**, **how**, and **to which databases**.
* Connection types:

  * `local` → Unix domain socket
  * `host` → TCP/IP connection
  * `hostssl` → TCP/IP with SSL
  * `hostnossl` → TCP/IP without SSL
* Authentication methods: `trust`, `md5`, `password`, `scram-sha-256`, etc.
* Example configuration included in this project:

```text
# Local connections
local   all             all                                     trust

# IPv4 and IPv6 local connections
host    all             all             127.0.0.1/32            trust
host    all             all             ::1/128                 trust

# Replication connections from localhost
local   replication     all                                     trust
host    replication     all             127.0.0.1/32            trust
host    replication     all             ::1/128                 trust

# All remote connections (for learning purposes)
host all all all scram-sha-256
host all all 0.0.0.0/0 md5
```

**Caution**: For local learning environments, `trust` is fine.
For production, use stronger authentication methods and restrict remote access.
