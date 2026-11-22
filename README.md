# Project design report

## Emilija Zubinaite 2930285 Mia Lydia Lehes 2898887 Jozef Hvolka 2920398 David Grosu 2921387 Group 15

Submitted on November 11, 2026

# Tasks

The document at docs/SHARE.md represents the list of tasks of each teammate in the project. Each one has his own tasks.

## Bachelor Business Analytics Vrije Universiteit Amsterdam Amsterdam, The Netherlands

1


## Contents


## 1 Dataset

For this project we chose the student performance dataset from Canvas. It is a CSV file with around 100 records and 15 columns, and each rowshows howa student did in one of their courses. We picked it because the school setting is easy for us to relate to, and because the data seems to contain several clear real-world entities, such as schools, students, teachers and courses, which should make it a good fit for an object-oriented design.

## 2 Dataset analysis

## 2.1 What each row represents

A row is a result of a student in one course. For example, record 3 says that Michael Cooper (S001) from Robinson College took Mathematics with teacher Kathrine Moore, studied 19.4 hours a week, missed 12 classes, and got 67. So, a row is a student and a course together, with extra information about how studying for that course went.

## 2.2 What information is stored in each column

The first column, record_id, simply numbers the rows. Thenext threecolumns, school_id, school_name, and school_city, describe the school. They are followed by four columns about the student, namely student_id, student_name, gender, and age. The course is described by course_id and course_name, and the teacher by teacher_id and teacher_name. The last three columns are related to performance. Thecolumn study_hours_week gives the number of hours the student studied perweek for that course, absences give the number of classes they missed, and final_grade gives the grade they received.

## 2.3 Which columns describe the same real-world entity

School_id always has the same school name and city next to it. A student_id always has the same name, gender and age, regardless of which row it is in. A teacher_id always comes with the same teacher name, and a course_id with the same course name. Study hours, ab- sences, and grades are different, because they do not belong to the student or the course alone, but to that student in that one course.

## 2.4 Which columns naturally belong together

After looking at the dataset, it becomes clear that certain columns always appear together. For example, school name, ID and city, or a student’s name, ID, gender and age. Because this information is repeated in every row, it would be more efficient to store it only once, in seperate groups for school, student, teacher, course.

## 2.5 What relationships appear to exist within the data

Looking through the rows, a few relationships between these groups can be seen. A student always appears with the sameschool, which suggests that each student belongs toone school, while a school can have many students. The same student also shows up in several rows with different courses, so one student can have multiple results. Each course seems to be linked to one teacher, although a teacher can appear with more than one course.


## 3 Class Identification

Based on our analysis of the data, we decided on five classes, namely School, Student, Teacher, Course, and Result.

## 3.1 Why the class is required

The sameschool ID, nameand city are repeated inmany rows, soaSchoolclass allowseach school to be stored only once.

Student is themost important class, because the wholedataset is about howstudents perform. A student can appear in several rows, but their name, gender, ID and age stay the same each time, so it makes sense to keep those details in one object.

Each teacher ID always appears with the same teacher name, and some teachers are linked to more than one course. A Teacher class therefore means the teacher’s details do not have to be repeated for every course.

Course is needed because each course ID always comes with the same course name and the

same teacher, so a course looks like something that exists apart from any single student. Result is neededbecausethestudyhours, absencesandfinal gradedonotbelong tothestudent alone, since a student has different values in each course. They do not belong to the course

alone either, because each student in a course has their own values.

## 3.2 Which columns from the dataset belong to that class

The columns school_id, school_name and school_city belong to School. The columns student_id, student_name, genderandagebelong toStudent,andteacher_idandteacher_name belong toTeacher. Coursegetscourse_idandcourse_name. Result is record_id, study_hours_week, absences and final_grade.

## 3.3 The responsibility of the class within the overall system

Each class has its own role within the system. School is responsible for storing the details of a school, such as its ID, name, and city. The Student stores a student’s personal details, including name, ID, gender, and age. Teacher stores the details of a teacher, such as their ID and name. Course is responsible for a course’s ID and name. Result represents one student’s performance in one course. It stores the study hours, absences and final grade and connects the student to that course.

## 4 Attribute Identification

Most of the attributes are strings (school_id, school_name, school_city, student_id, stu- dent_name, gender, teacher_id, teacher_name, course_id, course_name). Integer attributes include: age, record_id, absences and final_grade. There is only one float attribute, which is study_hours_week. All of the attributes are private so that they can not be accessed or modi- fied from outside the class.

## 4.1 Student

Attributes: student id, student name, gender, age


| Attribute | Data type | Explanation |
| --- | --- | --- |
| student_id | String | Individual to each student (ex: S035) |
| student_name | String | First and last name, individual to each student (ex: Brandy Cox) |
| gender | String | Two options: M or F |
| age | Integer | Age ranging from 14-18, stays the same for a student |

