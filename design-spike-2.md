# Design spike 2 for [jitt](https://github.com/ivogeorg/jitt)

```
  _____ _   _   _____  _____   ____   _____ _____  ______  _____ _____ 
 |_   _| \ | | |  __ \|  __ \ / __ \ / ____|  __ \|  ____|/ ____/ ____|
   | | |  \| | | |__) | |__) | |  | | |  __| |__) | |__  | (___| (___  
   | | | . ` | |  ___/|  _  /| |  | | | |_ |  _  /|  __|  \___ \\___ \ 
  _| |_| |\  | | |    | | \ \| |__| | |__| | | \ \| |____ ____) |___) |
 |_____|_| \_| |_|    |_|  \_\\____/ \_____|_|  \_\______|_____/_____/ 
```                                                                    
                                                                       
Table of Contents
=================

* [Design spike 2 for <a href="https://github\.com/ivogeorg/jitt">jiit</a>](#design-spike-2-for-jiit)
* [User stories](#user-stories)
  * [1\. Instructor stories](#1-instructor-stories)
    * [1\.1\. Question banks](#11-question-banks)
    * [1\.2\. Cards](#12-cards)
  * [2\. Student stories](#2-student-stories)
* [Data](#data)
  * [1\. Instructor](#1-instructor)
  * [2\. Class](#2-class)
  * [3\. Student](#3-student)
  * [4\. Meeting](#4-meeting)
  * [5\. Question](#5-question)
  * [6\. Answer](#6-answer)
  * [7\. Digest](#7-digest)
        * [Preliminary notes](#preliminary-notes)
* [Views](#views)
* [Workflows](#workflows)
* [Implementation notes](#implementation-notes)
* [Open questions](#open-questions)


# User stories

## 1. Instructor stories

1. I want to ask my students a small number of _warmup questions_ (see [Question](#question)) before every class period (see [Meeting](#meeting)).
3. I want to be able to quickly review the answers (see [Answers](#answers)), possibly group them, and select some as discussion triggers (aka hooks, engagement elements, etc.) for class.  
   1. View all answers to a given question in one compact list, differentiate each student's response with different subtle highlight. This can be thought of as _manual clustering_. See the following examples:
      Slide | Description
      --- | ---
      <img src="assets/warmup_slides/Slide1.png" width="300" />  |  Contains a summary of a popular answer and a general "Conceptual problems" cluster/pool
      <img src="assets/warmup_slides/Slide2.png" width="300" />  |  Contains two anonymized sample answers
      <img src="assets/warmup_slides/Slide3.png" width="300" />  |  Contains three clusters with brief description
      <img src="assets/warmup_slides/Slide4.png" width="300" />  |  Contains two anonymized sample (parts of) answers
      <img src="assets/warmup_slides/Slide5.png" width="300" />  |  For a multiple-choice question, provides percentages for each answer
   2. _Manual clustering_ may have a very interesting UX/UI, in which, while the instructor is reviewing the answers, they can create clustering on the fly:
      1. If there is already a cluster category matching the highlight for an answer, it is selected to flag the answer.
      2. If an answer does not match any of the already available cluster categories, it is created on the fly, the answer is automatically flagged with it, and the category is added to the available one.
      3. Each set of cluster categories defines one "dimension". Multiple dimensions may be supported easily with the same UX/UI. A visual example:
         ```
         ---------------          ---------------          
         | DIMENSION 1 |          | DIMENSION 2 |          
         ---------------          ---------------          
         | Category 1  |          | Category 1  |          
         ---------------          ---------------          
         | Category 2  |          | Category 2  |          
         ---------------          ---------------          
         | Category 3  |          
         ---------------          
         | Category 4  |          
         ---------------          
         ```
      4. Automatic clustering can be presented in the same UX/UI, just populating the cluster dimensions and categories.
   3. Sort student responses in a few ways. For example:
      1. By timestamp.
      2. Alphabetical by last name.
      3. Randomized. 
4. I want to be able to load up questions for the class periods for a whole semester, and have the flexibility to adjust to the way the specific class is moving through the material. Easily move a question from one WarmUp to another, change the order of WarmUps. Here, "WarmUp" is funcitonally synonymous to class period or "Meeting" (see [Meeting](#meeting)).  
5. I want to be able to import questions:
   1. From csv, spreadsheet, etc. and then arrange them.  
   2. From a previous edition of the same class.  
6. I want to be able to export questions to csv or spreadsheet.  
7. I want to be able to edit the text of a question, both while it is unassigned (that is, in a [question bank](#11-question-banks)) and assigned (so the unassigned and assigned can differ).  
8. I want the option to have the computer assign grades automatically (auto-grade) to student responses.  
   1. Select mode:
      1. Auto-grade for participation. For example, on a 0-2 scale, default to 1 for having provided an answer and 0 for no answer. 
      2. Auto-grade for correctness, for questions which have correct answers (e.g. multiple-choice, single-answer or multiple-answer, questions).
      3. No auto-grade (meaning, manual grading only). 
   2. Customize point value per question. That is, the scale is definable.   
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
      1. Grouping answers per question.  
      2. Clustering of a question's answers.
      3. Collecting instructor feedback on each cluster.
      4. Assembling instructor feedback for for each student (for all answered question).
      5. Mailing feedgack to each student.
10. During class meeting, I want to show some curated information from the warmups:  
    1. Question text and images. These suppot are points of engagement. For example:
       1. Remind students what the question was about.   
       2. Show how many students didn't do it.  
    2. Aggregate responses categorized by the instructor when reading a sample of student responses. (see slide examples above)
    3. Anonymous responses from some students, especially the "useful wrong answers".  
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
   2. This is a first step toward figuring out how to maintain domain knowledge in a hierarchy of academic difficulty.
   3. In general, these levels should be configurable by the instructor, though concensus and normativity are desired.  
3. Quesitons can be searched by users based on:
   1. Author.  
   2. Topic.  
   3. Question level.  
   4. Date.  
   5. Course name.   
   
### 1.2. Cards

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

_Notes: Class is an instance (sometimes synonymous to "section") of a course, in time priod (e.g. semester) and place (school)._

**TODO ([@ivogeorg](https://github.com/ivogeorg)):** Makes sense to have Class ID!

1. (GUID, _composite, partially readable_) Course catalog ID.  
2. (GUID, _composite, partially readable_) Offering school ID.  
3. (UID per "Offering school ID") Offering department ID.  
4. (UID per "Course catalog ID") Term ID.  _There are semesters, winterims, maymesters, quarters, and other varieties. May need to be free-form._
5. (UID per "Course catalog ID" and "Term ID") Section ID.  
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

_Notes: Meetings are class periods, where the instructor and the students meet, in person or online. For example, twice-per-week course over a 15-week semester has 30 meetings. Questions can be associated with meetings, dynamically. Curated results are associated with a meeting, but meeting is a **not** a schema linking entity. "Meeting" may be functionally synonymous to "WarmUp", when the latter means a set of warmup questions for a meeting._

1. (UID per "Section ID") Meeting ID.
2. (UID per "Section ID") Order index.  
3. _(optional)_ Date. _Should be a standard yyyy-mm-dd format throughout._   
4. (set) Questions.  
5. (set) Digests.  

## 5. Question

**TODO ([@ivogeorg](https://github.com/ivogeorg)):** Question types (e.g. narrative, interactive card, multiple-choice, etc.)!

1. (UID per "Instructor ID", "Course catalog ID", "Term ID", and "Section ID") Question ID.  **TODO ([@ivogeorg](https://github.com/ivogeorg)):** Need to lay out the schema to get this one right!
2. Card data:  
   1. Mathematical expressions  
   2. HTML links  
   3. Basic rich-text editor (lists, italics, bold, etc.)  
   4. Embed images  
2. (set) Answers.  

## 6. Answer

**TODO ([@ivogeorg](https://github.com/ivogeorg)):** Answer value scales and grades!

1. (UID per "Question ID", "Student ID", "Course ID", "Term ID", "Section ID") Answer ID.
3. Cards data:  
   1. Mathematical expressions.
   2. Images.
4. Responses flagged to be presented in the meeting view. The system tracks and displays how many times a particular student has has their answer flagged for display.

## 7. Digest

_Notes: A digest is a data product, based on the system data and content. For example, this includes curated answer aggregations._

##### Preliminary notes

1. Various combinations of data. Example:
   1. Question or questions.
   2. Answers.
   3. Metadata. Example:
      1. The answer is selected for anonymous show in class.
2. Various aggregations.  
3. Support for different views. For example:
   1. Meeting view, which displays:  
      1. Full question
      2. Aggregated response pattern, a.k.a. clustering (37% gave this kind of response, 29% gave that kind of response, 20% showed confusion).
      3. Selected student responses presented anonymously. Responses can be displayed in a certain order.
4. Manual clustering (per the instructor story) is also a Digest. So, the dimensions and categories will be held here.

# Views

_Notes: Views are formatted combinations of information, promptings, and editing elements. A view may contain [cards](#12-cards)._

**UNDER REVIEW**

1. View responses.
2. Meeting view, which displays:  
   1. Full question
   2. Aggregated response pattern, a.k.a. clustering (37% gave this kind of response, 29% gave that kind of response, 20% showed confusion).
   3. Selected student responses presented anonymously. Responses can be displayed in a certain order.


# Workflows

_Notes: Workflows should translate into detailed UI specs, e.g. a mesh of views._

1. Pre-meeting warmup questions and instructor review and digest.
2. Real-time in-class questions with automatic answer digest. 


# Implementation notes

1. A web application with Node.js, React, supported initially by a database.  **TODO ([@ivogeorg](https://github.com/ivogeorg)):** Stacks website, MEAN stack.  
2. The data has schema, but should not be as rigid as classic relational schema. Keys where necessary, mappings otherwise. Uniqueness still has to be enforceable.  For example, Digest is associated with Meeting, but need not have a key relationship with it.
3. Taking the previous point further, UIDs per this and that are hard to maintain, whereas GUID generation is super simple. For example, Google's myriad types of search and ad data are based on trillions of GUIDs. Switch to GUIDs. As a result:
   1. Uniqueness need not be enforced up-front, but becomes post factum repairable.
   2. In general, this is the modus operandi of non-relational databases. **TODO ([@ivogeorg](https://github.com/ivogeorg)):** What falls under "No-SQL"?  
4. All entities should have GUIDs. (And, looking forward to STEMGraph, so should concepts)
   1. Tables should still hold the relations and maps.
   2. JSON can be used as fundamental storage format.
5. If questions become cards, and cards are documents, then the database can be document-oriented (e.g. MongoDB). Storage in JSON!
6. 


# Open questions

1. What about mobile?  
2. Can question-answering be in the form of short dialog? (Looking forward to LA Bot)
3. Instead of email, may the questions be served over Teams?
4. How does one embed an app in Canvas? (In general, the logistics might not be worth it, if we can bypass with social media.)
5. The system that is taking shape can easily support real-time question generation, answer digestion, and engagement. Should we make sure the design does not preclude this workflow?
