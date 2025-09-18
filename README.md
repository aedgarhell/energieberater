# Lead-Funnel Strom & Gas

This repository contains the full application stack for the "Lead-Funnel Strom & Gas" project.

## Overview

The goal of this project is to create a fully automated sales funnel for an energy cooperative. It handles the entire process from a potential customer visiting a landing page to a legally binding contract being signed.

The architecture is based on a set of containerized microservices orchestrated by Docker Compose.

## Getting Started

All services are defined and configured in the `app/infra/` directory. For detailed instructions on how to set up the environment, build the services, and run the entire stack, please refer to the infrastructure documentation.

**→ [Infrastructure & Setup Instructions](./app/infra/README.md)**

## Repository Structure

- `/app/frontend`: Contains the Next.js/Astro landing page, funnel, and admin UI.
- `/app/backend`: Contains the NestJS/FastAPI backend, API, and business logic.
- `/app/workflows`: Contains n8n workflow exports and templates.
- `/app/infra`: Contains the `docker-compose.yml` file, Traefik configuration, and backup scripts.
- `/app/templates`: Contains email and PDF templates.
- `/app/docs`: Contains architecture diagrams, DSGVO documentation, and runbooks.
