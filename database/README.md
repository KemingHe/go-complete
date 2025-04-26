# Setting Up Docker, PostgreSQL, and TablePlus

This guide will walk you through setting up a PostgreSQL database with Docker and connecting to it using TablePlus.

## Step 1: Install Docker Desktop

1. Visit the [Docker website](https://www.docker.com/products/docker-desktop) and download Docker Desktop for your operating system.
2. Install Docker Desktop by following the installation wizard.
3. After installation, launch Docker Desktop and wait for it to start (the icon will turn green when ready).

## Step 2: Pull PostgreSQL Image

1. Open a terminal and run this command to pull the PostgreSQL Alpine image:

    ```shell
    docker pull postgres:12-alpine
    ```

2. Verify the image was downloaded:

    ```shell
    docker images
    ```

## Step 3: Start PostgreSQL Container

1. Run the PostgreSQL container with the following command:

    ```shell
    docker run --name postgres12 -p 5432:5432 -e POSTGRES_PASSWORD=secret -e POSTGRES_USER=root -d postgres:12-alpine
    ```

    This command:
    - Names the container `postgres12`
    - Maps port 5432 on your machine to port 5432 in the container
    - Sets the superuser name to `root`
    - Sets the password to `secret`
    - Runs the container in detached mode

2. Verify the container is running:

    ```shell
    docker ps
    ```

## Step 4: Connect to PostgreSQL via Terminal (Optional)

1. Access the PostgreSQL console:

    ```shell
    docker exec -it postgres12 psql -U root
    ```

2. Run a test query:

    ```postgres
    SELECT NOW();
    ```

3. Exit the console:

    ```postgres
    \q
    ```

4. View container logs:

   ```shell
   docker logs postgres12
   ```

## Step 5: Install TablePlus

1. Visit the [TablePlus website](https://tableplus.com/) and download the application.
2. Install TablePlus by following the installation wizard.

## Step 6: Connect TablePlus to PostgreSQL

1. Open TablePlus and create a new connection.
2. Select PostgreSQL as the connection type.
3. Enter the following details:
   - Name: `postgres12`
   - Host: `localhost`
   - Port: `5432`
   - User: `root`
   - Password: `secret`
   - Database: `root`
4. Click "Test" to verify the connection.
5. Click "Connect" to connect to the database.

## Step 7: Create Database Schema Using SQL Files

1. In TablePlus, click on the SQL button to open a new query window.
2. Open the `simple_bank_postgres.sql` file from this directory.
3. Select all queries and run them by pressing Cmd+Enter (Mac) or Ctrl+Enter (Windows).
4. Refresh the connection (Cmd+R or Ctrl+R) to see the newly created tables:
   - `account`
   - `entries`
   - `transfers`
5. You can view each table's structure by clicking on the table name and selecting the "Structure" tab.

## Additional Commands

### Managing Docker Containers

- List running containers:

    ```shell
    docker ps
    ```

- Stop a container:

    ```shell
    docker stop postgres12
    ```

- Start a stopped container:

    ```shell
    docker start postgres12
    ```

- Remove a container:

    ```shell
    docker rm postgres12
    ```

### Database Management in TablePlus

- Delete tables: Select tables, right-click, and choose "Delete" (use cascade option to delete related data).
- Modify schemas: Edit the SQL files and re-run them after dropping the existing tables.
