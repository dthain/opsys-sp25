# CSE 30341-01 Operating Systems Principles Spring 2025

- Prof. Douglas Thain (dthain@nd.edu), University of Notre Dame

## Overview

An operating system (OS) is a critical layer of software that manages hardware resources and that provides user programs with a simple and consistent interface to the machine. In this course, we will examine the services and abstractions commonly provided by operating systems, and we will study the underlying mechanisms used to implement them. Topics include processes and threads, synchronization, cpu scheduling, deadlocks, memory management, segmentation and paging systems, storage and file systems, distributed systems, and virtualization.

A good understanding of OS internals offers several benefits, whether you intend to work in computer hardware or computer software. First, by understanding how an OS works, you will be able to write better software applications by making efficient use of OS abstractions such as filesystems and memory allocators. Second, the traditional problems that occur in operating systems (like scheduling, synchronization, and resource management) also occur in all sorts of other systems, ranging from small embedded computers to large scale cloud computing systems. The techniques that you learn here will have applications in every kind of computer system.

The concepts presented in class will be explored through a series of six intensive programming assignments. The assignments will make use of the C programming language, which the universal language for implementing and accessing operating systems at the lowest level. The projects will give you lots of practice in manipulating pointers, managing memory, invoking system services, and dealing with error conditions. Although we will offer some technical guidance on these matters, students should expect to spend significant time debugging, consulting reference materials, and revising the projects until they work properly.

The goals for each student in this course are:

- To understand the abstractions and services provided by an operating system.
- To understand the mechanisms and algorithms used to implement these services
- To get practical experience using and implementing operating system services.

The course materials will test each student's achievement of these goals at several levels. For each topic in the course, students must be able to:

- Describe traditional operating system structures and algorithms.
- Demonstrate in detail how they apply to various programs and data.
- Evaluate the strengths and weaknesses of related structures and algorithms.
- Propose and evaluate a variety of improvements upon traditional methods.
- Implement basic methods in a working computing system.

## Course Communications

