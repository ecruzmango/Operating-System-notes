# Chapter 7: Scheduling introduction

* By now low-level mechanisms of running processes (e.g., context switching) should be clear; if they are not, please go back to __CHAPTER 6__
* We still need to understand the high-level `policies` that an OS scheduler employs


>[NOTE!]
    >How should we develop a basic framework for thinking about
    scheduling policies? What are the key assumptions? What metrics are important? What basic approaches have been used in the earliest of computer systems?


## 7.1 Workload Assumptions

* Workload - the processes running on the system (understanding the workload is critical to building policies)

We will make simplifying (and unrealistic) assumptions about the processes, sometimes called jobs, that are running in the system:
1. Each job runs for the same amount of time
2. All jobs arrive at the same time 
3. All jobs run to the end
4. NO I/O
5. Runtime is known

## 7.2 Scheduling Metrics

* we need a way to compare scheduling polices 
* we will keep it simple with a single metric known as `turnaround time`
* this is a performance metric and our primary focus for now

__Turn_around = T_completion - T_arrival__

### 7.3 First In, First Out (FIFO) or FIrst Come, First Served (FCS)

* most basic scheduling algo. 
__EX1__: Assume jobs A,B,C each take 10 seconds and arrive simultaneously in the give order (T_arrival = 0). What is the avg turnaround time?

| A | B | C|
|---|---|---|
|10|20|30|

(10+20+30)/3 = 20 (Avg turn around time)

__EX2__: Now let us drop the assumption that jobs have the same running time. What kind of workload would make this perform poorly 


|INCLUDE GRAPH|
(100+110+120)/3 = 110 (painful 110 seconds)

* this is known as the `convoy effect`

__ANALOGY__:
This scheduling scenario might
remind you of a single line at a grocery store and what you feel like when
you see the person in front of you with three carts full of provisions and
a checkbook out; it’s going to be a while. So what should we do? How can we develop a better algorithm to
deal with our new reality of jobs that run for different amounts of time?
Think about it first; then read on. [pg.4 Aparci]

## 7.4 Shortest Job FIrst (SJF)

The solution to this is known as the `shortest job first`. It runs shortest job first, then the next shortest and so on

__INCLUDE GRAPH__

* Now lets run the algorithm again and find out the difference
* as you can see,  (10+20+120)/3 = 50, this improves the time by more than half the amount of time

we arrive upon a good approach to scheduling, but our assumptions are still fairly unrealistic. Let's remove the assumption that jobs arrive at the same time. __What can go wrong?__

## 7.6 A New Metric: Response Time (STCF)

