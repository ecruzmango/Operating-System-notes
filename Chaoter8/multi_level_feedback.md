# Chapter 8: Multi-Level Feedback Queue

* We are taking away the final assumption of knowing the runtime of each job since this is unrealistic 
* there certain branches where it is very difficult to know when the job might be completed 

### Multi-Level Feedback Queue Goals

* For `turnaround time`, we know SJF (from chapter 7) is optimal, but remember, in reality, we don't know how long jobs will run
* We want the system to feel responsive to interactive users, so we need to optimize response time as well 

>[!NOTE]
> If we reduce response time, then turnaround time increases

>[!NOTE]
> How can we design a scheduler that both minimizes response time for
interactive jobs while also minimizing turnaround time without a priori knowledge of job length? [Arpaci Pg.1]

## 8.1 MLFQ: Basic Rules

* We're going to have multiple queues.
* Each is assigned a different priority level.
* When we need to make a scheduling decision, we pick a job with the
highest priority.
* For jobs in the same priority queue, use Round Robin.



## 8.2 Changing Priority

__RULES:__
RULE 1: If Priority(A) > Priority(B), A runs
RULE 2: if Priority(A) = Priority(B), A & B run in RR

 The key to MLFQ scheduling lies in how we select priorities for jobs.
• For example, a job that often yields waiting on keyboard input might
have a high priority because it is obviously interactive.
• MLFQ will learn based on history of observed behavior to make
decisions.
o For example, if a job keeps releasing control waiting on keyboard input, MLFQ
might give it a high priority so that it is interactive.
o In contrast, if a job uses the CPU for a long time, reduce its priority.

[INSERT IMAGE]

 With our current rules, only A & B
will run and never give C or D any
CPU time.
• Job priority must change over time!



__ADDITIONAL RULES__:
RULE 3: Start in highest Queue
RULE 4a: if a job uses up an entire time slice while running, it's priority is reduced 
RULE 4b: If a job gives up the CPU before the time slice is up, it stays at the same priority level

__EXAMPLE 1: A Single Long Running Job__
• Let's look at a long running
job in a 3-queue scheduler.
• Starts at the top and quickly
gets knocked down to the
bottom.

__EXAMPLE 2: What about I/O__
Interactive jobs will stay at
highest priority because it is
likely that it will not use the
entire time slice before
blocking on I/O and remain
at the top priority.


### ISSUES: 

__ISSUE #1: Starvation__

* too many interactive jobs will __starve__ long running jobs 
* we will need a way to increase in priority to avoid this!!

[INSERT IMAGE]


__ISSUE #2: Gaming the Scheduler__
* A program could issue I/O or yield after 99% of the time slice to stay in the top priority queue

[Insert IMAGE]

__ISSUE #3: Changing Behavior__
* 


## 8.3 Attempt #2: The Priority Boost

* Let's attempt to avoid starvation by boosting priority of jobs 

__MORE RULES:__
RULE 5: After some time period S, move all the jobs in the system to the topmost queue

* This will guarantee no starvation and low priority jobs that have become interacive will be treated poorly

__EXAMPLE 1:__
The first job was CPU
intensive at first but
became interactive.
• With the priority
boost, we account for
this.
• That solves starvation
and changing
behavior.

[insert image]


## 8.4 Better accounting
Let's try to prevent
gaming of the scheduler.
• Let's change rule 4
• Rule 4: Once a job uses
up its time allotment at a
given level (regardless of
how many times it has
given up the CPU), its
priority is reduced (i.e., it
moves down one
queue).


[Insert the 2 Images] 


## 8.5 Turning MLFQ And Other Issues


## 8.6 MLFQ Summary: 

We have described a scheduling approach known as the Multi-Level
Feedback Queue (MLFQ). Hopefully you can now see why it is called
that: it has multiple levels of queues, and uses feedback to determine the
priority of a given job. History is its guide: pay attention to how jobs
behave over time and treat them accordingly.
The refined set of MLFQ rules, spread throughout the chapter, are reproduced here for your viewing pleasure:
• Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t).
• Rule 2: If Priority(A) = Priority(B), A & B run in round-robin fashion using the time slice (quantum length) of the given queue.
• Rule 3: When a job enters the system, it is placed at the highest
priority (the topmost queue).
• Rule 4: Once a job uses up its time allotment at a given level (regardless of how many times it has given up the CPU), its priority is
reduced (i.e., it moves down one queue).
• Rule 5: After some time period S, move all the jobs in the system
to the topmost queue