## 4.2 Course

Attributes: course id, course name

| Attribute | Data type | Explanation |
| --- | --- | --- |
| course_id | String | Different for each course (ex: CRS001) |
| course_name | String | Dependent on the course ID, individual (ex: Chemistry) |

## 4.3 School

Attributes: school id, school name, school city

| Attribute | Data type | Explanation |
| --- | --- | --- |
| school_id | String | Each school has a separate ID (ex: SCH01-SCH04) |
| school_name | String | Each school has a separate name (ex: Robinson College) |
| school_city | String | Ex: Lake Joyside |

## 4.4 Teacher

Attributes: teacher id, teacher name

| Attribute | Data type | Explanation |
| --- | --- | --- |
| teacher_id | String | Individual to each teacher (ex: T003) |
| teacher_name | String | Individual first and last name (ex: Katherine Moore) |

## 4.5 Result

Attributes: record id, student id, study hours per week, absences, final grade

| Attribute | Data type | Explanation |
| --- | --- | --- |
| record_id | Integer | Ranges from 1-100 |
| student_id | String | Individual to each student, to access a student’s results |
| study_hours_week | Float | Student’s study hours per course (ex: 2,8; 19) |
| absences | Integer | Student’s absences per course |
| final_grade | Integer | Student’s final grade per course (ex: 18) |


## 5 Method Identification

Every attribute is private and has a getter and a setter. Each setter calls a private validation helper before it changes the value. All attributes are kept private so that no value can change without passing validation: an invalid grade or negative absences can never enter the program. Linked objects also stay consistent, because a result can only be added or removed through methods that update both the student and the course it belongs to. The lists below present the behaviours of each class beyond these getters and setters. validation helper before it changes the value. The lists belowpresent the behaviors of each class beyond these getters and setters.

## 5.1 Student

- average_grade(): Average grade over all courses.

- total_absences(): Total absences over all courses.

- is_at_risk(grade_limit, absence_limit): True if theaveragegradeis belowgrade_limit or the total absences are above absence_limit.

- add_result(result): Links a new result to the student.

- remove_result(result): Removes a result from the student.

## 5.2 Course

- average_grade(): Average grade of all students in the course.

- highest_grade(): Highest grade in the course.

- lowest_grade(): Lowest grade in the course.

- pass_rate(pass_mark): Percentage of students who passed.

- add_result(result): Links a new result to the course.

- remove_result(result): Removes a result from the course.

## 5.3 School

- load_from_csv(path): Staticmethodthat readstheCSVfile and returnsthe list ofSchool objects with all linked objects.

- add_student(student), add_course(course), add_teacher(teacher): Add an object to the school.

- find_student(student_id): Returns the student with the given ID.

- average_grade(): Average grade over all students and courses.

- top_students(n): Returns a list of the top n students by average grade.

- at_risk_students(grade_limit, absence_limit): Returns all students forwhomis_at_risk() is True.

- search_results(course, min_grade): Returns all results that match the given criteria.


- sort_results(key): Returns the results sorted by final grade, absences or study hours.

- remove_result(result): Removes a result and updates the linked student and course.

## 5.4 Teacher

- add_course(course): Assigns a course to the teacher.

- student_count(): Returns the number of students taught by this teacher.

- average_grade(): Average grade of all students taught by this teacher.

- course_summary(): Returns each course of the teacher with its number of students and average grade.

## 5.5 Result

- is_passed(pass_mark): True if the grade is at or above the pass mark.

- update_grade(new_grade): Updates the grade after validation.

- update_absences(new_absences): Updates the absences after validation.

- matches(course, min_grade): True if the result meets all given search criteria.

## 5.6 Special Methods

All classes define __init__(), which creates the object and validates its starting values, __str__(), which returns a readable description used to display the object, and __eq__(), which compares two objects by their unique ID. The classes that hold other objects also define __len__() and __contains__():

- School: __len__() returns the number of students in the school, and __contains__() checks if a student belongs to the school.

- Course: __len__() returnsthenumberofstudentsenrolled in thecourse, and__contains__() checks if a student is enrolled in the course.

- Teacher: __len__() returnsthenumberofstudents taughtbythe teacher, and__contains__() checks if a student is taught by the teacher.

- Student: __len__() returnsthenumberofcoursesthestudent takes, and__contains__() checks if the student takes a given course.

Result defines neither, because it represents a single result and holds no other objects.

## 6 Class Relationship Identifications

This section describes howthe five classes are linked. It first explains howthe classes avoid repeating data, thendescribes the aggregation, association andcomposition relationships, and ends with the decision not to use inheritance.


