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


##History
JPA 2.0 was released with the specifications of JAVA EE6 => JSR 317.
JPA 2.1 was released with the specification of JAVA EE7 => JSR 338.

##Key Components

- Entity : A JPA entity class is a POJO , i.e. an ordinary Java class that is annotated as having the ability to represent objects in the database

- EntityManager : Manages the persistence operations on Entities.

- EntityTransaction : One-to-one relationship with EntityManager. For each EntityManager, operations are maintained by EntityTransaction class.

- EntityManagerFactory : Factory class of EntityManager. 

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




Find
Delete


Java Persistence Query language
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

Eager and Lazy Fetching


Criterial Query - Todo


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
