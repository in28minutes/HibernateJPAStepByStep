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

Assumptions
- Student can be in multiple Projects. A Project can have multiple Students. Many to Many.
- Student can have one Passport and vice versa. One to One.
- One Project can have Many Tasks. But one Task is associated with One Project Only. Many to One.
