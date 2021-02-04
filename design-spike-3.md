# Design spike 3 for [jitt](https://github.com/ivogeorg/jitt)

[[taag](https://patorjk.com/software/taag/#p=display&f=Big&t=something)]
```
  _____                                                 
 |_   _|                                                
   | |  _ __    _ __  _ __ ___   __ _ _ __ ___  ___ ___ 
   | | | '_ \  | '_ \| '__/ _ \ / _` | '__/ _ \/ __/ __|
  _| |_| | | | | |_) | | | (_) | (_| | | |  __/\__ \__ \
 |_____|_| |_| | .__/|_|  \___/ \__, |_|  \___||___/___/
               | |               __/ |                  
               |_|              |___/                   
```
                                                                       
Table of Contents
=================

* [Design spike 3 for <a href="https://github\.com/ivogeorg/jitt">jitt</a>](#design-spike-3-for-jitt)
* [User stories](#user-stories)
  * [1\. Instructor stories](#1-instructor-stories)
    * [1\.1\. Question banks](#11-question-banks)
    * [1\.2\. Questions between banks and courses](#12-questions-between-banks-and-courses)
      * [1\.2\.1\. Two divergent ways to design this:](#121-two-divergent-ways-to-design-this)
        * [1\.2\.1\.1\. Copies](#1211-copies)
        * [1\.2\.1\.2\. Metadata](#1212-metadata)
    * [1\.3\. Cards](#13-cards)
  * [2\. Student stories](#2-student-stories)
* [Design spike protocol](#design-spike-protocol)
* [Views](#views)
* [Workflows](#workflows)
* [Data](#data)

# User stories

## 1. Instructor stories

1. I want to ask my students a small number of _questions_ (see [Question](#question)).  
   1. WarmUp is before class (optional deadline, etc.).  
   2. Lightning is live questions.  
2. What are questions?
   1. Questions have _optional_ meta-data.  
   2. A course label is meta-data of top priority. Course labels will appear on top of all questions as immediate filters.  
   3. Questions can be different type. Core kinds are:
      1. Essay.  
      2. Multiple-choice.  
      3. Multiple-answer.  
   4. Questions appear as richly-formatted markup [cards](#cards).   
   5. Expected clustering is meta-data.  
   6. Questions have versions, that can be shown together.  **TODO:** What are "versions", especially regarding whether two questions are versions or separate quesions?  
      1. Questions have "history".  
      2. (ivogeorg-2020-02-03) I think this matter is actually of great importance. Looking forward to a concept-graph representations of human concepts, the structure of the graph might be used to compose questions of various types and difficulty. So, even thought his might be an advanced feature further down the road (that is, not for MVP), the design of the system should anticipate it.
   7. Questions and their versions live like topic-centered clusters in questions banks. 
   8. Implementation [zoom-in](12-uestions-between-banks-and-courses):
      1. Course questions are partial snapshots of the question bank. (They are stored independently. Any reintegration will be designed in the full bank-course interation, **post-MVP**. Meanwhile, **a note to the instructor** that this is a "local copy" of the questions will be displayed if they initiate edits on the course question.)  
      2. Course questions accumulate a lot of meta-data and versions as they are used in courses, but remain at large, possibly clustered, in the question bank.
3. Interaction with questions:
   1. Before the semester. (Assemble a bank of questions for the course.)  
      1. No particular order (e.g. syllabus is in flux).  
      2. Not homogenous (e.g. I am teaching two different levels).  
   2. Before the semester. (Bank available.)
      1. Questions for the course are selected into a _local snapshot of the originating question bank_.  
   3. During the semester. (The course trajectory changed in this class.)   
      1. Syllabus got rearranged (e.g. drilled down into a topic too early and that requires the next topics to appear in a slightly different order).  
      2. Change the wording.  
      3. Delete a question.  
      4. Move a questions backward or forward.  
      5. Change the expected clustering.  
2. For WarmUp, I want to be able to quickly review the answers (see [Answers](#answers)) for a question, and:
   1. The view can be in two modes, squished and expanded. In squished, I can see as many answers as possible. In expanded, I can interact with the answers.  
   2. Interactions are:
      1. Adjust the default grade.  
      2. Send private feedback to a student off a template. 
      3. _Optionally, enter feedback for each answer cluster for each question and have the system broadcast stitched up responses to each student._  
      4. Manual clustering with on-the-fly category creation.  
      5. Flag for inclusion in a [digest](#digest). 
3. Digest.
   1. Examples:      
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
      4. This is just like adding new folders in Outlook (which is overly clunky), O'Reilly's lists, or SparkFun's lists.  *[Applying labels in Gmail might be a good example. It is very fast, and has a lot of the features you describe.]*
      5. Automtic clustering can be presented in the same UX/UI, just populating the cluster dimensions and categories.
   3. Sort student responses in a few ways. For example:
      1. By timestamp.
      2. Alphabetical by last name.
      3. Randomized. 
      4. Instructor requests "Random 15 responses at the top, from those I have seen the least." The system tallies which students are displayed at the top. The next time, those with the lowest tally are shown at the topc.
4. I want to be able to load up questions for the class periods for a whole semester, and have the flexibility to adjust to the way the specific class is moving through the material. Easily move a question from one WarmUp to another, change the order of WarmUps. Here, "WarmUp" is funcitonally synonymous to class period or "Meeting" (see [Meeting](#meeting)). [WarmUP should be distinct from "Meeting" some isntructors give one WarmUp each week, covering 2 class meetings, for example.]  **I think this is addressed earlier. Remove?**
5. I want to be able to import questions:
   1. From csv, spreadsheet, etc. and then arrange them.  
   2. From a previous edition of the same class.  
   3. From another instructor that uses the system.
6. I want to be able to export questions to csv or spreadsheet.  
7. I want to be able to edit the text of a question, both while it is unassigned (that is, in a [question bank](#11-question-banks)) and assigned (so the unassigned and assigned can differ).  
8. I want the option to have the computer assign grades automatically (auto-grade) to student responses.  
   1. Select mode:
      1. Auto-grade for participation. For example, on a 0-2 scale, default to 1 for having provided an answer and 0 for no answer. 
      2. Auto-grade for correctness, for questions which have correct answers (e.g. multiple-choice, single-answer or multiple-answer, questions).
      3. No auto-grade (meaning, manual grading only). 
   2. Customize point value per question. That is, the scale is definable.   
   3. Manually adjust the grade for a student response at the same time (same view) as reviewing responses. Simple +/- buttons to increase/decrease grades.  **Addressed earlier? Remove?**
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
10. During class meeting, I want to show some curated information from the warmups:  **This is Digest. Discussed earlier. Remove?**
    1. Question text and images. These suppot are points of engagement. For example:
       1. Remind students what the question was about.   
       2. Show how many students didn't do it.  
    2. Aggregate responses categorized by the instructor when reading a sample of student responses. (see slide examples above)
    3. Anonymous responses from some students, especially the "useful wrong answers".  
11. Ideally the student view of questions is LMS embeddable. If the system could connect to the LMS via an LTI so that there is no additional login required, that would be even better.  

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
4. Querstion banks are closely related to other forms of knowledge organization.  
   1. Concept graphs.  
   2. Concept inventories.  
   3. Curricula recommendations (e.g. [ce2016](http://www.acm.org/binaries/content/assets/education/ce2016-final-report.pdf)).  
   4. Conceptual change models.  
   
   
### 1.2. Questions between banks and courses

#### 1.2.1. Two divergent ways to design this:

1. (copies) Course questions are partial snapshots of the question bank. (They are stored independently. Any reintegration will be designed in the full bank-course interation, **post-MVP**. Meanwhile, **a note to the instructor** that this is a "local copy" of the questions will be displayed if they initiate edits on the course question.)  
2. (metadata) Course questions accumulate a lot of meta-data and versions as they are used in courses, but remain at large, possibly clustered, in the question bank.

##### 1.2.1.1. Copies

TBD

##### 1.2.1.2. Metadata

Metadata is "tags", "labels", ...

Question bank has (possibly loose) metadata categories:
- Knowledge graph (e.g. hierarchy of concepts, clustering of question versions)  
- Course assignment  (e.g. this course, that course)  
- Presentation (e.g. question type, card elements (e.g. images, diagrams, drawings, etc.))   



Question bank 
Topic cluster
                 Question version
                                  ==> Course 1000 
                                                  ==> Semester
                                                               ==> Assignment (e.g. WarmUp, Lightnihg)
                                  ==> Course 2000 
                                                  ==> Semester
                                                               ==> Assignment (e.g. WarmUp, Lightnihg)
                                  ==> Meta-data (association, type, level, card elements, ...)
                 
      
                 
Two questions:
1. How does the instructor tag.  (Potentially a lot of work.)    
2. What does the instructor see at the top (i.e. question bank) level.  (Potentially bewildering.)  
   
### 1.3. Cards

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

# Design spike protocol

1. Review [stories](#user-stories).  
2. Answer [open questions](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#open-questions).  
3. Integrate notes on [digests](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#7-digest).  
4. Update [stories](#user-stories).  
5. Integrate notes on [views](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#views).  
6. Design all [views](#views) based on [stories](#user-stories).  
7. Integrate notes on [workflows](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#workflows). Workflows are a graph of [views](#views).  
8. Design all [workflows](#workflows) on the view graph.  
9. Design core [data](#data) using:
   1. GUID.  
   2. Description of relation to other _core data_.
      1. Associations can be many-many, many-one, one-to-many, and one-to-one.  
      2. Mappings are usually one-to-one or one-to-many (dictionary). In the one-to-one case they are _equivalences_.  
   3. **Unique**, e.g. email address.
   3. _Optional_, usually for _non-core data_.


# Views

# Workflows

# Data

