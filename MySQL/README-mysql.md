# MySQL

This folder contains instructions to set up a **MySQL database environment**.

## Usage

### Starting the Container

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

   ```bash
   docker compose up -d
   ```

**Explanation**: This command will download the MySQL image, start the container, and configure the database environment.

---

### Accessing MySQL

* **Host**: `localhost:3306`
* **Credentials**:

  * **Root user**: `root`
  * **Root password**: `rootpassword`
  * **Database**: `my_database`
  * **Limited user**: `user`
  * **Limited user password**: `userpassword`

You can connect using any MySQL client, such as **MySQL Workbench** or the command line.

For IDE integration:

* **IntelliJ**: use **Database Navigator** plugin
* **Visual Studio**: use **JDBC Client**

## Explanation

### docker-compose.yml

The `docker-compose.yml` file defines how the container runs and its configuration:

```yaml
services:
  mysql:
    image: mysql:latest
    container_name: mysql
    ports:
      - "3306:3306"
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword 
      MYSQL_DATABASE: my_database       
      MYSQL_USER: user                  
      MYSQL_PASSWORD: userpassword
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```

### Configuration Details

* **MySQL Service**:

  * `image: mysql:latest`: Uses the latest MySQL image.
  * `container_name: mysql`: Assigns a name to the container.
  * `ports`: Maps container port `3306` to the host machine, enabling local connections.
  * `environment`: Sets environment variables for database configuration:

    * `MYSQL_ROOT_PASSWORD`: Root user's password.
    * `MYSQL_DATABASE`: Name of the initial database.
    * `MYSQL_USER`: Name of an additional user.
    * `MYSQL_PASSWORD`: Password for the additional user.
  * `volumes`: Persists MySQL data using a volume to ensure data is not lost when the container stops or is removed.