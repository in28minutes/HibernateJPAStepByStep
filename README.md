# HibernateJPAStepByStep

##Entities

Student
- id
- passport_id
- name
- email

Passport
- id
- number
- issued_country

Project
- id
- name

StudentProject
- id
- student_id
- project_id  (Not really needed!!!)
- task_id

Task
- id
- project_id
- desc
- start_date

Assumptions
- Student can be in multiple Projects. A Project can have multiple Students. Many to Many.
- Student can have one Passport and vice versa. One to One.
- One Project can have Many Tasks. But one Task is associated with One Project Only. Many to One.

##Overview
- Working with both Object-Oriented software and Relational Databases can be cumbersome and time consuming. Development costs are significantly higher due to a paradigm mismatch between how data is represented in objects versus relational databases.
- http://www.agiledata.org/essays/dataModeling101.html and http://en.wikipedia.org/wiki/Data_modeling are good starting points for understanding these data modeling principles.
- Hibernate takes care of the mapping from Java classes to database tables, and from Java data types to SQL data types.
- Also data query and retrieval facilities 
- Hibernate’s design goal is to relieve the developer from 95% of common data persistence-related programming tasks
- Hibernate is not a silver bullet. Not meant for application relying heavily on Stored Procedures.

http://docs.jboss.org/hibernate/orm/5.0/userGuide/en-US/html_single/images/overview.png

##History
- JPA 2.0 was released with the specifications of JAVA EE6 => JSR 317.
- JPA 2.1 was released with the specification of JAVA EE7 => JSR 338.
- SessionFactory (org.hibernate.SessionFactory) : A thread-safe (and immutable) representation of the mapping of the application domain model to a database. Acts as a factory for org.hibernate.Session instances. A SessionFactory is very expensive to create; there should be only one SessionFactory for an application for a given database. Maintains services that Hibernate uses across all Sessions such as second level caches, connection pools, transaction system integrations, etc.
- Session (org.hibernate.Session) : A single-threaded, short-lived object conceptually modeling a "Unit of Work"[PoEAA]. Wraps a JDBC java.sql.Connection. Acts as a factory for org.hibernate.Transaction instances. Maintains a generally "repeatable read" persistence context (first level cache) of the application's domain model.

##Key Components

- Entity : A JPA entity class is a POJO , i.e. an ordinary Java class that is annotated as having the ability to represent objects in the database
- EntityManager : Manages the persistence operations on Entities.
- EntityTransaction : One-to-one relationship with EntityManager. For each EntityManager, operations are maintained by EntityTransaction class.
- EntityManagerFactory : Factory class of EntityManager. 
- Transaction (org.hibernate.Transaction) TODO - A single-threaded, short-lived object used by the application to demarcate individual physical transaction boundaries. It acts as an abstraction API to isolate the application from the underling transaction system in use (JDBC, JTA, CORBA, etc).
- Domain Model : The POJO should have a no-argument constructor. Both Hibernate and JPA require this.

##Important Annotations

- @Entity : Indicates the class as an entity or a table.
- @Table  : Indicates table name.

- @Id : Identifies the column as a primary key of the Entity.
- @GeneratedValue : Indicates that the column will be auto generated.
- @Transient : Column will not be persisted.
- @Column : Used to specify properties on a Entity Column

- @JoinColumn : Used to specify the column used in the Join.

- @ManyToMany 	Indicates a many-to-many relationship between the join Tables.
- @ManyToOne 	Indicates a many-to-one relationship between the join Tables.
- @OneToMany 	Indicates a one-to-many relationship between the join Tables.
- @OneToOne 	Indicates a one-to-one relationship between the join Tables.

- @NamedQueries 	Indicates list of named queries.
- @NamedQuery 	Indicates a Query using static name.

# Retrieving Data
The Java Persistence API provides the following methods for querying entities.
- The Java Persistence query language (JPQL) is a simple, string-based language similar to SQL used to query entities and their relationships. 
- The Criteria API is used to create typesafe queries using Java programming language APIs to query for entities and their relationships. 

Both JPQL and the Criteria API have advantages and disadvantages:
- Just a few lines long, JPQL queries are typically more concise and more readable than Criteria queries. Developers familiar with SQL will find it easy to learn the syntax of JPQL. 
- JPQL named queries can be defined in the entity class using a Java programming language annotation or in the application’s deployment descriptor. Criteria queries are typesafe and therefore don’t require casting, as JPQL queries do.
- JPQL queries are not typesafe, however, and require a cast when retrieving the query result from the entity manager. This means that type-casting errors may not be caught at compile time. JPQL queries don’t support open-ended parameters.
- Criteria queries allow you to define the query in the business tier of the application. Although this is also possible using JPQL dynamic queries, Criteria queries provide better performance because JPQL dynamic queries must be parsed each time they are called. 
-  The Criteria API is just another Java programming language API and doesn’t require developers to learn the syntax of another query language. Criteria queries are typically more verbose than JPQL queries and require the developer to create several objects and perform operations on those objects before submitting the query to the entity manager.

###Find
###Delete
##Java Persistence Query language
- JPQL syntax is similar to the syntax of SQL. 
- JPQL works with Java Entity Classes

