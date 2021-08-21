# Design spike 4 for [jitt](https://github.com/ivogeorg/jitt)

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

* [Design spike 4 for <a href="https://github\.com/ivogeorg/jitt">jitt</a>](#design-spike-4-for-jitt)
* [User stories](#user-stories)
  * [1\. Instructor stories](#1-instructor-stories)
    * [1\.1\. Question lists](#11-question-lists)
    * [1\.2\. Metadata](#12-metadata)
    * [1\.3\. Cards](#13-cards)
  * [2\. Student stories](#2-student-stories)
* [Design spike protocol](#design-spike-protocol)
* [Views](#views)
  * [Hierarchical drill\-down](#hierarchical-drill-down)
  * [General notes](#general-notes)
  * [View list](#view-list)
    * [Instructor](#instructor)
    * [Student](#student)
  * [View details](#view-details)
      * [Instructor\-Account](#instructor-account)
      * [Instructor\-Question list](#instructor-question-list)
      * [Instructor\-Question design](#instructor-question-design)
      * [Instructor\-All courses](#instructor-all-courses)
      * [Instructor\-Course details](#instructor-course-details)
      * [Instructor\-All assignments for a course](#instructor-all-assignments-for-a-course)
      * [Instructor\-Assignment design](#instructor-assignment-design)
      * [Instructor\-Response review and digest creation](#instructor-response-review-and-digest-creation)
      * [Instructor\-Digest presentation](#instructor-digest-presentation)
      * [Instructor\-Grades for all assignments in a course](#instructor-grades-for-all-assignments-in-a-course)
      * [Instructor\-Student roster for a course](#instructor-student-roster-for-a-course)
      * [Student\-Account](#student-account)
      * [Student\-All courses](#student-all-courses)
      * [Student\-Course details](#student-course-details)
      * [Student\-Course assignments with grades](#student-course-assignments-with-grades)
      * [Student\-Assignment questions with grades](#student-assignment-questions-with-grades)
      * [Student\-Assignment digests](#student-assignment-digests)
* [Workflows](#workflows)
* [Data](#data)

# User stories
[[toc](#table-of-contents)]

## 1. Instructor stories
[[toc](#table-of-contents)]

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
3. Interaction with questions:
   1. Before the semester. (Assemble a bank of questions for the course.)  
      1. Import from csv, spreadsheet, etc. and then arrange them.  ==> Bank building.    
      2. Import from a previous edition of the same class.  
      3. Import from another instructor that uses the system. ==> Bank building.  
      4. No particular order (e.g. syllabus is in flux).  
      5. Not homogenous (e.g. I am teaching two different levels). 
   2. Before the semester. (Bank available ==> Assignment building)
      1. Questions for a course from the bank by entering from course creation workflow with **breadcrumbs**, providing automatic course-usage metadata association.    
      2. New question creation should be integrate in the same view. 
         1. How do we associate a new question with question-bank questions?   
      4. Customize point value per question. Instructor can alter scale.
   3. During the semester. (The course trajectory changed in this class.)   
      1. Syllabus got rearranged (e.g. drilled down into a topic too early and that requires the next topics to appear in a slightly different order).  
      2. Change the wording.  
      3. Delete a question.  
      4. Move a questions backward or forward in the order, also in and out of the question bank.  
      5. Change the expected clustering.  
   4. After the semester. (Looking back on your life as an instructor :D)
      1. I want to be able to export questions to csv or spreadsheet.  
   5. Searching (TODO: Jeff & Ivo).  
   6. Sharing & author rights (TODO: Jeff & Ivo). Preliminary notes:  
      1. Jeff: "Thanks for your work on this, Ivo. It is indeed a non-trivial project. I need to make sure I understand how the final product works well enough that I can help maintain and update it.

          Thinking through something after we talked:
          Suppose I create a question that has a unique identifier: Q24. That question has a Title, Stem, Notes, Image, etc. It also has a bunch of tags (Subject, Topic, Class>Assignment, Level, Provenance, Type, etc.).

          One problem I see is along the lines of "Editing rights"
          Can someone else edit the question? Clearly they have to be able to add tags, in order to put it in their own Class>Assignment. But can they add other tags? Can they also remove tags?
          The problem that could arise is that someone else starts adding tags that don't apply to my question, or they remove tags that I want to be there.
          Roughly speaking, this is about "editing rights" in some way. Seems pretty messy. This may cause us to go back to a discussion we had in the past where we debated about how copying questions should work.

          Brainstorming solutions:  
          Could there be a drop-down tag with "Shared/Master/Personal/" as the options?  
          A "Shared" question is fair game. Anyone can edit any part of it.  
          A "Master" can be viewed by anyone, but only the user that created it can edit it. Everyone else can only copy it.  
          A "Personal" question is just yours. Only you can edit it and only you can see it.  
          Perhaps the default is that when you copy a question it gets tagged as Personal. You have to actively choose to make it Shared or Master.".  
      2. Ivo: "Thanks for the comments! I have worked on a large Web project involving right, content provenance, true ownership, and inheritance. My dissertation was partly on this. I will write more later. In short, don't worry about it, we've got this!".  
      3. Ivo: "I like your solutions ideas. Those make for a very clean ecosystem. I would have picked strategies along these lines based on my experience, too: minimal, functional, and efficient. We just need to brainstorm a bit about the long-term potential of the application we are creating and its scaling to an arbitrarily large user base. That might or might not require migrating to a larger server base, but we'll cross that bridge when we get to it.".   
4. Interactions with responses:
   1. Grading.  
      1. Select among the following grading modes:
         1. Auto-grade for participation. For example, on a 0-2 scale, default to 1 for having provided an answer and 0 for no answer. 
         2. Auto-grade for correctness, for questions which have correct answers (e.g. multiple-choice, single-answer or multiple-answer, questions).
         3. No auto-grade (meaning, manual grading only). 
   2. Reviewing.  
      1. The view can be in two modes, squished and expanded. In squished, I can see as many answers as possible. In expanded, I can interact with the answers.  
      2. Interactions are:
         1. Adjust the default grade.  
         2. Manually adjust the grade for a student response at the same time (same view) as reviewing responses. Simple +/- buttons to increase/decrease grades. 
         3. Email individual students (from my default email client) about their response with one click from the basic "view responses" view.  
            1. Emails are based on templates.
            2. A template should be prefilled with available slot data. For example:
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
         4. _Optionally, enter feedback for each answer cluster for each question and have the system broadcast stitched up responses to each student._  
         5. Manual clustering with on-the-fly category creation.  
         6. Flag for inclusion in a [digest](#digest). 
3. Digests.
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
      4. This is just like adding new folders in Gmail and hierarchical drill-down filtering as in this digital-library screenshot:  
         <img src="/assets/digital-library-screenshot-for-fitt-layout.png" width="400" />   
         and also like the new [UI features of Chrome](https://www.google.com/chrome/tips/?utm_source=chrome&utm_medium=material-callout&utm_campaign=tipsq1_21&utm_content=did-you-know).  
      5. Automatic clustering can be presented in the same UX/UI, just populating the cluster dimensions and categories.  
   3. Sort student responses in a few ways. For example:
      1. By timestamp.
      2. Alphabetical by last name.
      3. Randomized. 
      4. Instructor requests "Random 15 responses at the top, from those I have seen the least." The system tallies which students are displayed at the top. The next time, those with the lowest tally are shown at the topic.
4. Ideally the student view of questions is LMS embeddable (like Canvas rich-editor **Embed**).  
5. System connects to the LMS via an [LTI](https://community.canvaslms.com/t5/Canvas-Basics-Guide/What-are-External-Apps-LTI-Tools/ta-p/57#:~:text=LTI%20provides%20a%20framework%20through,authenticity%20of%20the%20data%20sent.) so that there is no additional login required, that would be even better.  

### 1.1. Question lists
[[toc](#table-of-contents)]

_Notes: (Was Question banks) Questions banks work like Google Photos albumns. They are just extra metadata associated with particular questions, which are the main unit of data in the application. A question bank is not an object (programmatically or semantically), just an associative piece of metadata on the questions._

1. Questions can be flagged with subject.  
2. Questions can be flagged with conceptual topics.  
3. Questions can be flagged with question level, most likely to follow adopted and/or conventional scale of academic difficulty. 
   1. For example, for physics, these can be:
      1. Conceptual.  
      2. Algebra-based.   
      3. Calculus-based. 
      4. Upper-division.  
      5. Graduate-level.  
   2. This is a first step toward figuring out how to maintain domain knowledge in a hierarchy of academic difficulty.
   3. In general, these levels should be configurable by the instructor, though concensus and normativity are desired.  
4. Quesitons can be searched by users based on:
   1. Author. 
   2. Subject. 
   3. Topic.  
   4. Question level.  
   5. Date.  
   6. Course name.   
5. Question banks are closely related to other forms of knowledge organization.  
   1. Concept graphs.  
   2. Concept inventories.  
   3. Curricula recommendations (e.g. [ce2016](http://www.acm.org/binaries/content/assets/education/ce2016-final-report.pdf)).  
   4. Conceptual change models.  
   
   
### 1.2. Metadata
[[toc](#table-of-contents)]

General notes:
1. Metadata is "tags", "labels", "GUIDs", ...  
2. Metadata serves as filters and automatically clusters questions:
   1. Some or all metadata will be clickable for filtering and unclickable for removing the filter.   
4. Questions banks are just extra metadata:  
   1. As in Google Photos, there is no copying, just assiation through metadata and filtering on the fly.  
   2. Also, any question that is edited propagates these edits to all question banks that contain it. There is only one version of the question.  

Metadata categories, possibly loosely overlapping:  
1. Knowledge graph (e.g. hierarchy of concepts, clustering of question versions, question sets that are asked together)  
   1. Folks: Concepts, Conceptual association, ...
2. Course usage (e.g. course level, course-semester-assignment, class-n-answers)  
   1. Folks: Course associations, ...
3. Presentation (e.g. question type, card elements (e.g. images, diagrams, drawings, etc.))   
   1. Folks: Design, ...
4. Provenance (e.g. author, etc.)  
   1. Folks: Origins, History, ...  
5. Usage stats (e.g. how many times, digests)  
   1. Folks: History, Answer stats, ...
                 
   
### 1.3. Cards
[[toc](#table-of-contents)]

_Notes: Cards are semi-independent embeddable interactive units of rich multimedia content stored in JSON and MongoDB._

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
[[toc](#table-of-contents)]

1. I want to have an easy interface to answer _warmup questions_ on any device.  
2. I want to have alerts about the pre-class deadlines for _warmup questions_.  
3. I want to be able to edit my answers before the corresponding deadline.  
4. I want to have the questions sent to me over email and apps that I use (e.g. Teams, FB Messenger).  
5. I want to be able to review questions, my responses and the grades I got, after the deadline.  

# Design spike protocol
[[toc](#table-of-contents)]

1. ~Review [stories](#user-stories).~  
2. ~Answer [open questions](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#open-questions).~  
3. ~Integrate notes on [digests](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#7-digest).~  
4. ~Update [stories](#user-stories).~  
5. ~Integrate notes on [views](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#views).~  
6. Design all [views](#views) based on [stories](#user-stories).  
7. Integrate notes on [workflows](https://github.com/ivogeorg/jitt/blob/main/design-spike-2.md#workflows). Workflows are a graph of [views](#views).  
8. Design all [workflows](#workflows) on the view graph.  
9. Design core [data](#data) using:
   1. GUID.  
   2. Description of relation to other _core data_.
      1. Associations can be many-to-many, many-to-one, one-to-many, and one-to-one.  
      2. Mappings are usually one-to-one or one-to-many (dictionary). In the one-to-one case they are _equivalences_.  
   3. **Unique**, e.g. email address.
   3. _Optional_, usually for _non-core data_.


# Views
[[toc](#table-of-contents)]

_Notes: Views are formatted combinations of information, promptings, and editing elements. A view may contain [cards](#12-cards)._

## Hierarchical drill-down
[[toc](#table-of-contents)]

<img src="/assets/digital-library-screenshot-for-fitt-layout.png" width="400" />

## General notes
[[toc](#table-of-contents)]

1. In the question list, questions are by default listed compactly, and they can be expanded one-by-one in place by a click.  
2. In the expanded question, some information will be one-click down for an extra expansion (e.g. historical usage).  
3. _Questions for a course from the bank by entering from course creation workflow with **breadcrumbs**, providing automatic course-usage metadata association._ (TODO: ???)   

## View list
[[toc](#table-of-contents)]

### Instructor
[[toc](#table-of-contents)]

1. [Account](#instructor-account). **[DONE]**  
2. [Question list](#instructor-question-list). **[TODO]**   
3. [Question design: all details for a question](#instructor-question-design). **[DONE]**  
4. [All courses](#instructor-all-courses). **[TODO]**  
5. [Course details](#instructor-course-details). **[TODO]**  
6. [All assignments for a course](#instructor-all-assignments-for-a-course). **[DONE]**
7. [Assignment design]((#instructor-assignment-design)). **[DONE]** 
8. [Response review and digest creation](#instructor-response-review-and-digest-creation). **[DONE]**      
9. [Digest presentation](#instructor-digest-presentation). **[TODO]**  
10. [Grades for all assignments in a course](#instructor-grades-for-all-assignments-in-a-course). **[TODO]**  
11. [Student roster for a course](#instructor-student-roster-for-a-course). **[TODO]**  

### Student
[[toc](#table-of-contents)]

1. [Account](#student-account). **[TODO]**    
2. [All courses](#student-all-courses). **[TODO]**    
3. [Course details](#student-course-details). **[TODO]**    
4. [Course assignments with grades per asst](#student-course-assignments-with-grades). **[TODO]**    
5. [Assignment questions with grades per q](#student-assignment-questions-with-grades). **[TODO]**    
6. [Assignment digests](#student-assignment-digests). **[TODO]**    


## View details
[[toc](#table-of-contents)]  
[[list](#instructor)]  

#### Instructor-Account
[[list](#instructor)]

_This is a view, so graphical elements._

1. Update password
2. Change email (optional, not MVP)
3. Time zone
4. Toggle automatic daylight savings adjustment


#### Instructor-Question list
[[list](#instructor)]

_This is a view, so graphical elements._

1. At the top level, there are all the questions.
   1. Only 1-2 pages at a time are loaded at the same time (a la maps, photos, etc. etc.).
   2. It is scrollable, with dynamic loading of more pages.
   3. It also shows all filters at the top.
   4. Filters are applied as _intersection_ (AND), **not** _union_ (OR).
2. By default, the list view shows a compact list of questions with a default set of data and metadata displayed with each:
   1. Title.
   2. Subject.
   3. Topic.
   4. Type
   5. Provenance
2. Each question can be individually expanded, in place, into a box, containing:
   1. All data and metadata except choices.

TODO (Ivo & Jeff) What are the mechanics of working with a list of questions?:  
1. Do we filter and then add to a course? In general, how do we add a new tag to a filtered selection of questions? This means applying a new label to a selection of questions. ==> **_Yes, we can add a new tag to a filtered (multiple metadata in the form of clickable tags) and selected (binary checkbox) set of questions. On left of each question row there is a selection box. On the right of each question row there is an expand toggle. Upon a single selection (i.e. at least one question row), the "More tags" button appears. When clicked, it opens a (sophisticated) overlay view with all possible ways to apply tags. Those could be (i) a selection from existing unapplied binary tags, (ii) a selection of existing category and value from the category, and (iii) those two but with the createion of a new tag or a new category (or adding a new value to an existing category). A binary tag is easy - just a text box with "Add" button._**  (TODO (Ivo & Jeff): How does that interact with single-question design?)     
2. How does filtering work?  
   1. The 3 main set operations are: union (OR), intersection (AND), and negation (NOT). In the extreme, we can apply arbitrarily complex set-alebraic expressions on a set of questions with multiple filtering criteria. Nobody really does that on the Web, because the complexity quickly moves beyond human cognitive capabilities. Even Google tried and then hid this under "Advanced options", instead focusing on making search results most relevant (heuristics, personalization, etc.). This might be aleviated by a very clever UI/UX though even that should be grounded in currently expected graphical dynamics. ==> **_Default to intersection and leave any advanced set selections to later versions of the app._**   
   2. If we default to intersection (AND), we can use _breadcrumbs_ showing at the top of the list, in the order of their applicaion (e.g. Physics (subject) | Gravity (topic) | PHY 1000 (class) | Essay (question type) ). ==> **_Yes, default to intersection. No metadata in the compact or expanded question-list view are active (clickable or selectable). All selectable/clickable tags, whether binary (equivalent to a checkbox) or discrete-set (equivalent to a drop-down), are in a bar on the left side, possibly in hierarchies, and they might (i) either filter on each selection/click, or (ii) with an "APPLY" button after multiple selection have been made._**  
   3. In the expanded label box for a question in a question list, how do we show labels coming from discrete label sets/categories (e.g. question type is {"multiple-choice", "multiple-answer", "essay"}, jitt type is {"warmup", "lightning"}, etc.). Per the stories, we want to be able to create such categories on the fly during review!    
   4. In the compact view, what labels are shown? Is this fixed or does it depend on some dynamic features (e.g. order of creation, explicit ordering, etc.)?    
   5. **(MAJOR DESIGN ISSUE)** This should probably be co-designed with Question design, so as to sensibly/intuitively divide the functionality between List and Single Question and not pile everything in one or the other or have an inconsistent sets of functionality. This may very well require involving the Workflows, and iterate a few times between Views and Workflows.  

A hand sketch as visual context to the questions and design decisions above:  

<img src="/assets/sketch-q-list-view.jpg" width="400" />  

Notes:  
1. Bottommost sketch decided on.  
2. TODO (Ivo): For next design spike, organize descriptions of design decisions with sketches/wireframes/etc.  

Actions:
1. "Add to Assignment" button.
2. Duplicate question. Meaning begin creating a new question, but with almost all the details of the duplicated question already present.
3. TODO (Ivo & Jeff). (See previous TODO for various issues to resolve.)  


#### Instructor-Question design
[[list](#instructor)]

_This is a view, so graphical elements._

1. Question metadata (free-floating fields).
   1. Title. (Note: The implicit hierarchy here is Subject-->Topic-->Title, like Physics-->Gravity-->Satalellite crashing potential energy.)  
   2. Subject.
   3. Topic.
   4. Type (multiple-choice, multiple-answer, essay).
   5. Course level (Conceptual, Algebra-based, Calculus-based, Upper-division, Graduate-level).
   6. Provenance.
   7. Generic tag(s).
   8. Rest of the metadata.
2. Card.
   1. Type-specific template (incl. title, stem (text), choices, etc.).
   2. Question multimedia content and narrative. 
   3. Choices multimedia content and narratives.

#### Instructor-All courses  
[[list](#instructor)]

#### Instructor-Course details   
[[list](#instructor)]

#### Instructor-All assignments for a course
[[list](#instructor)]

_This is a view, so graphical elements._

1. Course metadata.

2. Ordered assignments. For each:
   1. Number.
   2. Topic.
   3. Type.
   4. Dates.

3. Actions.
   1. Create/add new assignment.
   2. Duplicate assignment.
   3. Delete/remove assignment.
   4. Reorder assignments.


#### Instructor-Assignment design 
[[list](#instructor)]

_This is a view, so graphical elements._

1. Assignment metadata & content.
   1. Number (in course).
   2. Subject.
   3. Topic.
   4. Type (warmup, lightning).
   5. Dates (available, due).
   6. Instructions.

2. Ordered questions. For each:
   1. Title.
   2. Card.
   3. Grading mode.
   4. Grading scale for mode.

3. Actions.
   1. Create/add new question.
   2. Duplicate question.
   3. Delete/remove question.
   4. Reorder questions.


#### Instructor-Response review and digest creation      
[[list](#instructor)]

_This is a view, so graphical elements._

1. Ordered questions for assignment. For each:
   1. Card.
   2. Responses for question type.
2. Responses. For each:
   1. Types:
      1. For choices:
         1. Picked.
         2. Correct/incorrect, if applicable.
         3. Adjustable auto-grade.
         4. Summary stats.
      2. Essays:
         1. Narrative.
         2. Adjustable auto-grade.
         3. Include in digest (single (partial) response).
         4. Cluster(s), shown immediately at creation time.
         3. Highlighted for careful review.
         4. Student digest tally. _(To make sure all students get their responses discussed at least once.)_
         5. Student email tally. 
         6. Highlight tally.
         2. Include in digest.

3. Digest fields.
   1. Question types:
      1. Choices:
         1. Summary stats.
      2. Essays:
         1. Sample responses, possibly editable.
         2. Categorization/clustering.

4. Response view actions.
   1. Pick N number of random responses for careful reading from the lowest highlight tally.
   2. Order responses.
   3. Adjuct auto-grade or assign grade.
   4. Show/hide student identification data.

5. Digest actions. (category ~ cluster)
   1. Create new category.
   2. Add response to existing or new category, updating digest tally.
   3. Edit responses.
   4. Comments.

6. Student interaction actions.
   1. Email individual student from template, updating email tally.
   2. Actions for using automatic aggregate clustering results (a la [sense education](https://www.sense.education/)). (non-MVP)


#### Instructor-Digest presentation  
[[list](#instructor)]

#### Instructor-Grades for all assignments in a course  
[[list](#instructor)]

#### Instructor-Student roster for a course  
[[list](#instructor)]


#### Student-Account  
[[list](#student)]

#### Student-All courses 
[[list](#student)]

#### Student-Course details   
[[list](#student)]

#### Student-Course assignments with grades
[[list](#student)]

#### Student-Assignment questions with grades  
[[list](#student)]



#### Student-Assignment digests  
[[list](#student)]


# Workflows
[[toc](#table-of-contents)]

<img src="/assets/digital-library-screenshot-for-fitt-layout.png" width="400" />  

# Data
[[toc](#table-of-contents)]