The course web page contains the schedule, assignments, and other critical information:
- [http://dthain.github.io/opsys-sp25](http://dthain.github.io/opsys-sp25)

Canvas will be used for the limited purposes of handing in written homeworks and returning grades:
- [https://canvas.nd.edu/courses/109948](https://canvas.nd.edu/courses/109948)

## Textbooks

- Required: Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau, **Operating Systems in Three Easy Pieces**, Arpaci-Dusseau Books, 2019. [http://ostep.org](http://ostep.org)
- Suggested Reference: [Dive Info Systems](https://diveintosystems.org/book), Suzanne Matthews, Tia Newhall, and Kevin Webb, 2020.

The course web page will also have a selection of short topical readings linked to certain class days.
You are also responsible for reading and absorbing that material.

## Reading Assignments

The textbook provides the foundation of the course, and it will be important for you to absorb all of it thoroughly.
To encourage regular reading and preparation for class, you should read the assigned chapters and take notes each weekend.
Summary notes on the assigned readings will be due in Canvas each **Monday** by 11:59PM.  (Except for the first week,
which are due **Friday** at 11:59PM.)

The most effective way to read and retain information is to take notes the old-fashioned way, by hand with pen and paper.
If you are out of the habit of writing by hand, now is a great time to start, so as to build up your muscles and practice
for the exams.  Once written out, just snap a few photos and upload to Canvas.

Your notes can be organized in whatever way is appropriate to that chapter and is useful for you. Good things to include may be an outline of the chapter, definition of key terms, a sketch of the systems or data structures being discussed, that sort of thing.  The **last** item in your notes should be **one thoughtful question (circled)** about the reading: something that you would like to discuss or learn more about.  We will look over the
questions and address several highlighted ones each week.

Grading on notes will be very simple: either one, one-half, or zero points. There will be fourteen opportunities to earn a total of twelve points, so you may miss two without penalty.  The act of taking notes is entirely for your benefit, so we are just looking for a constructive effort on your part.

## Homeworks and Projects

Each week will have either a written homework or a programming project due on ** Friday at 5:00PM**.

Homeworks will typically involve short answers, sketches, and hand computations that follow from
the class discussion of that week.  Again, these should be done with pen and paper, then take a picture and then upload to Canvas.

Programming projects will be more challenging.  Each will explore a specific topic in operating systems,
and will generally require that you spend some time researching system calls, troubleshooting problems,
taking measurements, and refining your work in order to reach a quality results.  Don't expect to complete
these projects in one sitting!  It will work best if you can start the project well before the deadline.
and then take a break to reflect, ask questions, or come to office hours.  The projects increase in
scope and difficulty through the semester -- expect that the final two projects will take about three
weeks of effort each.  These will be submitted to a dropbox directory on the student machines.

## Attendance

To succeed in this class, you should attend and actively participate in all the scheduled lectures,  However, attendance is not taken.
You will get the most out of class if you have done the required readings in advance and actively take notes during class.
I typically work by diagramming key structures of the OS on the chalkboard, and then work through examples and take questions.

Please refrain from using your laptops for non-class activities, since this [tends to distract others](https://www.insidehighered.com/news/2018/07/27/class-cellphone-and-laptop-use-lowers-exam-scores-new-study-shows) as well as yourself.  Let's use this time to work together instead.

Class recordings will be available as a **back-up option** if you are out sick
or travelling on University business.  But don't make that your primary plan:
you will get much more out of actively participating in person.

## Grading

The course grade will be based on six programming projects, four homeworks, a midterm, and a final exam. For each assignment, a numeric grade will be assigned. Grades will be made available in Canvas.  The grade breakdown is projects (45%), homeworks (10%), reading notes (10%), midterm (15%) and final exam (20%). At the end of the semester, number grades will be converted to letter grades: 90/80/70 are the usual cutoffs for A/B/C grades, respectively. The instructor will exercise discretion on borderline grades, or to account for a trend of increasing/decreasing grades throughout the semester.

If you believe an error has been made in grading an item, please bring it to the attention of the TA who graded it within seven days of receiving it. (The TA who graded it knows the details best, and so can give you the best explanation.) Factual and clerical errors will of course be cheerfully corrected. If you are unsatisfied with the TA's explanation, you may appeal to the instructor. After seven days, graded items are final and will not be revisited.

**Late assignments are not accepted.**  Plan accordingly and start work well before each deadline.  You may submit partially complete code at multiple points prior to the deadline, so there is no reason not to turn in **something** for each deadline.  If you happen to fall behind and miss a deadline, then let it go and focus your efforts on the next assignment.

Exceptions will be made only for the circumstances given in section 3.1.3 of the undergraduate academic code, such as major illness,
death in the family, or participation in an authorized university event.  In those cases, the student should confer with the instructor
at the earliest opportunity.

## Academic Code of Honor

As a student at Notre Dame, you are bound by the Academic Code of Honor (http://honorcode.nd.edu), which states:

> As a member of the Notre Dame community, I acknowledge that it is my responsibility to learn and abide by principles of intellectual honesty and academic integrity, and therefore I will not participate in or tolerate academic dishonesty.

The purpose of the homeworks and assignments in this course is for each student to gain the discipline and skills in design, programming, and analysis, so that they will be able to work independently in a professional setting. To that end, all exams, homeworks, and programming assignments are to be completed individually. Each student must write their own code from scratch with their own hands based on their own understanding of the course material.

You may consult with other students and outside resources in order to better understand concepts and techniques, or to debug localized problems. You may not copy code from another student or resource, excepting any starter code provided by the instructor.

**In a similar way, you may not use generative AI assistants (ChatGPT, Copilot, etc) to generate material for any assignments in this class, since this does not serve to develop or demonstrate your own understanding of the material.**

## Some Campus Resources

If you require an accommodation for a disability, please first contact the Sara Bea Center (sarabeadisabilityservices.nd.edu) for a consultation, and we will be happy to work together on a solution. If you encounter a difficult life situation and don't know what to do, the University Counseling Center (ucc.nd.edu) or the Care and Wellness Consultants (care.nd.edu) can help and also connect you with other campus resources.
