# Oracle Database

This folder contains instructions to set up an **Oracle Database environment**.

## Usage

### Starting the Container

1. **Navigate to this folder**  
   Open your terminal and go to the folder containing the `docker-compose.yml` file.

2. **Run the command**:

        docker compose up -d

**Explanation**: This command will download the Oracle Database image, start the container, and configure the database environment.

---

### Accessing Oracle Database

* **Host**: `localhost:1521`
* **Credentials**:

  * **SYS User**: `SYS`
  * **SYS Password**: `mysecurepassword`
  * **Database**: `mydatabase`
  * **Additional User**: `myuser`
  * **Additional User Password**: `userpassword`

You can connect using any Oracle client, such as **SQL Developer** or **DBeaver**.

For IDE integration:

* **IntelliJ**: use **Database Navigator** plugin
* **Visual Studio**: use **JDBC Client**