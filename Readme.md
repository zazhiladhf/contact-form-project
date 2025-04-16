# Contact Form Application

Author by **Zazhil Adhafi**

<a href="http://zazhil.my.id" target="_blank"><img src="" 
alt="" height="15" /> zazhil.my.id</a> | <a href="https://www.instagram.com/zazhiladhf/" target="_blank"><img src="https://ugc.production.linktr.ee/a8ba242a-a7f1-4d2e-93f0-15124dbbb705_IMG-4598.jpeg?io=true&size=thumbnail-stack-v1_0" 
alt="IMAGE ALT TEXT HERE" height="15" /> Instagram </a> | <a href="https://web.facebook.com/zazhil95" target="_blank"><img src="https://ugc.production.linktr.ee/2f39a16b-d486-42f7-90be-504dbfacbde9_IMG-1825.png?io=true&size=thumbnail-stack-v1_0" 
alt="IMAGE ALT TEXT HERE" height="15" /> Facebook </a>

## Table of Contents

- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Clone the Repository](#clone-the-repository)
  - [Project Structure](#project-structure)
  - [Set Up Environment Variables](#set-up-environment-variables)
  - [Build and Run the Application](#build-and-run-the-application)
- [Services Overview](#services-overview)
- [Accessing the Application](#accessing-the-application)
- [Stopping the Application](#stopping-the-application)
- [Volumes and Networks](#volumes-and-networks)
- [Troubleshooting](#troubleshooting)

## Getting Started

This project is a Dockerized contact form application that includes the following services:

1. **MariaDB**
   Database service for storing form submissions.
   <img src="readme-assets/mariadb.png" height="200"/>

2. **phpMyAdmin**
   Web interface for managing the MariaDB database.
   <img src="readme-assets/pypmyadmin.png" height="200"/>

3. **API Contact Form**
   Backend API for handling form submissions. Built with Golang.
   <img src="readme-assets/api.png" height="200"/>

4. **CMS Contact Form**
   <img src="readme-assets/cms.png" height="200"/>
   CMS for viewing and managing submitted forms. Built with Laravel.

5. **Client Contact Form**
   <img src="readme-assets/client.png" height="200"/>
   Embed Contact Form: Web interface for clients to embed into their applications. Built with React (Next.js).

### Prerequisites

- [Docker](https://www.docker.com/get-started) installed on your machine.
- [Docker Compose](https://docs.docker.com/compose/install/) installed.

### Clone the Repository

```bash
git clone https://github.com/zazhiladhf/contact-form-project.git
cd contact-form-project
```

### Project Structure

```
├── /app/
│    ├── api-contact-form
│    │   └── ...
│    ├── client-contact-form
│    │   └── ...
│    └── cms-contact-form
│        └── ...
├── /readme-assets/
├── .env
├── .gitignore
├── docker-compose.yaml
└── Readme.md
```

### Build and Run the Application

1. Build all services using Docker Compose:

```bash
docker compose build
```

2. start all services using Docker Compose:

```bash
docker compose up -d
```

This command will build the Docker images and start all containers as defined in the `docker-compose.yaml` file.

## Services Overview

### MariaDB

- **Image**: `mariadb:latest`
- **Ports**: Exposed on `3306`
- **Access Credentials**:
  - **Root User**: `user=root`, `password=rootpassword`
  - **User**: `user=user`, `password=password`
- **Environment Variables**: Uses variables from the root `.env` file.
- **Data Persistence**: Data is stored in the `mariadb-data` Docker volume.

### phpMyAdmin

- **Image**: `phpmyadmin/phpmyadmin:latest`
- **Ports**: Accessible via `http://localhost:8011`
- **Access Credentials**:
  - **Root User**: `user=root`, `password=rootpassword`
  - **User**: `user=user`, `password=password`
- **Dependencies**: Depends on the `mariadb` service.

### API Contact Form

- **Build Context**: `./app/api-contact-form`
- **Ports**: Accessible via `http://localhost:8080`
- **Environment Variables**: Uses variables from `app/api-contact-form/.env`
- **Dependencies**: Depends on the `mariadb` service.

### CMS Contact Form

- **Build Context**: `./app/cms-contact-form`
- **Ports**: Accessible via `http://localhost:8081`
- **Environment Variables**: Uses variables from `app/cms-contact-form/.env`
- **Dependencies**: Depends on the `api-contact-form` service.

### Client Contact Form

- **Build Context**: `./app/client-contact-form`
- **Ports**: Accessible via `http://localhost:8082`
- **Environment Variables**: Uses variables from `app/embed-contact-form/.env.local`
- **Dependencies**: Depends on the `api-contact-form` service.

## Accessing the Application

## 1. **API Contact Form**

```bash
Base URL: 'http://localhost:8080'
```

### Get All Contacts

Retrieve a list of all contacts.

- **HTTP Method**: `GET`
- **Endpoint**: `/contacts`

#### Request

```bash
curl --location 'http://localhost:8080/contacts'
```

#### Response

```json
{
  "code": "SUCCESS",
  "message": "Contacts retrieved successfully",
  "data": null
}
```

### Create New Contact

Create a new contact in the system.

- **HTTP Method**: `POST`
- **Endpoint**: `/contacts`
- **Headers**:
  - `Content-Type: application/json`

#### Request

```bash
curl --location 'http://localhost:8080/contacts' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "John Doe",
  "email": "john@example.com",
  "phone": "1234567890",
  "message": "Hello, World!"
}'
```

#### Response

```json
{
  "code": "CREATED",
  "message": "Contact created successfully",
  "data": {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com",
    "phone": "1234567890",
    "message": "Hello, World!",
    "created_at": "2024-10-17T15:18:21.119Z"
  }
}
```

### Update Contact

Update an existing contact's information.

- **HTTP Method**: `PUT`
- **Endpoint**: `/contacts/{id}`
  - Replace `{id}` with the contact's ID (e.g., `/contacts/1`)
- **Headers**:
  - `Content-Type: application/json`

#### Request

```bash
curl --location --request PUT 'http://localhost:8080/contacts/1' \
--header 'Content-Type: application/json' \
--data-raw '{
  "name": "Jane Doe",
  "email": "jane@example.com",
  "phone": "0987654321",
  "message": "Updated Message"
}'
```

#### Response

```json
{
  "code": "SUCCESS",
  "message": "Contact updated successfully",
  "data": {
    "id": 1,
    "name": "Jane Doe",
    "email": "jane@example.com",
    "phone": "0987654321",
    "message": "Updated Message",
    "created_at": "2024-10-17T15:18:21Z"
  }
}
```

### Delete Contact

Delete a contact from the system.

- **HTTP Method**: `DELETE`
- **Endpoint**: `/contacts/{id}`
  - Replace `{id}` with the contact's ID (e.g., `/contacts/1`)

#### Request

```bash
curl --location --request DELETE 'http://localhost:8080/contacts/1'
```

#### Response

```json
{
  "code": "SUCCESS",
  "message": "Contact deleted successfully",
  "data": null
}
```

## Notes

- Replace any placeholder values (like `{id}`) with actual data as needed.
- Ensure that the API server is running and the endpoint URLs are correct.
- If you encounter any errors, check the server logs for more details.

## 2. **CMS Contact Form**

Accessible at [http://localhost:8081](http://localhost:8081)

## 3. **Client Contact Form**

Accessible at [http://localhost:8082](http://localhost:8082)

## 4. **phpMyAdmin**

Accessible at [http://localhost:8011](http://localhost:8011)

## Stopping the Application

To stop containers without removing them:

```bash
docker-compose stop
```

To stop and remove all running containers and networks:

```bash
docker-compose down
```

## Volumes and Networks

- **Volumes**:
  - `contact-form-project_mariadb-contact-form-data`: Stores MariaDB data persistently.
- **Networks**:
  - `contact-form-network-database`: A bridge network for database communication.
  - `contact-form-network-api`: A bridge network for api communication.
  - `contact-form-network-cms`: A bridge network for cms communication.

## Troubleshooting

- **Port Conflicts**: Ensure that ports `3306`, `8011`, `8080`, `8081`, and `8082` are not being used by other applications.
- **Environment Variables**: Double-check all `.env` files for correct configurations.
- **Docker Resources**: Make sure Docker has enough resources allocated (CPU, memory).
