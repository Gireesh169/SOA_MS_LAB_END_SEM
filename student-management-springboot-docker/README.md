# Student Management System — Spring Boot 3 + Java 21 + MySQL + Docker

A basic REST API with CRUD operations for students.

## Requirements
- Java 21
- Maven 3.6+ (or use your IDE's Maven support)
- Docker Desktop with Docker Compose

## Project structure
```text
student-management/
├── src/main/java/com/example/studentmanagement/
│   ├── StudentManagementApplication.java
│   ├── controller/StudentController.java
│   ├── entity/Student.java
│   ├── repository/StudentRepository.java
│   └── service/StudentService.java
├── src/main/resources/application.properties
├── Dockerfile
├── docker-compose.yaml
└── pom.xml
```

## Run with Docker Compose
Open a terminal in this project folder and run:

```bash
mvn clean package -DskipTests
docker compose up --build
```

The API runs at `http://localhost:8080`.
MySQL is exposed to your computer at `localhost:3307` (inside Docker, the app connects to `db:3306`).

Stop the containers:
```bash
docker compose down
```

Stop and also delete the database volume (this permanently deletes saved database data):
```bash
docker compose down -v
```

## Run without Docker
1. Start a local MySQL server.
2. Create a database named `studentdb`.
3. Check the username/password in `src/main/resources/application.properties`.
4. Run:
```bash
mvn spring-boot:run
```

## REST API endpoints

Base URL: `http://localhost:8080/api/students`

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/students` | Add a student |
| GET | `/api/students` | Get all students |
| GET | `/api/students/{id}` | Get one student |
| PUT | `/api/students/{id}` | Update a student |
| DELETE | `/api/students/{id}` | Delete a student |

### Sample POST / PUT JSON
```json
{
  "name": "Ravi Kumar",
  "email": "ravi@example.com",
  "course": "Computer Science",
  "age": 20
}
```

## Test using curl
Create:
```bash
curl -X POST http://localhost:8080/api/students \
  -H "Content-Type: application/json" \
  -d '{"name":"Ravi Kumar","email":"ravi@example.com","course":"Computer Science","age":20}'
```

Get all:
```bash
curl http://localhost:8080/api/students
```

Get by ID:
```bash
curl http://localhost:8080/api/students/1
```

Update:
```bash
curl -X PUT http://localhost:8080/api/students/1 \
  -H "Content-Type: application/json" \
  -d '{"name":"Ravi Kumar","email":"ravi@example.com","course":"CSE","age":21}'
```

Delete:
```bash
curl -X DELETE http://localhost:8080/api/students/1
```

## Notes
- The Compose service name `db` is the hostname used by the application container. Do not use `localhost` for the database URL inside Docker.
- `depends_on` with a health check waits for MySQL to become healthy before starting the app.
- `spring.jpa.hibernate.ddl-auto=update` creates/updates the table automatically for this learning project.
- The sample database credentials are for local practice only. Change them before using this project outside a local development environment.
