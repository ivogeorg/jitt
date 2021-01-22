# Design spike 2 for [jiit](https://github.com/ivogeorg/jitt)





# User stories

## 1. Instructor stories

1. I want to ask my students a small number of _warmup questions_ (see [Question[(#question)) before every class period (see [Meeting](#meeting).
2. Answering (see [Answer](#answer)) the questions shouldn't take a student more than 25 minutes.  
3. I want to be able to quickly review the answers, possibly group them, and select some as discussion triggers (aka hooks, engagement elements, etc.) for class.  
   1. View all answers to a given question in one compact list, differentiate each student's response with different subtle highlight. (**TODO:** Clarify.)    
   2. Sort student responses in a few ways. For example:
      1. By timestamp.
      2. Alphabetical by last name.
      3. Randomized. 
4. I want to be able to load up questions for the class periods for a whole semester, and have the flexibility to adjust to the way the specific class is moving through the material.  
5. I want to be able to import questions:
   1. From csv, spreadsheet, etc. and then arrange them.  
   2. From a previous edition of the same class.  
6. I want to be able to export questions to csv or spreadsheet.  
7. I want to be able to edit the text of a question, both while it is unassigned (that is, in a [question bank](#11-question-banks)) and assigned (so the unassigned and assigned can differ).  
8. I want to be able to optionally grade student responses.  
   1. Select mode:
      1. Auto-grade for participation. (**TODO:** Provide a sample rubric.)
      2. Auto-grade for correctness. (**TODO:** How?)  
      3. No auto-grade. (**TODO:** Is this equivalent to "manual" or to "no grading"? Are the two to be distinguished?)  
   2. Customize point value per question. (**TODO:** Example?)   
   3. Manually adjust the grade for a student response at the same time (same view) as reviewing responses. Simple +/- buttons to increase/decrease grades.  
9. Email individual students (from my default email client) about their response with one click from the basic "view responses" view.  
   1. Emails are based on templates.
   2. A template should be prifilled with available slot data. For example:
      1. Subject line.
      2. Greeting.
      3. The question.
      4. The addressee student's answer.
      5. Instructor signature.
   3. This feature may be scaled with clustering (a la [sense education](https://www.sense.education/):
      1. Aggregation per question.  
      2. Clustering of a question's answers.
      3. Collecting instructor feedback on each cluster.
      4. Assembling instructor feedback for all questions for each student.
      5. Mailing each student.
10. During class meeting, I want to show some curated information from the warmups:  
    1. Question text and images (remind students, or for students who didn't do it) (**TODO:** Reduce conflated features.)
    2. Aggregate responses categorized by the instructor when reading a sample of student responses. (see slide examples) (**TODO:** Where were they sent?)
    3. Anonymous responses from some students, especially the "useful wrong answers."  
11. Ideally the student view of questions is LMS embeddable.  

### 1.1. Question banks

1. Questions can be flagged with conceptual topics.  
2. Questions can be flagged with question level, most likely to follow adopted and/or conventional scale of academic difficulty. 
   1. For example, for physics, these can be:
      1. Conceptual.  
      2. Algebra-based.   
      3. Calculus-based. 
      4. Upper-division.  
      5. Graduate-level.  
   2. In general, these should be configurable by the instructor.  
3. Quesitons can be searched by users based on:
   1. Author.  
   2. Topic.  
   3. Question level.  
   4. Date.  
   5. Course name.   
   

## 2. Student stories

1. I want to have an easy interface to answer _warmup questions_ on any device.  
2. I want to have alerts about the pre-class deadlines for _warmup questions_.  
3. I want to be able to edit my answers before the corresponding deadline.  
4. I want to have the questions sent to me over email and apps that I use (e.g. Teams, FB Messenger).  
5. I want to be able to review questions, my responses and the grades I got, after the deadline.  

# Data

## 1. Instructor

1. (GUID) Instructor ID.  
2. Name. _Need to have a disambiguation mechanism during registration and GUID assignment. People should be uniquely identified. That should include school, department, email address, possibly DOB, possibly CC._
3. (GUID) Email address. _Will have to handle multiple workplaces for instructors._   
4. (GUID) Employer school ID. _See "Email address"._   
5. (UID per "Employer school ID") Employer department ID. _See "Email address"._    
6. (GUID) Login name.  
7. _(encripted)_ Password. _Support changing the password._    
8. (set) Classes.

## 2. Class

_Notes: Class is an instance (sometimes synonymous to "section") of a course, in time and place._

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

## 3. Student

1. (GUID) Student ID.  
2. Name.  
3. (GUID) School email address.
4. (GUID) Login name.
5. _(encripted)_ Password. _Support changing the password._    
6. (set) Classes.  

## 4. Meeting

_Notes: Meetings are class periods, where the instructor and the students meet, in person or online._

1. (UID per "Instructor ID", "Course catalog ID", "Term ID", and "Section ID") Meeting ID.
2. (UID per "Meeting ID") Order index.  
3. _(optional)_ Date. _Should be a standard yyyy-mm-dd format throughout._   
4. (set) Questions.  
5. Meeting view, which displays:  
   1. Full question
   2. Aggregated response pattern, a.k.a. clustering (37% gave this kind of response, 29% gave that kind of response, 20% showed confusion).
   3. Selected student responses presented anonymously. Responses can be displayed in a certain order.

## 5. Question

1. (UID per "Instructor ID", "Course catalog ID", "Term ID", and "Section ID") Question ID.  
2. Question text. _Should multimedia questions (that is, "cards") be supported? This will vastly complicate the design._  
3. Cards:  
   1. Mathematical expressions  
   2. HTML links  
   3. Basic rich-text editor (lists, italics, bold, etc.)  
   4. Embed images  
2. (set) Answers.  

## 6. Answer

1. (UID per "Question ID", "Student ID", "Course ID", "Term ID") Answer ID.
2. Selected for class. _This might require more input from an experienced user._  
3. Cards:  
   1. Mathematical expressions.
   2. Images.
4. Responses flagged to be presented in the meeting view. The system tracks and displays how many times a particular student has has their answer flagged for display.

# Views

1. View responses.

## Cards

_Notes: Cards are semi-independent embeddable interactive units of rich multimedia content._

1. What is a card good for:
2. What can be a card:
   1. Questions, by default. 
   2. Answers, if interactive may optionally be, especially in the interactive case.
   3. Curated results in class.
3. What may a card contain:
   1. Textual paragraphs.
   2. Formulae.
   3. Images.
   4. Embedded video.
4. In general, cards should support a full (initially 2D) pallete of formatting options, organized as a set of content-dependent rich editors. _Note that the **bang-for-the-buck** declines sharply for such editors. The range is from **HTML** (or **LaTeX**, or **Photoshop**) to **Markdown**. Err on the side of simplicity._

# Implementation notes

A web application with Node.js, React, supported initially by a qrelational database (TBD).

# Open questions

1. What about mobile?  
2. Can question-answering be in the form of short dialogue? (Looking forward to LA Bot)
3. Instead of email, may the questions be served over Teams?
4. How does one embed an app in Canvas? (In general, the logistics might not be worth it, if we can bypass with social media.)

