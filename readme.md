# MySQL Backup & CI/CD Proof-of-Concept
This project is a proof-of-concept demonstrating how to automate MySQL database backups using a Bash script, test the backup with Bats, and integrate everything into a CI/CD pipeline with GitHub Actions. In this demo, the SQL database is created and seeded on-the-fly within the GitHub Actions workflow. Note: This setup is for testing and demonstration purposes only and is not intended for production use.

## Overview

This project provides a simple Bash script (`backup.sh`) to back up a MySQL database using `mysqldump`. It also includes:

- **Bats tests** (`test.bats`) to verify that the backup directory exists and a non-empty backup file is created.
- A **GitHub Actions workflow** that sets up a MySQL service, creates and seeds a test database, runs the backup script, executes the Bats tests, and finally builds and pushes a Docker image.
- **Secure handling of sensitive credentials** via GitHub Secrets.

**Important:** The MySQL database is spun up only during the GitHub Actions run (using a Dockerized MySQL service) to simulate the backup process. This means that the database you see in this project is **not a live production database**.

## Features

- **Automated Backup:** The `backup.sh` script creates a timestamped backup file using `mysqldump`.
- **Testing with Bats:** Automated tests ensure that the backup file is created and non-empty.
- **CI/CD Pipeline:** GitHub Actions workflow demonstrates how to:
    - Spin up a temporary MySQL instance,
    - Create and seed a test database,
    - Run backup tests,
    - Build a Docker image, and
    - Push the image to Docker Hub.
- **Proof-of-Concept:** This project is designed solely as a demo; the database is created dynamically within the CI workflow.
## Prerequisites

- **MySQL:** A MySQL database (or the MySQL Docker container used in the GitHub Actions workflow).
- **Bash:** The backup script is written in Bash.
- **Docker:** To build and run the Docker image.
- **GitHub Account:** To use GitHub Actions and manage GitHub Secrets.
- **Docker Hub Account:** To push the Docker image (if desired).

## CI/CD Pipeline

The GitHub Actions workflow (`.github/workflows/backup.yml`) demonstrates the following:

1. **MySQL Service Setup:**  
    A temporary MySQL instance is spun up for testing.
    
2. **Database Initialization:**  
    The workflow creates a test database and seeds it using `seed.sql`.
    
3. **Backup Execution:**  
    The `backup.sh` script is run to create a backup of the test database.
    
4. **Testing:**  
    Bats tests verify the backup process.
    
5. **Docker Build & Push:**  
    The pipeline builds a Docker image and pushes it to Docker Hub based on commit hash and semantic versioning.