# Foresight BI E-learning Report

## Table of Contents
* [Introduction](#Introduction)
* [Data Cleaning and transformation](#Data_Cleaning)
* [Data Visualization](#Data_Visualization)
* [Recommendations](#Recommendations)

## Introduction

The Foresight BI E-Learning Courses dataset was downloaded from Foresight BI [click here](https://docs.google.com/spreadsheets/u/0/d/1M3INoxFT5tzmjaDOS68TG4xfx7x4LERd/htmlview)

Foresight BI & Analytics is a Consulting & Training Firm. They specialize in developing Microsoft Power BI Reporting Solutions and Training for Individuals and Organizations. The Firm has some Self-Paced Courses hosted on a Learning Management System (LMS). The Analytics provided by the LMS application does not provide enough information required by Management.
They can extract some Data from the app that includes:
* Students Enrollments
* Students Quizzes. 

### Business Task
Foresight BI hopes to use the Data to improve Student Engagement, Marketing as well Improve the course curriculum.

## Data_Cleaning

Tools used for data cleaning include:
* Microsoft Excel
* Microsoft Power BI

The E-learning Dataset contained 5 different worksheets:
* Courses
* QuizDim
* Users
* Enrollments
* Quiz Detailed
* Quiz Summary

### The Cleaning Process

Microsoft excel was used to add context to the Quiz names, such that each quiz name had a suffix that denotes the course that contained the quiz, the was done by using the course ID to lookup the course name in the COURSES worksheet and concatenating the Quiz name and the course name while separated by a hyphen and two white spaces at both sides of the hyphen. See below before and after:

BEFORE

![quizdim before cleaning](https://user-images.githubusercontent.com/109347195/231433264-d6ec0fef-7739-4716-b022-a898643ff4cf.JPG)

AFTER

![quizdim after cleaning](https://user-images.githubusercontent.com/109347195/231433331-2f029c93-b28a-40fb-98d5-33f22a2e0345.JPG)

Microsoft Power Query was used to cleaning and do the following data transformations on the different worksheets as outlined below:

* Enrollments: All ID Columns was set to text data type so as to stop the data being arithmetically summarized in the visualization. Blank rows were checked for and none was found. All dates columns - items.completed_at, items.started_at, items.activated_at and items.updated_at, were set to date data type.
* Quiz score summary: Quiz score was set to percentage using a custom formula
  `[#"% Score"] / 100` 
  the column was then set to percentage data format. see below:
  
BEFORE 

![quiz score before](https://user-images.githubusercontent.com/109347195/231442002-71fecbba-6fd8-449c-9421-197cd539ff1e.JPG)

AFTER 

![quiz score after](https://user-images.githubusercontent.com/109347195/231442523-4a003427-954a-4766-bf11-9f0f361787f0.JPG)

* Users: The columns items.firts_name and items.last_name were combined to get the Users' full names, the custom formula used to do this:
  `[items.first_name] & ", " & [items.last_name]`
See pictures of before and after:
BEFORE

![users before](https://user-images.githubusercontent.com/109347195/231443913-79964264-c339-48cc-98ad-263dec009fd9.JPG)

AFTER

![users after](https://user-images.githubusercontent.com/109347195/231443996-7b05e606-9b2c-4846-8c4c-25831fe719ed.JPG)

* Courses: The items.name column was changed to Course name for better context. No duplicate or blank rows were found too.

### DAX functions and Measures

* `Course type = RELATED(Courses[Course Type])` was used to bring in the course type column into the enrollment table.
* `Ave Course Completion Rate = AVERAGE(Enrollments[Course Completion Rate])` measure for Average Course Completion Rate.
* `Average Course duration in minutes = AVERAGE(Courses[Course Duration (hrs)]) * 60` measure for Average course duration in minutes.
* `Total number of courses = DISTINCTCOUNT(Courses[Course ID])` measure for total number of courses.
* `Total Number of Users = DISTINCTCOUNT(Enrollments[User ID])` measure for total of users enrolled.

## Data_Visualization
Data visualization was done in Microsoft Power BI.

Data modelling was done prior to data visualization on Microsost Power BI, using the foreign keys in our fact tables (Enrollments, Quiz Score Summary) and the primary keys in our dimension tables (Users, Courses, QuizDim).

To answer the business question, the following visuals were used to answer the structured sub-questions in order to arrive at and inference and recommendation:

### What are the top KPIs?

![KPIs](https://user-images.githubusercontent.com/109347195/233841628-8ec328c8-a81e-4a6e-8ff5-af4ec54cb4e9.JPG)

Using the measures, it was derived that the E-learning data had a total of 27 courses, 1,236 Unique enrolled users, an average course duration of 21.06 hours and an average course completion rate of 45%.

### Course enrollment trend:

![COURSE ENROLLMENT](https://user-images.githubusercontent.com/109347195/233841644-7e9f8d91-823d-4503-abd4-e8b3ce200044.JPG)

The data showed that there was high course enrollments in January, followed by a drop in February, and then steadied trend between March and July, before another Spike in August, then a steadied drop till December. This theme was not consistent across all the years, in 2021 enrollments peaked in January and October, and was lowest in December, March and April. In 2022, there was only one peak, which was in August. When viewed by year, Enrollments has been on a continuous high from inception in 2020 till October 2022, as shown below:

![course enrollment - YEAR](https://user-images.githubusercontent.com/109347195/233841706-588dd28f-4d0d-48b1-8354-f73ed7171d1d.JPG)

### Enrollment by Course type:

![COURSE TYPE](https://user-images.githubusercontent.com/109347195/233841727-9f81baf0-38ce-444d-a700-7f8d02ca144f.JPG)

Online self paced courses had the highest enrollments - 1,358 users enrolled and the most enrolled course for this course type was Introduction to Power BI. 

### Course type completion rate:

![COURSE COMPLETION](https://user-images.githubusercontent.com/109347195/233841749-0a0ce2fb-6f55-4b30-bb57-7716db8d0c58.JPG)

Online Test and Survey came out top for course completion rate at 96% and 64% respectively, this was in no doubt expected as they needed just a one-time participarion to attain 100% completion rate, however, between the two more comprehensive course types - Online Self paced and onlive live, online live came out top with 46% completion rate while online self paced had 43% completion rate. User behavior, User preference, Course Curriculum model could have played a role in this.

### Course duration vs Course Completion Rate:

![COURSE DURATION VS](https://user-images.githubusercontent.com/109347195/233841764-02243234-5bbb-4d64-bdab-0341f2c9326c.JPG)

I tried to find out if the course duration had an influence on the course completion rate, this was not the case with the comprehensive course types - Online live and Online self-paced, while online live had the higher average course duration it also has the higher average course completion rate. User behavior can be faulted for this, as online live courses are instructor led and paced, while online self paced are user-paced.

### Quiz score

![QUIZ SCORE](https://user-images.githubusercontent.com/109347195/233841785-2540f92c-b29f-4338-b42f-ec565601810f.JPG)

The Check your knowledge quiz in the Intro to Power BI course had the highest number of engagements and average quiz score.

### Other Insights

![insight 1](https://user-images.githubusercontent.com/109347195/231459223-b77ad3ab-278f-4af3-91c0-a5c5a35cfbbd.JPG)

![insight 2](https://user-images.githubusercontent.com/109347195/231459444-0fc14597-6b1a-478d-86ce-51f1c5ac15d3.JPG)

![insight 3](https://user-images.githubusercontent.com/109347195/231459831-f26baa76-9fda-4fa3-8b80-ca69277c6ba8.JPG)

### Tool Tip

![TOOLTIP](https://user-images.githubusercontent.com/109347195/233841802-9a260633-5c94-4702-95f1-3cb5625dcf82.JPG)

A tool tip page that filtered by Course type and Enrollment date (month hierarchy) was created to provide added context on the top course for that category and that month. This can be seen when the mouse is hovered over the data point.

## Recommendations

It is pertinent to note that the data had limitations, which are:
* The 2022 data was not a full year data but ended in October.
* The data did not contains user sentiments like reviews, ratings and responses to further shed more light on the courses uers preferred the most and why.

Here are recommendations deduced from the analysis:

* Marketing should be focused on online self paced course types as courses in this course type had more user enrollments and engagement but lower course completion rate. To improve user experience on this course types, user reviews, feedbacks and ratings at the end of each section of the course should be retrieved in order to understand the low completion rate and improve on the course curriculum.
* To understand the enrollment rate peaks at the first quarter of the year and then August and October, if related to promotion sales at the beginning of the year, such promotions should be replicated at least thrice in the year to increase user enrollments and course sales all through the year.

THANK YOU.
