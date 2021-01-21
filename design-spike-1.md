# Design spike 1 for [jiit](https://github.com/ivogeorg/jitt)

**Date:** 2021-01-15

Table of Contents
=================

* [Design spike 1 for <a href="https://github\.com/ivogeorg/jitt">jiit</a>](#design-spike-1-for-jiit)
  * [1\. Instructor stories](#1-instructor-stories)
  * [2\. Instructor data](#2-instructor-data)
  * [3\. Class data](#3-class-data)
  * [4\. Student data](#4-student-data)
  * [5\. Meeting data](#5-meeting-data)
  * [6\. Question data](#6-question-data)
  * [7\. Answer data](#7-answer-data)
  * [8\. Student stories](#8-student-stories)
  * [9\. Technical details](#9-technical-details)
    * [Questions](#questions)


## 1. Instructor stories

1. I want to ask my students a small number of _warmup questions_ before every class period.
2. Answering the questions shouldn't take a student more than 25 minutes.  
3. I went to be able to quickly review the answers, possibly group them, and select some as discussion triggers for class.  
   1. View all answers to a given question in one compact list, differentiate each student's response with different subtle highlight.  
   2. Sort student responses in a few wasy: timestamp, alphabetical by last name, randomized. 
4. I want to be able to load up questions for the class periods for a whole semester, and have the flexibility to adjust to the way the specific class is moving through the material.  
5. I want to be able to import questions:
   1. From csv, spreadsheet, etc. and then arrange them.  
   2. From a previous edition of the same class.  
6. I want to be able to export questions to csv or spreadsheet.  
7. I want to be able to edit the text of a question, both while it is unassigned (that is, in a _question bank_) and assigned (so the unassigned and assigned can differ).  
8. Grade student responses  
   1. Customize point value per question  
   2. Select "auto-grade for participation," "auto-grade for correctness," or "no auto-grade."  
   3. Manually adjust the grade for a student response at the same time (same view) as reviewing responses. Simple +/- buttons to increase/decrease grades.  
9. Email individual students (from my default email client) about their response with one click from the basic "view responses" view.  
   1. Email is pre-filled with: Subject line, greeting, the question, that student's answer, signature.  
10. During class meeting, instructor shows some curated information from the warmups  
    1. Question text and images (remind students, or for students who didn't do it)
    2. Aggregate responses categorized by the instructor when reading a sample of student responses. (see slide examples)
    3. Anonymous responses from some students, especially the "useful wrong answers."

### Question banks.  
1. Questions can be flagged with coneptual topics
2. Questions can be flagged with "question level" : conceptual, algebra-based, calc-based, upper-division, graduate-level
3. Quesitons can be searched by users based on: author, topic, question level, date, course name

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
5. Meeting view, which displays:  
   1. Full question
   2. Aggregated response pattern, a.k.a. clustering (37% gave this kind of response, 29% gave that kind of response, 20% showed confusion).
   3. Selected student responses presented anonymously. Responses can be displayed in a certain order.

## 6. Question data

1. (UID per "Instructor ID", "Course catalog ID", "Term ID", and "Section ID") Question ID.  
2. Question text. _Should multimedia questions (that is, "cards") be supported? This will vastly complicate the design._  
3. Cards:  
   1. Mathematical expressions  
   2. HTML links  
   3. Basic rich-text editor (lists, italics, bold, etc.)  
   4. Embed images  
2. (set) Answers.  

## 7. Answer data

1. (UID per "Question ID", "Student ID", "Course ID", "Term ID") Answer ID.
2. Selected for class. _This might require more input from an experienced user._  
3. Cards:  
   1. Mathematical expressions  
4. Responses flagged to be presented in the meeting view. The system tracks and displays how many times a particular student has has their answer flagged for display.
   
## 8. Student stories

1. I want to have an easy interface to answer _warmup questions_ on any device.  
2. I want to have alerts about the pre-class deadlines for _warmup questions_.  
3. I want to be able to edit my answers before the corresponding deadline.  
4. I want to have the questions sent to me over email and apps that I use (e.g. Teams, FB Messenger).  
5. I want to be able to review questions, my responses and the grades I got, after the deadline.  

## 9. Technical details

A web application with Node.js, React, supported initially by a relational database (TBD).

### Questions

1. What about mobile?  
2. 
