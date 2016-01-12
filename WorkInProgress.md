pom.xml
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.in28minutes</groupId>
	<artifactId>jpa-hibernate-step-by-step</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<properties>
		<spring-framework.version>4.2.3.RELEASE</spring-framework.version>
		<hibernate.version>5.0.6.Final</hibernate.version>
	</properties>

	<dependencies>
		<dependency>
			<groupId>org.hibernate</groupId>
			<artifactId>hibernate-entitymanager</artifactId>
			<version>${hibernate.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-context</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-tx</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-orm</artifactId>
			<version>${spring-framework.version}</version>
		</dependency>

		<dependency> <!-- Actually recommended to use JTA 1.1 but it needs different maven repository -->
			<groupId>org.jboss.spec.javax.transaction</groupId>
			<artifactId>jboss-transaction-api_1.2_spec</artifactId>
			<version>1.0.0.Final</version>
		</dependency>


		<dependency>
			<groupId>c3p0</groupId>
			<artifactId>c3p0</artifactId>
			<version>0.9.1.2</version>
		</dependency>

		<dependency>
			<groupId>org.hsqldb</groupId>
			<artifactId>hsqldb</artifactId>
			<version>2.3.2</version>
		</dependency>

		<dependency>
			<groupId>log4j</groupId>
			<artifactId>log4j</artifactId>
			<version>1.2.14</version>
			<scope>runtime</scope>
		</dependency>


		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>4.12</version>
			<scope>test</scope>
		</dependency>

		<dependency>
			<groupId>org.springframework</groupId>
			<artifactId>spring-test</artifactId>
			<version>${spring-framework.version}</version>
			<scope>test</scope>
		</dependency>

	</dependencies>
</project>
```
Readme.md
```
- Working with both Object-Oriented software and Relational Databases can be cumbersome and time consuming. Development costs are significantly higher due to a paradigm mismatch between how data is represented in objects versus relational databases.
- http://www.agiledata.org/essays/dataModeling101.html and http://en.wikipedia.org/wiki/Data_modeling are good starting points for understanding these data modeling principles.
- Hibernate takes care of the mapping from Java classes to database tables, and from Java data types to SQL data types.
- Also data query and retrieval facilities 
- Hibernate’s design goal is to relieve the developer from 95% of common data persistence-related programming tasks
- Hibernate is not a silver bullet. Not meant for application relying heavily on Stored Procedures.

http://docs.jboss.org/hibernate/orm/5.0/userGuide/en-US/html_single/images/overview.png

SessionFactory (org.hibernate.SessionFactory)
A thread-safe (and immutable) representation of the mapping of the application domain model to a database. Acts as a factory for org.hibernate.Session instances.

A SessionFactory is very expensive to create; there should be only one SessionFactory for an application for a given database. Maintains services that Hibernate uses across all Sessions such as second level caches, connection pools, transaction system integrations, etc.

Session (org.hibernate.Session)
A single-threaded, short-lived object conceptually modeling a "Unit of Work"[PoEAA].

Wraps a JDBC java.sql.Connection. Acts as a factory for org.hibernate.Transaction instances. Maintains a generally "repeatable read" persistence context (first level cache) of the application's domain model.

Transaction (org.hibernate.Transaction)
A single-threaded, short-lived object used by the application to demarcate individual physical transaction boundaries. It acts as an abstraction API to isolate the application from the underling transaction system in use (JDBC, JTA, CORBA, etc).

Domain Model
The POJO should have a no-argument constructor. Both Hibernate and JPA require this.


Hibernate Bean Validation

Bidirectional relationships must follow these rules.
- The inverse side of a bidirectional relationship must refer to its owning side by using the mappedBy element of the @OneToOne, @OneToMany, or @ManyToMany annotation. The mappedBy element designates the property or field in the entity that is the owner of the relationship.
- The many side of many-to-one bidirectional relationships must not define the mappedBy element. The many side is always the owning side of the relationship.
- For one-to-one bidirectional relationships, the owning side corresponds to the side that contains the corresponding foreign key.
- For many-to-many bidirectional relationships, either side may be the owning side.

Entities that use relationships often have dependencies on the existence of the other entity in the relationship. For example, a line item is part of an order; if the order is deleted, the line item also should be deleted. This is called a cascade delete relationship.
The javax.persistence.CascadeType enumerated type defines the cascade operations that are applied in the cascade element of the relationship annotations. Table 32-1 lists the cascade operations for entities.

Embeddable classes have the same rules as entity classes but are annotated with the javax.persistence.Embeddable annotation instead of @Entity.

The following embeddable class, ZipCode, has the fields zip and plusFour:

@Embeddable
public class ZipCode {
  String zip;
  String plusFour;
...
}
This embeddable class is used by the Address entity:

@Entity
public class Address {
  @Id
  protected long id
  String street1;
  String street2;
  String city;
  String province;
  @Embedded
  ZipCode zipCode;
  String country;
...
}


You can configure how the Java Persistence provider maps inherited entities to the underlying datastore by decorating the root class of the hierarchy with the annotation javax.persistence.Inheritance. The following mapping strategies are used to map the entity data to the underlying database:

A single table per class hierarchy

A table per concrete entity class

A “join” strategy, whereby fields or properties that are specific to a subclass are mapped to a different table than the fields or properties that are common to the parent class


The Java Persistence API provides the following methods for querying entities.

The Java Persistence query language (JPQL) is a simple, string-based language similar to SQL used to query entities and their relationships. See Chapter 34, The Java Persistence Query Language for more information.

The Criteria API is used to create typesafe queries using Java programming language APIs to query for entities and their relationships. See Chapter 35, Using the Criteria API to Create Queries for more information.

Both JPQL and the Criteria API have advantages and disadvantages.

Just a few lines long, JPQL queries are typically more concise and more readable than Criteria queries. Developers familiar with SQL will find it easy to learn the syntax of JPQL. JPQL named queries can be defined in the entity class using a Java programming language annotation or in the application’s deployment descriptor. JPQL queries are not typesafe, however, and require a cast when retrieving the query result from the entity manager. This means that type-casting errors may not be caught at compile time. JPQL queries don’t support open-ended parameters.

Criteria queries allow you to define the query in the business tier of the application. Although this is also possible using JPQL dynamic queries, Criteria queries provide better performance because JPQL dynamic queries must be parsed each time they are called. Criteria queries are typesafe and therefore don’t require casting, as JPQL queries do. The Criteria API is just another Java programming language API and doesn’t require developers to learn the syntax of another query language. Criteria queries are typically more verbose than JPQL queries and require the developer to create several objects and perform operations on those objects before submitting the query to the entity manager.
```
src\main\java\com\in28minutes\hibernate\model\Passport.java
```
package com.in28minutes.hibernate.model;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Passport {
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;

	private String number;

	@Column(name = "issued_country")
	private String issuedCountry;

	public Passport() {
		super();
	}

	public Passport(long id, String number, String issuedCountry) {
		super();
		this.id = id;
		this.number = number;
		this.issuedCountry = issuedCountry;
	}

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getNumber() {
		return number;
	}

	public void setNumber(String number) {
		this.number = number;
	}

	public String getIssuedCountry() {
		return issuedCountry;
	}

	public void setIssuedCountry(String issuedCountry) {
		this.issuedCountry = issuedCountry;
	}

	@Override
	public String toString() {
		return "Passport [id=" + id + ", number=" + number + ", issuedCountry=" + issuedCountry + "] ";
	}
}
```
src\main\java\com\in28minutes\hibernate\model\Project.java
```
package com.in28minutes.hibernate.model;

public class Project {
	private int id;
	private String name;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	@Override
	public String toString() {
		return "Project [id=" + id + ", name=" + name + "]";
	}
}
```
src\main\java\com\in28minutes\hibernate\model\Student.java
```
package com.in28minutes.hibernate.model;

import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.NamedQuery;
import javax.persistence.OneToOne;
import javax.persistence.Table;

@Entity
@Table(name = "Student")
@NamedQuery(query = "select s from Student s", name = "find all students")
public class Student {

	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	private long id;

	private String name;

	@OneToOne
	private Passport passport;

	private String email;

	public long getId() {
		return id;
	}

	public void setId(long id) {
		this.id = id;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public Passport getPassport() {
		return passport;
	}

	public void setPassportId(Passport passport) {
		this.passport = passport;
	}

	public String getEmail() {
		return email;
	}

	public void setEmail(String email) {
		this.email = email;
	}

	@Override
	public String toString() {
		return "Student [id=" + id + ", name=" + name + ", passport=" + passport + ", email=" + email + "]";
	}

}
```
src\main\java\com\in28minutes\hibernate\model\Task.java
```
package com.in28minutes.hibernate.model;

import java.util.Date;

public class Task {
	private int id;
	private int projectId;
	private Date startDate;
	private String desc;

	public int getId() {
		return id;
	}

	public void setId(int id) {
		this.id = id;
	}

	public int getProjectId() {
		return projectId;
	}

	public void setProjectId(int projectId) {
		this.projectId = projectId;
	}

	public Date getStartDate() {
		return startDate;
	}

	public void setStartDate(Date startDate) {
		this.startDate = startDate;
	}

	public String getDesc() {
		return desc;
	}

	public void setDesc(String desc) {
		this.desc = desc;
	}

	@Override
	public String toString() {
		return "Task [id=" + id + ", projectId=" + projectId + ", startDate=" + startDate + ", desc=" + desc + "]";
	}

}
```
src\main\java\com\in28minutes\hibernate\service\StudentRepository.java
```
package com.in28minutes.hibernate.service;

import java.util.List;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.persistence.Query;

import org.springframework.stereotype.Repository;

import com.in28minutes.hibernate.model.Student;

@Repository
public class StudentRepository {

	@PersistenceContext
	private EntityManager entityManager;

	public Student getStudent(final long id) {
		return entityManager.find(Student.class, id);
	}

	public Student insertStudent(Student student) {
		if (student.getPassport() != null)
			entityManager.merge(student.getPassport());
		return entityManager.merge(student);
	}

	public Student updateStudent(Student student) {
		entityManager.merge(student);
		return student;
	}

	public Student retrieveStudentsFrom(String string) {
		return null;
	}

	public List<Student> getAllStudents() {
		Query query = entityManager.createNamedQuery("find all students");
		return query.getResultList();
	}
}
```
src\main\java\com\in28minutes\hibernate\service\StudentService.java
```
package com.in28minutes.hibernate.service;

import java.util.List;

import javax.transaction.Transactional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.in28minutes.hibernate.model.Student;

@Service
public class StudentService {

	@Autowired
	StudentRepository service;

	@Transactional
	public Student insertStudent(Student student) {
		return service.insertStudent(student);
	}

	@Transactional
	public Student getStudent(final long id) {
		return service.getStudent(id);
	}

	@Transactional
	public Student updateStudent(Student student) {
		return service.updateStudent(student);
	}

	@Transactional
	public Student retrieveIndianStudents() {
		return service.retrieveStudentsFrom("India");
	}

	@Transactional
	public List<Student> getAllStudents() {
		return service.getAllStudents();
	}

}
```
src\main\resources\config\data.sql
```
INSERT INTO passport VALUES (201,'L1234567','India');
INSERT INTO passport VALUES (202,'L1234568','India');

INSERT INTO student VALUES (101,201,'Jane', 'jane@doe.com');
INSERT INTO student VALUES (102,202,'Doe', 'doe@doe.com');

INSERT into project VALUES (301, 'In28Minutes Project 1');

INSERT into task VALUES (401, 301, 'Create JPA Tutorial','2015-12-24');

INSERT into student_project VALUES (501, 101, 301, 401);
```
src\main\resources\config\database.properties
```
#HSQL in-memory db
db.driver=org.hsqldb.jdbcDriver
db.url=jdbc:hsqldb:mem:firstdb
db.username=sa
db.password=
```
src\main\resources\config\hibernate.properties
```
hibernate.dialect=org.hibernate.dialect.HSQLDialect
hibernate.show_sql=false
hibernate.format_sql=false
hibernate.use_sql_comments=true
```
src\main\resources\config\schema.sql
```
CREATE TABLE student (
  id int IDENTITY NOT NULL PRIMARY KEY,
  passport_id int NULL,
  name varchar(32) NOT NULL,
  email varchar(32) NOT NULL
);

CREATE TABLE passport (
  id int IDENTITY NOT NULL PRIMARY KEY,
  number varchar(32) NOT NULL,
  issued_country varchar(32) NOT NULL
);

alter table student add constraint student_passport_fk foreign key (passport_id) references passport(id);


CREATE TABLE project (
  id int IDENTITY NOT NULL PRIMARY KEY,
  name varchar(32) NOT NULL
);

CREATE TABLE task (
  id int IDENTITY NOT NULL PRIMARY KEY,
  project_id int NOT NULL,
  desc varchar(216) NOT NULL,
  start_date date DEFAULT NULL
);

alter table task add constraint task_project_fk foreign key (project_id) references project(id);

CREATE TABLE student_project (
  id int IDENTITY NOT NULL PRIMARY KEY,
  student_id int NOT NULL,
  project_id int NOT NULL,
  task_id int NOT NULL
);

alter table student_project add constraint student_project_student_fk foreign key (student_id) references student(id);
alter table student_project add constraint student_project_project_fk foreign key (project_id) references project(id);
alter table student_project add constraint student_project_task_fk foreign key (task_id) references task(id);
```
src\main\resources\log4j.properties
```
log4j.rootLogger=info, stdout

#log messages to stdout
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.out
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern= %d{dd/MM/yyyy HH:mm} [%-5p] %c:%L - %m%n

# Log everything. Good for troubleshooting  
log4j.logger.org.hibernate=debug
log4j.logger.org.springframework=debug
log4j.javax.servlet=info
   
# Log all JDBC parameters  
log4j.logger.org.hibernate.type=all
```
src\main\resources\META-INF\persistence.xml
```
<?xml version="1.0" encoding="UTF-8"?>

<persistence xmlns="http://java.sun.com/xml/ns/persistence"
	version="2.0">
	<persistence-unit name="hsql_pu" transaction-type="RESOURCE_LOCAL">
		<provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
		<properties>
			<property name="hibernate.dialect" value="org.hibernate.dialect.HSQLDialect" />
			<property name="hibernate.connection.url" value="jdbc:hsqldb:mem:firstdb" />
			<property name="hibernate.connection.driver_class" value="org.hsqldb.jdbcDriver" />
			<property name="hibernate.connection.username" value="sa" />
			<property name="hibernate.connection.password" value="" />
		</properties>
	</persistence-unit>
</persistence>
```
src\main\resources\META-INF\spring\spring-context.xml
```
<?xml version="1.0" encoding="UTF-8"?>

<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:jdbc="http://www.springframework.org/schema/jdbc"
	xmlns:context="http://www.springframework.org/schema/context" xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-3.2.xsd
       http://www.springframework.org/schema/jdbc
       http://www.springframework.org/schema/jdbc/spring-jdbc-3.2.xsd
       http://www.springframework.org/schema/context
       http://www.springframework.org/schema/context/spring-context-3.2.xsd
       http://www.springframework.org/schema/tx
       http://www.springframework.org/schema/tx/spring-tx-3.2.xsd">

	
	<context:component-scan base-package="com.in28minutes" />
	
	<context:property-placeholder location="classpath:config/*.properties" />

	<context:annotation-config />

	<!-- ======================================== -->
	<!-- embedded database configuration -->
	<!-- ======================================== -->

	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource"
		destroy-method="close">
		<property name="driverClass" value="${db.driver}" />
		<property name="jdbcUrl" value="${db.url}" />
		<property name="user" value="${db.username}" />
		<property name="password" value="${db.password}" />
	</bean>

	<jdbc:initialize-database data-source="dataSource">
		<jdbc:script location="classpath:config/schema.sql" />
		<jdbc:script location="classpath:config/data.sql" />
	</jdbc:initialize-database>

	<bean
		class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean"
		id="entityManagerFactory">
		<property name="persistenceUnitName" value="hsql_pu" />
		<property name="dataSource" ref="dataSource" />
	</bean>

	<bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
		<property name="entityManagerFactory" ref="entityManagerFactory" />
		<property name="dataSource" ref="dataSource" />
	</bean>
	
	<tx:annotation-driven transaction-manager="transactionManager"/>

    <bean class="org.springframework.orm.jpa.support.PersistenceAnnotationBeanPostProcessor" />

</beans>
```
src\test\java\com\in28minutes\hibernate\service\StudentServiceTest.java
```
package com.in28minutes.hibernate.service;

import static org.junit.Assert.assertEquals;
import static org.junit.Assert.assertNotNull;

import org.junit.After;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;
import org.springframework.transaction.annotation.Transactional;

import com.in28minutes.hibernate.model.Passport;
import com.in28minutes.hibernate.model.Student;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = { "classpath:META-INF/spring/spring-context.xml" })
@Service
public class StudentServiceTest {

	@Autowired
	private StudentService service;

	@Test
	@Transactional
	public void testGetStudent() {
		Student student = service.getStudent(101);
		System.out.println(student);
		assertNotNull(student);
		assertEquals(101, student.getId());
	}

	@Test
	@Transactional
	public void testGetStudent_GettingAPassport() {
		Student student = service.getStudent(101);
		assertNotNull(student.getPassport());
		assertEquals(201, student.getPassport().getId());
	}

	@Test
	@Transactional
	public void testUpdateStudent() {
		Student student = service.getStudent(101);
		student.setName("Doe v2");
		Student insertedStudent = service.updateStudent(student);
		Student retrievedStudent = service.getStudent(insertedStudent.getId());
		System.out.println(student);
		assertNotNull(retrievedStudent);
	}

	@Test
	public void testInsertStudent() {
		Passport passport = new Passport(202, "L12344432", "India");
		Student student = createStudent("dummy@dummy.com", "Doe", passport);
		Student insertedStudent = service.insertStudent(student);
		Student retrievedStudent = service.getStudent(insertedStudent.getId());
		assertNotNull(retrievedStudent);
	}

	@Test
	public void testInsertStudent_withoutPassport() {
		Student student = createStudent("dummy@dummy.com", "Doe", null);
		Student insertedStudent = service.insertStudent(student);
		Student retrievedStudent = service.getStudent(insertedStudent.getId());
		assertNotNull(retrievedStudent);
	}

	private Student createStudent(String email, String name, Passport passport) {
		Student student = new Student();
		student.setEmail(email);
		student.setName(name);
		student.setPassportId(passport);
		return student;
	}

	@After
	public void printAllDataAfterTest() {
		System.out.println(service.getAllStudents());
	}
}
```
