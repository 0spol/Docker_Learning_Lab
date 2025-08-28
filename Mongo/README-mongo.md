# MongoDB

This folder contains instructions to set up a **MongoDB database environment**.

## Usage

### Starting the Container

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

```bash
docker compose up -d
  ```

**Explanation**: This command will download the latest MongoDB image, start the container, and configure the database environment.

---

### Accessing MongoDB

* **Host**: `localhost:27017`
* **Database**: `test` (default)
* **Authentication**:

  * If authentication is enabled, use the credentials defined in your `docker-compose.yml` file.
  * For learning purposes, you can start with no authentication or a simple username/password.

You can connect using any MongoDB client, such as **Mongo Shell**, **Mongo Compass**, or IDE plugins.

For IDE integration:

* **IntelliJ**: use **Database Navigator** plugin
* **Visual Studio**: use **MongoDB for Visual Studio** or **NoSQL Manager**

## Docker Configuration

### docker-compose.yml Overview

```yaml
services:
  mongo:
    image: mongo:latest
    container_name: mongo
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: root
      MONGO_INITDB_ROOT_PASSWORD: rootpassword
    volumes:
      - mongo_data:/data/db

volumes:
  mongo_data:
```

**Explanation**:

* `image: mongo:latest`: Uses the latest MongoDB image.
* `container_name: mongo`: Names the container.
* `ports`: Maps container port `27017` to the host, allowing local connections.
* `environment`: Sets environment variables for the root user.
* `volumes`: Persists database data using a Docker volume.

---

### Optional Initialization Scripts

You can add custom scripts to initialize your database:

* Place `.js` or `.sh` files inside `/docker-entrypoint-initdb.d/`.
* They will run automatically when the container is first started.
* Example: creating initial collections or inserting test data.
