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
- project_id
- role_id

Role
- id
- desc

Task
- id
- desc
- planned_end_date

StudentProjectTask
- student_id
- project_id
- task_id

Assumptions
- 1 Task is performed by 1 Student only at a time and belongs to 1 Project. 
- A Student can be part of multiple Projects and have Multiple Tasks.
- A Project can have multiple students and multiple tasks.
- One Student has One Passport
