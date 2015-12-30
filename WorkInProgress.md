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
import javax.persistence.OneToOne;
import javax.persistence.Table;

@Entity
@Table(name = "Student")
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
src\main\java\com\in28minutes\hibernate\service\StudentService.java
```
package com.in28minutes.hibernate.service;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;

import org.springframework.stereotype.Repository;

import com.in28minutes.hibernate.model.Passport;
import com.in28minutes.hibernate.model.Student;

@Repository
public class StudentService {

	@PersistenceContext
	private EntityManager entityManager;

	public Student getStudent(final long id) {
		return entityManager.find(Student.class, id);
	}

	public Student insertStudent(Student student) {
		Passport passport = entityManager.merge(student.getPassport());
		Student persistedStudent = entityManager.merge(student);
		return persistedStudent;
	}

	public Student updateStudent(Student student) {
		entityManager.merge(student);
		return student;
	}

}
```
src\main\java\com\in28minutes\hibernate\service\StudentService2.java
```
package com.in28minutes.hibernate.service;

import javax.persistence.EntityManager;
import javax.persistence.PersistenceContext;
import javax.transaction.Transactional;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.in28minutes.hibernate.model.Student;

@Service
public class StudentService2 {

	@Autowired
	StudentService service;

	@PersistenceContext
	private EntityManager entityManager;

	@Transactional
	public Student insertStudent(Student student) {
		return service.insertStudent(student);
	}

}
```
src\main\resources\config\data.sql
```
INSERT INTO passport VALUES (201,'L1234567','India');

INSERT INTO student VALUES (101,201,'Jane', 'jane@doe.com');

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

	@Autowired
	private StudentService2 service2;

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

		Student insertedStudent = service2.insertStudent(student);
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
}
```
