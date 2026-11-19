# EXp_03_-Entity-Student-and-build-a-CRUD-operations-using-Spring-Boot-Hibernate-Configuration

## AIM:
To develop a Spring Boot application that performs CRUD (Create, Read, Update, Delete) operations on a Student entity using Spring Data JPA (Hibernate).

## ALGORITHM

### 1. Create Spring Boot Project

Add dependencies:

* Spring Web
* Spring Data JPA
* H2 Database or MySQL
* Spring Boot DevTools

### 2. Configure application.properties

* Define database connection
* Enable Hibernate auto DDL

### 3. Create Student Entity Class

* Annotate with `@Entity`
* Define fields with `@Id`, `@GeneratedValue`, etc.

### 4. Create StudentRepository

Extend `JpaRepository<Student, Long>` for CRUD methods.

### 5. Create StudentController

Handle HTTP methods:

| HTTP Method | Endpoint         | Operation         |
| ----------- | ---------------- | ----------------- |
| POST        | `/students/add`      | Add student       |
| GET         | `/students/get`      | Get all students  |
| GET         | `/students/{id}` | Get student by ID |
| PUT         | `/students/{id}` | Update student    |
| DELETE      | `/students/{id}` | Delete student    |


## PROGRAM CODE

### pom.xml
```
<dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webmvc</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-webmvc-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```
 ### application.properties
```
spring.application.name=Exp3
server.port=8082
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=jdbc:mysql://localhost:3306/students
spring.datasource.username=ro
spring.datasource.password= ****
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.jpa.show-sql=true
```
### Student.java
```
package com.sridhar.exp3.Entity;

import jakarta.persistence.*;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;

@Entity
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "sId")
    private Long id;
    private String name;
    private String department;
    private int age;

}

```
### StudentRepository.java
```
package com.sridhar.exp3.Repository;

import com.sridhar.exp3.Entity.Student;
import org.springframework.data.jpa.repository.JpaRepository;

public interface StudentRepository extends JpaRepository<Student,Integer> {

}

```
### StudentController.java
```
package com.sridhar.exp3.Controller;

import com.sridhar.exp3.Entity.Student;
import com.sridhar.exp3.Service.StudentService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;
import java.util.Optional;

@RestController
@RequestMapping("/students")
public class StudentController {
    @Autowired
    private StudentService service;
    @GetMapping("/get")
    public List<Student> getAllStudent(){
        return service.getAllStudents();
    }
    @PostMapping("/add")
    public String addStudent(@RequestBody Student student){
        service.addStudent(student);
        return "Student Added Successfully✅✅";
    }
    @GetMapping("/{id}")
    public Optional<Student> getStudent(@PathVariable int id){
        return service.getStudent(id);
    }
    @PutMapping("/{id}")
    public Student updateStudent(@PathVariable Integer id, @RequestBody Student studentDetails) {

        return service.updateStudent(id,studentDetails);
    }
    @DeleteMapping("/{id}")
    public String deleteStudent(@PathVariable Integer id) {
        service.deleteStudent(id);
        return "Student with ID " + id + " deleted successfully!";
    }


}

```
### StudentService.java
```
package com.sridhar.exp3.Service;

import com.sridhar.exp3.Entity.Student;
import com.sridhar.exp3.Repository.StudentRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import java.util.List;
import java.util.Optional;

@Service
public class StudentService {
    @Autowired
    private StudentRepository repo;
    public List<Student> getAllStudents(){
       return repo.findAll();

    }
    public void addStudent(Student student){
        repo.save(student);

    }

    public Optional<Student> getStudent(int id){
        return  repo.findById(id);
    }

    public Student updateStudent(Integer id, Student studentDetails) {
        Student student = repo.findById(id).orElseThrow();
        student.setName(studentDetails.getName());
        student.setAge(studentDetails.getAge());
        student.setDepartment(studentDetails.getDepartment());
        return repo.save(student);
    }

    public void deleteStudent(Integer id) {
            repo.deleteById(id);

    }
}

```
### Exp3.java
```
package com.sridhar.exp3;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Exp3Application {

    public static void main(String[] args) {
        SpringApplication.run(Exp3Application.class, args);
    }

}

```
## Output:
### POST /students/add
<img width="1917" height="1075" alt="image" src="https://github.com/user-attachments/assets/42762cc2-5f43-4551-a715-9c14e76958c3" />


### GET /students/get
<img width="1915" height="1075" alt="image" src="https://github.com/user-attachments/assets/0991a382-0c26-457d-92f1-fd0c9fe19884" />


### GET /students/:id
<img width="1916" height="1076" alt="image" src="https://github.com/user-attachments/assets/23595138-2675-49c7-82b2-b803e2c26496" />



### PUT /students/:id
<img width="1917" height="1076" alt="image" src="https://github.com/user-attachments/assets/7aec084e-829c-44c3-8ef8-c02ed3da952f" />


### DELETE /students/:id

<img width="1917" height="1078" alt="image" src="https://github.com/user-attachments/assets/3783b15a-1e4e-4c08-910e-7ea010ac9126" />


## Result:
Thus, the Spring Boot application was successfully developed to perform CRUD (Create, Read, Update, Delete) operations on the Student entity using Spring Data JPA (Hibernate).
