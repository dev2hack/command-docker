Here's the updated version of your `README.md` to include the new `pgadmin` setup with the `docker run` command you provided:

---

# Docker Container Setup for PostgreSQL, Redis, RabbitMQ, and pgAdmin

Great! Now that we have the correct images and tags listed in your `docker images` output, let's make sure everything is aligned with your current configuration and the Docker container setup.

### Docker Images Available:
- **PostgreSQL**: `bitnami/postgresql:14.13.0-debian-12-r26`
- **Redis**: `bitnami/redis:latest`
- **RabbitMQ**: `bitnami/rabbitmq:latest`
- **pgAdmin**: `elestio/pgadmin:latest`

### Let's proceed with verifying and updating your commands.

---

### 1. **PostgreSQL Setup**

Your image tag is `14.13.0-debian-12-r26`, which corresponds to the latest version of PostgreSQL in the Bitnami repository. You already have the correct `POSTGRES_USER` and `POSTGRES_PASSWORD` set, so let's include the correct port mapping.

The `postgres` container should be configured as follows:

```bash
docker run -d --name postgres \
  --network atcode-network \
  -e POSTGRES_PASSWORD=postgrespassword \
  -e POSTGRES_USER=postgresuser \
  -p 5432:5432 \
  bitnami/postgresql:14.13.0-debian-12-r26
```

---

### 2. **Redis Setup**

The `bitnami/redis` image is the latest available tag, so you're good to go there. The `REDIS_PASSWORD` environment variable is set properly, and port `6379` is exposed. Here's the updated command for Redis:

```bash
docker run -d --name redis \
  --network atcode-network \
  -e REDIS_PASSWORD=redispassword \
  -p 6379:6379 \
  bitnami/redis:latest
```

---

### 3. **RabbitMQ Setup**

The RabbitMQ container uses the latest tag (`bitnami/rabbitmq:latest`), and your environment variables for setting the default user (`rabbitmquser`) and password (`rabbitmqpassword`) are correct. Also, we'll expose the necessary ports: `5672` for AMQP and `15672` for the RabbitMQ management interface.

Here’s the command for RabbitMQ:

```bash
docker run -d --name rabbitmq \
  --network atcode-network \
  -e RABBITMQ_DEFAULT_USER=rabbitmquser \
  -e RABBITMQ_DEFAULT_PASS=rabbitmqpassword \
  -p 5672:5672 \
  -p 15672:15672 \
  bitnami/rabbitmq:latest
```

---

### 4. **pgAdmin Setup**

To run `pgAdmin` with your custom configuration (email, password, and port mapping), use the following command:

```bash
docker run -d \
  --name pgadmin \
  --network atcode-network \
  -p 5050:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@atcode.com \
  -e PGADMIN_DEFAULT_PASSWORD=pgadminpassword \
  elestio/pgadmin:latest
```

### Breakdown:
- **`-p 5050:80`**: Maps port `5050` on your host machine to port `80` inside the container.
- **`PGADMIN_DEFAULT_EMAIL`**: Sets the default email for logging into pgAdmin.
- **`PGADMIN_DEFAULT_PASSWORD`**: Sets the password for logging into pgAdmin.

You can access pgAdmin in your browser at `http://localhost:5050`.

---

### **Steps to Run the Containers:**
Now that everything is aligned, follow these steps to ensure everything is set up correctly:

1. **Create the custom Docker network:**

   If you haven't created the network `atcode-network`, do it with:

   ```bash
   docker network create atcode-network
   ```

2. **Run the containers with the above commands**. 

### 5. **Check running containers**

After running the commands, verify that the containers are up and running, with the ports exposed correctly:

```bash
docker ps
```

You should see output similar to this:

```
CONTAINER ID   IMAGE                            COMMAND                  CREATED        STATUS        PORTS                                      NAMES
f8c093dff0a0   bitnami/postgresql:14.13.0-debian-12-r26   "/opt/bitnami/script…"   2 minutes ago  Up 2 minutes  0.0.0.0:5432->5432/tcp                     postgres
d8c90a5c43f4   bitnami/redis:latest              "/opt/bitnami/script…"   2 minutes ago  Up 2 minutes  0.0.0.0:6379->6379/tcp                     redis
bc72c7e8b57d   bitnami/rabbitmq:latest           "/opt/bitnami/script…"   2 minutes ago  Up 2 minutes  0.0.0.0:5672->5672/tcp, 0.0.0.0:15672->15672/tcp  rabbitmq
9f8d51d76b2e   elestio/pgadmin:latest           "/docker-entrypoint.s…"   2 minutes ago  Up 2 minutes  0.0.0.0:5050->80/tcp                       pgadmin
```

---

### **Check Container Logs** (Optional)

If you want to check logs for any container to ensure it's running properly, use:

```bash
docker logs postgres
docker logs redis
docker logs rabbitmq
docker logs pgadmin
```

---

### **Access Services**:

- **PostgreSQL**: You can connect to PostgreSQL via `psql` using the following:

  ```bash
  psql -h localhost -U postgresuser -d postgres -p 5432
  ```

- **Redis**: Use `redis-cli` to connect:

  ```bash
  redis-cli -h localhost -p 6379 -a redispassword
  ```

- **RabbitMQ Management**: Open RabbitMQ's web UI in your browser:

  ```
  http://localhost:15672
  ```

  Log in using:
  - Username: `rabbitmquser`
  - Password: `rabbitmqpassword`

- **pgAdmin**: Open pgAdmin in your browser:

  ```
  http://localhost:5050
  ```

  Log in using:
  - Email: `admin@atcode.com`
  - Password: `pgadminpassword`

---

Now, everything should be set up and ready to go with the containers running in the custom network (`atcode-network`), with the ports exposed and mapped correctly. Let me know if you need further clarification or assistance!

---

This should ensure your `README.md` is up-to-date with the new `pgAdmin` configuration along with your existing services!