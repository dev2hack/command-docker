Here's the updated version of your `README.md` to include the new `pgadmin` setup with the `docker run` command you provided:

---

# Docker Container Setup for PostgreSQL, Redis, RabbitMQ, and pgAdmin

Great! Now that we have the correct images and tags listed in your `docker images` output, let's make sure everything is aligned with your current configuration and the Docker container setup.
Here’s the updated `README.md` with the Docker network creation at the top and sample environment variables for each container.

---

# Docker Container Setup for PostgreSQL, Redis, RabbitMQ, and pgAdmin

This guide provides instructions for setting up PostgreSQL, Redis (with and without a password), RabbitMQ, and pgAdmin in Docker containers.

---

### 1. **Create the Docker Network**

If you haven't already created the custom network `atcode-network`, do so with:

```bash
docker network create atcode-network
```

---

### Environment Variables

Here are sample environment variables to use for each service:

```env
# PostgreSQL
DATABASE_URL="postgresql://postgresuser:postgrespassword@localhost:5432/tigerhead?schema=public"

# Redis (choose based on whether you’re using a password or not)
REDIS_URL="redis://:redispassword@localhost:6379"  # with password
# OR
REDIS_URL="redis://localhost:6379"                 # without password

# RabbitMQ
RABBITMQ_URL="amqp://rabbitmquser:rabbitmqpassword@localhost:5672"
```

---

### 2. **PostgreSQL Setup**

```bash
docker run -d --name postgres \
  --network atcode-network \
  -e POSTGRES_PASSWORD=postgrespassword \
  -e POSTGRES_USER=postgresuser \
  -p 5432:5432 \
  bitnami/postgresql:14.13.0-debian-12-r26
```

---

### 3. **Redis Setup**

You can choose to run Redis **with** or **without a password**:

#### a. Redis with Password

```bash
docker run -d --name redis \
  --network atcode-network \
  -e REDIS_PASSWORD=redispassword \
  -p 6379:6379 \
  bitnami/redis:latest
```

In your `.env` file, configure `REDIS_URL` with the password:

```env
REDIS_URL="redis://:redispassword@localhost:6379"
```

#### b. Redis without Password

```bash
docker run -d --name redis \
  --network atcode-network \
  -e ALLOW_EMPTY_PASSWORD=yes \
  -p 6379:6379 \
  bitnami/redis:latest
```

In your `.env` file, update the `REDIS_URL` to remove the password:

```env
REDIS_URL="redis://localhost:6379"
```

---

### 4. **RabbitMQ Setup**

```bash
docker run -d --name rabbitmq \
  --network atcode-network \
  -e RABBITMQ_DEFAULT_USER=rabbitmquser \
  -e RABBITMQ_DEFAULT_PASS=rabbitmqpassword \
  -p 5672:5672 \
  -p 15672:15672 \
  bitnami/rabbitmq:latest
```

In your `.env` file, configure `RABBITMQ_URL`:

```env
RABBITMQ_URL="amqp://rabbitmquser:rabbitmqpassword@localhost:5672"
```

---

### 5. **pgAdmin Setup**

```bash
docker run -d \
  --name pgadmin \
  --network atcode-network \
  -p 5050:80 \
  -e PGADMIN_DEFAULT_EMAIL=admin@atcode.com \
  -e PGADMIN_DEFAULT_PASSWORD=pgadminpassword \
  elestio/pgadmin:latest
```

Access pgAdmin in your browser at `http://localhost:5050`.

---

### **Access Services**:

- **PostgreSQL**: Connect using `psql`:

  ```bash
  psql -h localhost -U postgresuser -d postgres -p 5432
  ```

- **Redis**:
  - With password:

    ```bash
    redis-cli -h localhost -p 6379 -a redispassword
    ```
  - Without password:

    ```bash
    redis-cli -h localhost -p 6379
    ```

- **RabbitMQ Management**: Open in your browser:

  ```
  http://localhost:15672
  ```

- **pgAdmin**: Open in your browser:

  ```
  http://localhost:5050
  ```

---

### **Steps to Run the Containers:**

1. **Ensure the custom Docker network `atcode-network` is created** (see step 1).
2. **Run each container** with the commands provided above.
3. **Verify running containers**:

   ```bash
   docker ps
   ```

---

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