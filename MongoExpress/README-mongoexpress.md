# MongoDB with Mongo-Express

This folder contains instructions to set up a **MongoDB environment with a web interface** provided by Mongo-Express.

## Usage

### Starting the Containers

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

        docker compose up -d

**Explanation**: This command will download the MongoDB and Mongo-Express images, start the containers, and configure the environment.

### Accessing MongoDB

* **Host**: `mongodb://localhost:27017`
* **Credentials**:
  * **User**: `admin`
  * **Password**: `admin123`

You can connect using any MongoDB client, such as **MongoDB Compass** or the command line `mongosh`:

     mongosh mongodb://admin:admin123@localhost:27017

> [!TIP]
>For IDE integration:
>* **IntelliJ**: use **Database Navigator** plugin.
>* **Visual Studio Code**: install **MongoDB for VS Code**.

### Accessing Mongo-Express

* **URL**: `http://localhost:8081`
* **Credentials**:
  * **User**: `admin`
  * **Password**: `pass`

The web interface allows you to easily explore and manage your MongoDB databases.

## Explanation

### docker-compose.yml

```yaml
services:
  mongodb:
    image: mongo:latest
    container_name: mongodb_container
    ports:
      - "27017:27017"
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: admin123
    volumes:
      - mongodb_data:/data/db

  mongo-express:
    image: mongo-express:latest
    container_name: mongo_express
    depends_on:
      - mongodb
    ports:
      - "8081:8081"
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: admin123
      ME_CONFIG_MONGODB_SERVER: mongodb

volumes:
  mongodb_data:
```

---

### Configuration Details

1. **MongoDB**:

   * `image: mongo:latest`: Uses the latest official MongoDB image.
   * `container_name: mongodb_container`: Names the MongoDB container.
   * `ports`: Maps container port `27017` to the host machine.
   * `environment`: Sets the administrator credentials:

     * `MONGO_INITDB_ROOT_USERNAME`: Admin user
     * `MONGO_INITDB_ROOT_PASSWORD`: Admin password
   * `volumes`: Uses a persistent volume (`mongodb_data`) to store database data.

2. **Mongo-Express**:

   * `image: mongo-express:latest`: Uses the latest official Mongo-Express image.
   * `container_name: mongo_express`: Names the Mongo-Express container.
   * `depends_on`: Ensures MongoDB starts before Mongo-Express.
   * `ports`: Maps container port `8081` to host port `8081` for web access.
   * `environment`: Configures admin credentials and MongoDB server:

     * `ME_CONFIG_MONGODB_ADMINUSERNAME`: Admin user for Mongo-Express
     * `ME_CONFIG_MONGODB_ADMINPASSWORD`: Admin password for Mongo-Express
     * `ME_CONFIG_MONGODB_SERVER`: Service name of the MongoDB container

3. **Volumes**:

   * `mongodb_data`: Persistent volume for MongoDB database data.

---

> [!IMPORTANT]  
>* Make sure ports `27017` and `8081` are free on your machine before starting the containers.
>* Mongo-Express depends on MongoDB being up and running, so don’t stop MongoDB while using the web interface.

