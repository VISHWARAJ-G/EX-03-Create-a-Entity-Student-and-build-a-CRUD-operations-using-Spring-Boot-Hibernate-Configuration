# EXp_03_-Entity-Student-and-build-a-CRUD-operations-using-Spring-Boot-Hibernate-Configuration

## AIM:
To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM:
Create Spring Boot Project

Add dependencies: Spring Web, Spring Data JPA, H2 Database or MySQL, Spring Boot DevTools

Configure application.properties

Define database connection

Enable Hibernate auto DDL

Create Student Entity Class

Annotate with @Entity

Define fields with @Id, @GeneratedValue, etc.

Create StudentRepository

Extend JpaRepository<Student, Long> for CRUD methods

Create StudentController

Handle HTTP methods:

POST /students → Add student

GET /students → Get all students

GET /students/{id} → Get student by ID

PUT /students/{id} → Update student

DELETE /students/{id} → Delete student

##PROGRAM CODE

### pom.xml
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
 ### application.properties

spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=
spring.jpa.hibernate.ddl-auto=update
spring.h2.console.enabled=true
### Student.java
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
    public Long getId() { return id; }

    public void setId(Long id) { this.id = id; }

    public String getName() { return name; }

    public void setName(String name) { this.name = name; }

    public String getDepartment() { return department; }

    public void setDepartment(String department) { this.department = department; }

    public int getAge() { return age; }

    public void setAge(int age) { this.age = age; }
}
### StudentRepository.java
package com.example.demo.repository;

import com.example.demo.model.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student, Long> {
}
### StudentController.java
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

    @PostMapping
    public Student addStudent(@RequestBody Student student) {
        return studentRepository.save(student);
    }

    @GetMapping
    public List<Student> getAllStudents() {
        return studentRepository.findAll();
    }

    @GetMapping("/{id}")
    public Optional<Student> getStudent(@PathVariable Long id) {
        return studentRepository.findById(id);
    }

    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Long id, @RequestBody Student studentDetails) {
        Student student = studentRepository.findById(id).orElseThrow();
        student.setName(studentDetails.getName());
        student.setAge(studentDetails.getAge());
        student.setDepartment(studentDetails.getDepartment());
        return studentRepository.save(student);
    }

    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Long id) {
        studentRepository.deleteById(id);
        return "Student with ID " + id + " deleted successfully!";
    }
}
### DemoApplication.java
```
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
### Output

#### Create
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/a6fa7322-b95a-4d06-ba72-3259cb5881cb" />

#### Read all
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/bf62d198-293a-47c2-a397-01b079abe76d" />

#### Read one
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/c9692de3-c9e8-4d7a-b754-d7c513b152a7" />

#### Update
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/5061c5bf-a245-46c7-b768-a491ddfeda6f" />

#### Delete
<img width="1366" height="768" alt="image" src="https://github.com/user-attachments/assets/a13f9b22-e97c-4de0-94f8-4fd54dd02b73" />

### Result

Thus, the Spring Boot application was successfully developed to perform CRUD (Create, Read, Update, and Delete) operations on the Student entity using Spring Data JPA (Hibernate) with MySQL database.


