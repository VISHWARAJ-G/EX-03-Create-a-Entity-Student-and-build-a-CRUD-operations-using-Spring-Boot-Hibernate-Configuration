# Exp 03 - Student Entity and CRUD Operations using Spring Boot Hibernate

# Name: Vishwaraj G
# Register Number: 212223220125

## AIM

To develop a Spring Boot application that performs **CRUD (Create, Read, Update, Delete)** operations on a `Student` entity using **Spring Data JPA (Hibernate)** with an **H2 in-memory database**.

---

## ALGORITHM

### Step 1: Create a Spring Boot Project

Create a new Spring Boot project using **Spring Initializr**.

Add the following dependencies:

* Spring Web
* Spring Data JPA
* H2 Database
* Spring Boot DevTools

### Step 2: Configure the Database

Configure the database connection in `application.properties`.

Define the H2 database URL, driver class, username, and password.

Enable Hibernate automatic Data Definition Language (DDL) using:

```properties
spring.jpa.hibernate.ddl-auto=update
```

Enable the H2 database console for viewing and managing the database.

### Step 3: Create the Student Entity Class

Create a `Student` entity class.

* Annotate the class with `@Entity`.
* Define the required fields.
* Annotate the primary key with `@Id`.
* Use `@GeneratedValue` for automatic ID generation.
* Create getters and setters for all fields.

### Step 4: Create StudentRepository

Create a repository interface that extends:

```java
JpaRepository<Student, Long>
```

`JpaRepository` provides built-in methods for performing CRUD operations.

### Step 5: Create StudentController

Create a REST Controller using the `@RestController` annotation.

Map the controller to:

```text
/students
```

Implement the following HTTP methods:

| HTTP Method | Endpoint         | Operation         |
| ----------- | ---------------- | ----------------- |
| `POST`      | `/students`      | Add a student     |
| `GET`       | `/students`      | Get all students  |
| `GET`       | `/students/{id}` | Get student by ID |
| `PUT`       | `/students/{id}` | Update student    |
| `DELETE`    | `/students/{id}` | Delete student    |

### Step 6: Run the Application

Run the Spring Boot application from the IDE or using Maven:

```bash
mvn spring-boot:run
```

### Step 7: Test the CRUD Operations

Use Postman or another REST API client to test the Create, Read, Update, and Delete operations.

---

# PROGRAM CODE

## Project Structure

```text
spring-boot-student-crud/
│
├── src/
│   └── main/
│       ├── java/
│       │   └── com/
│       │       └── example/
│       │           └── demo/
│       │               ├── DemoApplication.java
│       │               ├── model/
│       │               │   └── Student.java
│       │               ├── repository/
│       │               │   └── StudentRepository.java
│       │               └── controller/
│       │                   └── StudentController.java
│       │
│       └── resources/
│           └── application.properties
│
└── pom.xml
```

---

## 1. pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                             https://maven.apache.org/xsd/maven-4.0.0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.2</version>
        <relativePath/>
    </parent>

    <groupId>com.example</groupId>
    <artifactId>student-crud</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <name>Student CRUD</name>
    <description>Spring Boot Student CRUD Application</description>

    <dependencies>

        <!-- Spring Boot Web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Boot JPA -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- H2 Database (In-memory) -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

    </dependencies>

    <build>
        <plugins>

            <!-- Spring Boot Maven Plugin -->
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>

        </plugins>
    </build>

</project>
```

---

## 2. application.properties

```properties
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=

spring.jpa.hibernate.ddl-auto=update

spring.h2.console.enabled=true
```

---

## 3. Student.java

**Entity Class**

```java
package com.example.demo.model;

import jakarta.persistence.*;

@Entity
public class Student {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    private String name;
    private String department;
    private int age;

    // Getters and Setters

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getDepartment() {
        return department;
    }

    public void setDepartment(String department) {
        this.department = department;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

---

## 4. StudentRepository.java

**Repository Interface**

```java
package com.example.demo.repository;

import com.example.demo.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {
}
```

---

## 5. StudentController.java

**REST Controller**

```java
package com.example.demo.controller;

import com.example.demo.model.Student;
import com.example.demo.repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/students")
public class StudentController {

    @Autowired
    private StudentRepository studentRepository;

    // Create
    @PostMapping
    public Student addStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    // Read All
    @GetMapping
    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }

    // Read One
    @GetMapping("/{id}")
    public Optional<Student> getStudent(@PathVariable Long id) {
        return studentRepository.findById(id);
    }

    // Update
    @PutMapping("/{id}")
    public Student updateStudent(
            @PathVariable Long id,
            @RequestBody Student studentDetails) {

        Student student = studentRepository.findById(id).orElseThrow();

        student.setName(studentDetails.getName());
        student.setAge(studentDetails.getAge());
        student.setDepartment(studentDetails.getDepartment());

        return studentRepository.save(student);
    }

    // Delete
    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Long id) {

        studentRepository.deleteById(id);

        return "Student with ID " + id + " deleted successfully!";
    }
}
```

---

## 6. DemoApplication.java

**Main Application Class**

```java
package com.example.demo;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {

    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

---

# CRUD OPERATIONS

## 1. Create Student

### HTTP Request

```http
POST /students
```

### Request Body

```json
{
    "name": "John",
    "department": "Information Technology",
    "age": 21
}
```

### Output

![Create Student](https://github.com/user-attachments/assets/a6fa7322-b95a-4d06-ba72-3259cb5881cb)

---

## 2. Read All Students

### HTTP Request

```http
GET /students
```

### Output

![Read All Students](https://github.com/user-attachments/assets/bf62d198-293a-47c2-a397-01b079abe76d)

---

## 3. Read Student by ID

### HTTP Request

```http
GET /students/1
```

### Output

![Read Student](https://github.com/user-attachments/assets/c9692de3-c9e8-4d7a-b754-d7c513b152a7)

---

## 4. Update Student

### HTTP Request

```http
PUT /students/1
```

### Request Body

```json
{
    "name": "John Updated",
    "department": "Computer Science",
    "age": 22
}
```

### Output

![Update Student](https://github.com/user-attachments/assets/5061c5bf-a245-46c7-b768-a491ddfeda6f)

---

## 5. Delete Student

### HTTP Request

```http
DELETE /students/1
```

### Response

```text
Student with ID 1 deleted successfully!
```

### Output

![Delete Student](https://github.com/user-attachments/assets/a13f9b22-e97c-4de0-94f8-4fd54dd02b73)

---

# CRUD FLOW

```text
                    Client / Postman
                           │
                           ▼
                  StudentController
                           │
            ┌──────────────┼──────────────┐
            │              │              │
            ▼              ▼              ▼
         Create           Read        Update / Delete
            │              │              │
            └──────────────┼──────────────┘
                           ▼
                  StudentRepository
                           │
                           ▼
                  Spring Data JPA
                           │
                           ▼
                       Hibernate
                           │
                           ▼
                     H2 Database
```

---

# RESULT

Thus, the **Spring Boot application was successfully developed** to perform **CRUD (Create, Read, Update, and Delete)** operations on the `Student` entity using **Spring Data JPA (Hibernate)** with an **H2 in-memory database**.
