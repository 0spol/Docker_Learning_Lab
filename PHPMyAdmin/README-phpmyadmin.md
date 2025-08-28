# phpMyAdmin with MySQL

This folder contains instructions to set up a **database environment** that includes **MySQL** and **phpMyAdmin**.

## Usage

### Starting the Containers

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

        docker compose up -d

**Explanation**: This command will download the necessary images, start the MySQL and phpMyAdmin containers, and configure the environment.

---

### Accessing phpMyAdmin

* **URL**: Open your browser and go to `http://localhost:8080`
* **Credentials**:

  * **User**: `root`
  * **Password**: `rootpassword`

## Explanation

### docker-compose.yml

```yaml
services:
  mysql:
    image: mysql:latest
    container_name: mysql
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword 
      MYSQL_DATABASE: my_database       
      MYSQL_USER: user                  
      MYSQL_PASSWORD: userpassword
    volumes:
      - mysql_data:/var/lib/mysql
    networks:
      - app_network

  phpmyadmin:
    image: phpmyadmin:latest
    container_name: phpmyadmin
    environment:
      PMA_HOST: mysql
      MYSQL_ROOT_PASSWORD: rootpassword
    ports:
      - "8080:80"  
    depends_on:
      - mysql
    networks:
      - app_network

volumes:
  mysql_data:

networks:
  app_network:
    driver: bridge
```

---

### Configuration Details

1. **MySQL**:

   * `image: mysql:latest`: Uses the latest MySQL image.
   * `container_name: mysql`: Names the MySQL container.
   * `environment`: Sets the environment variables for database configuration:

     * `MYSQL_ROOT_PASSWORD`: Root password
     * `MYSQL_DATABASE`: Initial database name
     * `MYSQL_USER`: Additional user
     * `MYSQL_PASSWORD`: Additional user password
   * `volumes`: Uses a persistent volume to store MySQL data.
   * `networks`: Connects the container to the application network.

2. **phpMyAdmin**:

   * `image: phpmyadmin:latest`: Uses the latest phpMyAdmin image.
   * `container_name: phpmyadmin`: Names the phpMyAdmin container.
   * `environment`: Sets variables to connect phpMyAdmin to MySQL:

     * `PMA_HOST`: Name of the MySQL container
     * `MYSQL_ROOT_PASSWORD`: Root password for authentication
   * `ports`: Maps container port 80 to host port 8080.
   * `depends_on`: Ensures MySQL container is ready before starting phpMyAdmin.
   * `networks`: Connects the container to the application network.

