# Design spike 1 for [jiit](https://github.com/ivogeorg/jitt)

**Date:** 2021-01-15

## 1. Instructor stories

1. I want to ask my students a small number of _warmup questions_ before every class period.
2. Answering the questions shouldn't take a student more than 25 minutes.  
3. I went to be able to quickly review the answers, possibly group them, and select some as discussion triggers for class.  
4. I want to be able to load up questions for the class periods for a whole semester, and have the flexibility to adjust to the way the specific class is moving through the material.  
5. I want to be able to import questions:
   1. From csv, spreadsheet, etc. and then arrange them.  
   2. From a previous edition of the same class.  
6. I want to be able to export questions to csv or spreadsheet.  
7. I want to be able to edit the text of a question, both while it is unassigned (that is, in a _question bank_) and assigned (so the unassigned and assigned can differ).  

**TODO:** Question banks.  


## 2. Instructor data

1. (GUID) Instructor ID.  
2. Name. _Need to have a disambiguation mechanism during registration and GUID assignment. People should be uniquely identified. That should include school, department, email address, possibly DOB, possibly CC._
3. (GUID) Email address. _Will have to handle multiple workplaces for instructors._   
4. (GUID) Employer school ID. _See "Email address"._   
5. (UID per "Employer school ID") Employer department ID. _See "Email address"._    
6. (GUID) Login name.  
7. _(encripted)_ Password. _Support changing the password._    
8. (set) Classes.

## 3. Class data

1. (GUID) Course catalog ID.  
2. (GUID) Offering school ID.  
3. (UID per "Offering school ID") Offering department ID.  
4. (UID per "Course catalog ID") Term ID.  _There are semesters, winterims, maymesters, quarters, and other varieties. May need to be free-form._
5. (UID per "Offering school ID" and "Offering department ID") Section ID.  
6. _(optional)_ Catalog description.
7. _(optional)_ Syllabus.  
8. _(optional)_ Schedule.  
9. (set) Students.  
10. (set) Meetings.  

## 4. Student data

1. (GUID) Student ID.  
2. Name.  
3. (GUID) School email address.
4. (GUID) Login name.
5. _(encripted)_ Password. _Support changing the password._    
6. (set) Classes.  

## 5. Meeting data

_Meetings_ are class periods, where the instructor and the students meet, in person or online.

1. (UID per "Instructor ID", "Course catalog ID", "Term ID", and "Section ID") Meeting ID.
2. (UID per "Meeting ID") Order index.  
3. _(optional)_ Date. _Should be a standard yyyy-mm-dd format throughout._   
4. (set) Questions.  

## 6. Question data

1. (UID per "Instructor ID", "Course catalog ID", "Term ID", and "Section ID") Question ID.  
2. Question text. _Should multimedia questions (that is, "cards") be supported? This will vastly complicate the design._  
2. (set) Answers.  

## 7. Answer data

1. (UID per "Question ID" and "Student ID") Answer ID.
2. Selected for class. _This might require more input from an experienced user._  

## 8. Student stories

1. I want to have an easy interface to answer _warmup questions_ on any device.  
2. I want to have alerts about the per-class deadlines for _warmup questions_.  
3. I want to be able to edit my answers before the corresponding deadline.  
4. I want to have the questions sent to me over email and apps that I use (e.g. Teams, FB Messenger).  

## 9. Technical details

A web application with Node.js, React, supported initially by a relational database (TBD).

### Questions

1. What about mobile?  
2. 
