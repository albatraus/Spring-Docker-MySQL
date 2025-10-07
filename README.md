# 🐳 Spring Boot Docker Configuration Template

This repository contains Docker configuration files and Spring Boot application properties to help you set up a Spring application in a fully dockerized environment with MySQL database.

## 📋 What's Included

- **Dockerfile** - Multi-stage build configuration for Spring Boot applications
- **docker-compose.yml** - Complete orchestration for Spring app + MySQL database
- **application.properties** - Pre-configured database connection settings
- **schema.sql** - Database initialization script

## 🚀 Quick Start

### Prerequisites
- Docker Desktop installed and running
- Java 21 (for local development)
- Gradle or Maven

### Running the Application

1. Create your Spring project using Spring Initializer and insert the docker-compose and Dockerfile inside the Root folder of the project
2. Adjust the files depending on your needs (dbName user and passwords)
3. Add your ddls to the schema.sql
4. Navigate to project directory
5. Run the following command:

```bash
docker-compose up --build
```

This will:
- Start MySQL 8.0 database container
- Build and start your Spring Boot application container
- Create a network bridge between them
- Initialize the database schema

### Access Points

- **Spring Application**: http://localhost:8080
- **MySQL Database**: localhost:3307

## 🔧 Configuration Details

### Docker Compose Services

#### MySQL Container
- **Container Name**: `mysql-db`
- **Database**: `SOMEDB`
- **Username**: `springuser`
- **Password**: `springpass`
- **Port Mapping**: `3307:3306`
- **Volume**: Persistent storage with `mysql-data`

#### Spring Boot Container
- **Container Name**: `spring-app`
- **Port Mapping**: `8080:8080`
- **Depends On**: MySQL (with health check)

### Application Properties

The included `application.properties` is configured for:
- Docker networking with environment variable support
- Automatic schema initialization via `schema.sql`
- MySQL dialect and JPA settings
- Clean database on every restart

## 📝 Customization

### Change Database Name

**In `docker-compose.yml`:**
```yaml
environment:
  MYSQL_DATABASE: YOUR_DB_NAME
```

**In `application.properties`:**
```properties
spring.datasource.url=jdbc:mysql://${MYSQL_HOST:localhost}:${MYSQL_PORT:3306}/YOUR_DB_NAME
```

### Change Credentials

Update in both `docker-compose.yml` and `application.properties`:
```yaml
MYSQL_USER: your_username
MYSQL_PASSWORD: your_password
```

### Modify Schema

Edit `src/main/resources/schema.sql` with your table definitions.

## 🛠️ Useful Commands

```bash
# Start containers in detached mode
docker-compose up -d

# Stop containers
docker-compose down

# Stop and remove volumes (clean database)
docker-compose down -v

# View logs
docker-compose logs -f spring-app

# Rebuild after code changes
docker-compose up --build

# Access MySQL CLI
docker exec -it mysql-db mysql -u springuser -p
```

## 📂 Required File Structure

```
your-spring-project/
├── Dockerfile
├── docker-compose.yml
├── build.gradle (or pom.xml)
└── src/
    └── main/
        └── resources/
            ├── application.properties
            └── schema.sql
```

## 🔍 How It Works

1. **Docker Compose** starts MySQL container first
2. **Health Check** ensures MySQL is ready
3. **Spring Boot Container** builds from source using Gradle
4. **Spring Boot** waits for MySQL to be healthy before starting
5. **Schema.sql** executes automatically on startup
6. **Application** connects to MySQL via internal Docker network

## ⚙️ Environment Variables

The setup supports environment variable overrides:

| Variable | Default | Description |
|----------|---------|-------------|
| `MYSQL_HOST` | localhost | MySQL hostname |
| `MYSQL_PORT` | 3306 | MySQL port |
| `MYSQL_USER` | springuser | Database username |
| `MYSQL_PASSWORD` | springpass | Database password |
---

**Note**: This configuration is designed for development and learning purposes. Apply security best practices for production use.
