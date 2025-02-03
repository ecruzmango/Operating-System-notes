# Chapter 9: Proportional Share Scheduling:

### A new metric
* lets focus on turnaround or response time to guarantee each job gets a certain percantage of CPU time
* we can do this withe a proportional-share or fair share scheduler

>[!NOTE]
> How can we design a scheduler to share the CPU in a proportional
manner? What are the key mechanisms for doing so? How effective are
they

## 9.1 Basic Concepts

* __TICKETS__: represent the share of a resource that an entity should recieve 

__EXAMPLE__
if process A has 75 tickets and process B has 25 tickets,
then A should receive 75% of the CPU time and B should receive 25%
of the CPU time.


## 9.2 tickets mechanisms

* __Ticket currency__ allows a user to create their own currency.
There's a global currency and then whatever currency each user creates.\

For example, user A and user B both are given 100 global tickets.
— User A runs two jobs A1 and A2
• A1 and A2 are given 500 "A bucks" each
— User B runs one job B1
• B1 is given 10 "B bucks"
• A1 and A2 each have 50% of the "A bucks" so they each have 50
global tickets.
• B1 has 100% of the "B bucks" so it has 100 global tickets.

| | |  CPU TIme |
|--------------| ------------ | ---------- | 
| User A = 100 | A1 = 500, A2 = 500 "A" tickets | %50 | 
| User B = 100 | B2 = 10 "B" tickets | %50 |



### Ticket Transfer
* `Ticket transfer` allows a process to lend tickets to another process. It is useful in a cooperative setting like client/server running on the same
machine.
* The client sends a request to the server.
* Since it must wait on the server to finish the request, the client can lend
its tickets to the server to give it a higher chance of being scheduled.
*  When done, the server gives the tickets back to the client.



### Ticket Inflation
* `Ticket Inflation`: Allows a process to change the number of tickets it owns (note that this is only useful in a cooperative environment)
* If a process knows it's going to need more CPU Ticket, it can simply boost its tickrts
without coordinating with other processes


## 9.3 Implementation
__Lottery Scheduling__ is simple. It needs the following:
1. a good random number generator
2. a data structure to track processes (a list)
3. a total number of tickets

* Generate a number, N
* Traverse list of processes adding up their ticket values

### HEAD -> JOB:A(Tix: 100) -> JOB:B(Tix:50) -> JOB:C(Tix:250) -> NULL

### Fairness
* If we run two jobs of the same
length, how fair is this
scheduler?
*  Randomness affects short
jobs
*  Fairness = job_finish_1 /
job_finish_2
*  We want fairness to be 1

[INSERT IMAGE]

## 9.6 Stride Scheduling

 Why use randomness at all if sometimes it isn't fair?
• Stride Scheduling is a deterministic fair-share scheduler.
• Assign each job a stride which is the inverse in proportion to the
number of tickets it has.
— Stride = (some large number) / tickets
• Each process has a running pass value starting at 0.
• When a process is scheduled, increment its pass by stride.
• Always schedule the process with the lowest pass breaking ties
arbitrarily.


[Insert Image]

Each process ran exactly in proportion to its tickets so why use lottery
scheduling at all?
— No global state
— If a new process comes along, what should its pass be?

## 9.7 The Linux Completely Fair scheduler (CFS)

* Highly efficient and scalable fair-share scheduler.
• Aims to spend very little time making decisions.
• This is important to not waste resources.
— Google datacenter even after aggressive optimization used 5% of the CPU time
scheduling!
• Reducing overhead is a key goal in modern schedulers.
• Goal is to divide the CPU evenly among all competing processes.
• It does so with a virtual runtime (vruntime)



