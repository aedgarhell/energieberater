# Infrastructure Setup

This document provides instructions on how to set up and run the project's infrastructure using Docker Compose.

## 1. Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed and running.
- [Docker Compose](https://docs.docker.com/compose/install/) (usually included with Docker Desktop).
- A command-line tool capable of running `bash` scripts (e.g., Git Bash on Windows, or any standard terminal on macOS/Linux).

## 2. Environment Configuration

The entire stack is configured using environment variables.

### Step 2.1: Create the `.env` file

Copy the example file to a new `.env` file in the project's root directory (the one containing the `app` folder).

```bash
cp ../../.env.example ../../.env
```

### Step 2.2: Edit the `.env` file

Open the newly created `.env` file and customize the variables. At a minimum, you must change:
- `FRONTEND_HOST`: The public domain for your frontend (e.g., `funnel.my-company.com`).
- `BACKEND_HOST`: The public domain for your backend API (e.g., `api.my-company.com`).
- `N8N_HOST`: The public domain for your n8n instance (e.g., `flow.my-company.com`).
- `MINIO_HOST`: The domain for your MinIO S3 API (e.g., `files.my-company.com`).
- `ACME_EMAIL`: The email address for Let's Encrypt SSL certificate registration.
- All passwords and secrets (e.g., `POSTGRES_PASSWORD`, `JWT_SECRET`).

### Step 2.3: Generate Traefik Credentials

The Traefik dashboard is protected by basic authentication. You need to generate a user/password string and add it to your `.env` file.

1.  Run the following command, replacing `user` and `password` with your desired credentials:
    ```bash
    echo $(htpasswd -nb user password)
    ```
    *Note: If you don't have `htpasswd`, you can use an online generator or install `apache2-utils`.*

2.  The command will output a string like `user:$apr1$j52jA...`. Copy this entire string.

3.  Open your `.env` file and add the following variable, pasting the copied string as the value:
    ```
    TRAEFIK_USERS=user:$apr1$j52jA...
    ```
    *Important: If your password string contains `$` characters, you may need to escape them by replacing `$` with `$$`.*

## 3. Running the Stack

All commands should be run from within this `app/infra` directory.

- **To start all services in the background:**
  ```bash
  docker-compose up -d
  ```

- **To build or rebuild the `frontend` and `backend` images:**
  ```bash
  docker-compose build
  ```

- **To follow the logs of all services:**
  ```bash
  docker-compose logs -f
  ```

- **To check the status and health of running services:**
  ```bash
  docker-compose ps
  ```

- **To stop and remove all services, networks, and volumes:**
  ```bash
  docker-compose down -v
  ```

## 4. Accessing Services

Once the stack is running and DNS is configured to point your domains to the server's IP, you can access the services:

- **Frontend App:** `https://<your-FRONTEND_HOST>`
- **Backend API:** `https://<your-BACKEND_HOST>`
- **n8n UI:** `https://<your-N8N_HOST>`
- **MinIO S3 API:** `https://<your-MINIO_HOST>`
- **MinIO Console:** `https://console.<your-MINIO_HOST>`
- **Traefik Dashboard:** `https://traefik.<your-FRONTEND_HOST>` (uses the credentials you generated).

Healthchecks are in place to ensure services start in the correct order. It may take a few minutes for all services to become available, especially on the first run.