```

SELECT ... FROM ...
[WHERE ...]
[GROUP BY ... [HAVING ...]]
[ORDER BY ...]

DELETE FROM ... [WHERE ...]
 
UPDATE ... SET ... [WHERE ...]

MAX
Between A and B

LIKE 'R%'
ORDER BY columnname ASC

@NamedQuery(query = "", name = "")\
entitymanager.createNamedQuery
entitymanager.createQuery("Complete Query");

Queries with Parameters

@NamedQueries({
    @NamedQuery(name="name1",
                query="Query1"),
    @NamedQuery(name="name2",
                query="Query2"),
}) 


```
##Criteria Query
TODO

##Eager and Lazy Fetching

# Bidirectional relationships
- The inverse side of a bidirectional relationship must refer to its owning side by using the mappedBy element of the @OneToOne, @OneToMany, or @ManyToMany annotation. The mappedBy element designates the property or field in the entity that is the owner of the relationship.
- The many side of many-to-one bidirectional relationships must not define the mappedBy element. The many side is always the owning side of the relationship.
- For one-to-one bidirectional relationships, the owning side corresponds to the side that contains the corresponding foreign key.
- For many-to-many bidirectional relationships, either side may be the owning side.

## Cascading
- Entities that use relationships often have dependencies on the existence of the other entity in the relationship. For example, a line item is part of an order; if the order is deleted, the line item also should be deleted. This is called a cascade delete relationship.
- The javax.persistence.CascadeType enumerated type defines the cascade operations that are applied in the cascade element of the relationship annotations. Table 32-1 lists the cascade operations for entities.

##Embeddable Classes
- Embeddable classes have the same rules as entity classes but are annotated with the javax.persistence.Embeddable annotation instead of @Entity.

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

## Mapping Strategies : Inheritance Relationships to Database Entitities
You can configure how the Java Persistence provider maps inherited entities to the underlying datastore by decorating the root class of the hierarchy with the annotation javax.persistence.Inheritance. The following mapping strategies are used to map the entity data to the underlying database:
- A single table per class hierarchy
- A table per concrete entity class
- A “join” strategy, whereby fields or properties that are specific to a subclass are mapped to a different table than the fields or properties that are common to the parent class



##About in28Minutes
- At in28Minutes, we ask ourselves one question everyday. How do we create more effective trainings?
- We use Problem-Solution based Step-By-Step Hands-on Approach With Practical, Real World Application Examples. 
- Our success on Udemy and Youtube (2 Million Views & 12K Subscribers) speaks volumes about the success of our approach.
- While our primary expertise is on Development, Design & Architecture Java & Related Frameworks (Spring, Struts, Hibernate) we are expanding into the front-end world (Bootstrap, JQuery, Angular JS). 

###Our Beliefs
- Best Course are interactive and fun.
- Foundations for building high quality applications are best laid down while learning.

###Our Approach
- Problem Solution based Step by Step Hands-on Learning
- Practical, Real World Application Examples.
- We use 80-20 Rule. We discuss 20% things used 80% of time in depth. We touch upon other things briefly equipping you with enough knowledge to find out more on your own. 
- We will be developing a demo application in the course, which could be reused in your projects, saving hours of your effort.
- All the code is available on Github, for most steps.

###Useful Links
- [Our Website](http://www.in28minutes.com)
- [Youtube Courses](https://www.youtube.com/user/rithustutorials/playlists)
- [Udemy Courses](https://www.udemy.com/user/in28minutes/)
- [Facebook](http://facebook.com/in28minutes)
- [Twitter](http://twitter.com/in28minutes)
- [Google Plus](https://plus.google.com/u/3/110861829188024231119)

###Other Courses
- [Spring Framework](https://www.udemy.com/spring-tutorial-for-beginners/)
- [Maven](http://www.in28minutes.com/p/maven-tutorial-for-beginners.html)
- [Eclipse](http://www.in28minutes.com/p/eclipse-java-video-tutorial.html)
- Java
  * [Java](https://www.youtube.com/watch?v=Y4ftqcYVh5I&list=PLE0D4634AE2DFA591&index=1)
  * [Java Collections](http://www.in28minutes.com/p/java-collections-framework-video.html)
  * [Java OOPS Concepts](https://www.udemy.com/learn-object-oriented-programming-in-java/) 
- [Design Patterns](http://www.in28minutes.com/p/design-patterns-tutorial.html)
- [JUnit](https://www.udemy.com/junit-tutorial-for-beginners-with-java-examples/)
- [C](https://www.udemy.com/c-tutorial-for-beginners-with-puzzles/)
- [C Puzzles](https://www.udemy.com/c-puzzles-for-beginners/)
- [Javascript](https://www.youtube.com/watch?v=6TZdD-FR6CY)
- [More Courses on Udemy](https://www.udemy.com/user/in28minutes/)
  * Java Servlets and JSP : Your first web application in 25 Steps
  * Learn Spring MVC in 25 Steps 
  * Learn Struts in 25 Steps 
  * Learn Hibernate in 25 Steps
  * 10 Steps to Professional Java Developer
- [Java Interview Guide](http://www.in28minutes.com/p/buy-our-java-interview-guide.html)
  * Core Java
  * Advanced Java
  * Spring, Spring MVC
  * Struts
  * Hibernate
  * Design Patterns
  * 400+ Questions
  * 23 Videos