## 6.1 Linking Objects Instead of Copying Data

In the CSV file, every row repeats the data of the school, the student, the course and the teacher. The classes do not copy this data into each other. Instead, each object is stored once and linked tothe objects it belongs with. ASchoolkeeps lists of its students, coursesand teach- ers, so the school name and city are stored only once, in the School object, and not in every student. A Teacher keeps a list of their courses, and a Student and a Course each keep a list of their results. A Result keeps the student_id from the CSV file. When the objects are cre- ated, School.load_from_csv() uses this ID to find the right student and links the result to both that student and its course through Student.add_result() and Course.add_result(). If a school’s data changes, it changes in one place and every linked object sees the new value.

## 6.2 Aggregation

There is an aggregation relationship between School and Student, between School and Course, and between School and Teacher. A school hasmany students, courses and teachers, and each of them belongs to one school. The school acts like a box that groups these objects together. However, a student is still an independent person with their own attributes, such as name, gender and age, and the same holds for courses and teachers. If a school were deleted, these objects would still make sense on their own, so the school does not own them.

## 6.3 Association

There is an association between Course and Teacher. Each course is taught by one teacher, and a teacher can teach several courses. They are separate objects: the course does not own the teacher, and the teacher does not own the course.

There is alsoanassociation betweenResultandCourse. Each result belongs toonecourse, andacoursehasmany results. Thecourseuses its results tocompute its average grade, highest and lowest grade and pass rate, but it does not own them. A result already has one owner, the student (see Section 6.4), and in UML a part can belong to only one composite.

There is no direct link betweenTeacherandStudent. A teacher reaches their students indir- ectly, throughtheircoursesandthe resultsof thosecourses. MethodssuchasTeacher.student_count() and Teacher.average_grade() follow this path. This way, no student is stored twice.

## 6.4 Composition

There is a composition relationship between Student and Result. A result, which con- tains the final grade, absences and study hours, has no meaning on its own: a grade without the student who earned it is useless. A result is therefore created for one student and is de- leted together with that student. Only the student is the owner, because a result is a record of that student’s performance. When a result is removed, School.remove_result() calls both Student.remove_result() and Course.remove_result(), so the course’s list of results stays correct as well.

## 6.5 Inheritance

Student and Teacher both have an ID and a name, so a common Person base class was considered. It is not used, because the two classes share only these two attributes and none of the methods in Section 5. A base class would add an extra layer with very little reuse. This repeatstwoattribute definitions in thecode, but nodata is repeated, becauseevery studentand


teacherobjectstores itsownvaluesonlyonce. Inheritance is alsonotusedbetweenSchooland Student: a student belongs to a school, but a student is not a school.

## 7 Identify Functional Requirements

Theprogramreads all data fromtheCSV file intoobjectsandsupportsthe following. Each re-

quirementendswith themethods fromSection5that carry it out (→). Getters areusedwherever data is only displayed, for example School.get_students() in requirement 4.

- 1. Load all records from the CSV file and create the School, Student, Teacher, Course and Result objects.

→__init__()of all classes, School.load_from_csv(), School.add_student(), School.add_cours School.add_teacher(), Student.add_result(), Course.add_result(), Teacher.add_course()

- 2. Display all students, schools, courses or teachers.

→__str__() of all classes

- 3. Search for a student by ID and show their school, courses and grades.

→School.find_student(), __eq__(), Student.__str__()

- 4. Display all students of a given school.

→School.get_students(), School.__len__(), School.__contains__()

- 5. Search results using multiple criteria (e.g. school, course and minimum grade).

→School.search_results() (called on the chosen school), Result.matches()

- 6. Show the average, highest and lowest grade of a course.

→Course.average_grade(), Course.highest_grade(), Course.lowest_grade()

- 7. Compare the average grade of the schools.

→School.average_grade()

- 8. Show the courses of a teacher with their number of students and average grade.

```
→Teacher.course_summary(), Teacher.student_count(), Teacher.average_grade(),
Course.__len__(), Course.average_grade()
```

- 9. Calculate the pass rate of each course for a given pass mark.

→Course.pass_rate(), Result.is_passed()

- 10. Identify at-risk students (low average grade or many absences).

- 11. Display the top n students by average grade, overall or per school.

→School.top_students(), Student.average_grade()

- 12. Sort results by final grade, absences or study hours.

→School.sort_results()

- 13. Add a new result with validation of all values.

→Result.__init__()withthevalidation helpers, Student.add_result(), Course.add_result()

- 14. Update the grade or absences of a result with validation.

→Result.update_grade(), Result.update_absences()


- 15. Remove a result and update the linked student and course.

→School.remove_result(), Student.remove_result(), Course.remove_result()
