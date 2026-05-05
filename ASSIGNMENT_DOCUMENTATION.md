# Assignment 3 - Complete Documentation

**Student Name**: [nourah bader alogaily]  
**Student ID**: [445052170]  
**Date Submitted**: [5 may]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: https://drive.google.com/file/d/1AdjPIPRoCofTOp21ZHmMTszme_TmGLto/view?usp=drive_link

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [30 april, 7:00 pm]
**What I implemented**: 
I found shared resources like counters and the execution log by examining the original scheduler code.
**Challenges encountered**: 
being aware of potential racial situations.
**How I solved it**: 
I examined thread behavior and found important passages.
**Testing approach**: 
To see behavior, run the code without synchronization.
**Time spent**: 
 3 houres
---

### Entry 2 - [ 1 may, 6:00pm]
**What I implemented**: 
To safeguard the execution log and shared counters, I introduced a ReentrantLock.
**Challenges encountered**: 
ensuring the appropriate protection of all common variables.
**How I solved it**: 
All updates were enclosed in lock and unlock blocks.
**Testing approach**: 
To make sure there were no contradictory values, several runs were tested.
**Time spent**: 
2 hours
---

### Entry 3 - [ 1 may, 7:00pm]
**What I implemented**: 
To manage CPU access, I put in place a semaphore.
**Challenges encountered**: 
Understanding where to acquire and release the semaphore.
**How I solved it**: 
Used acquire before execution and release in finally block.

**Testing approach**: 
confirmed that only one process is active at a time.

**Time spent**: 
2 houres
---

### Entry 4 - [2 may,  5:00pm]
**What I implemented**: 
I enhanced logging and completed synchronization.
**Challenges encountered**: 
Ensuring no deadlocks occur.

**How I solved it**: 
Used try-finally blocks.

**Testing approach**: 
Ran multiple simulations.

**Time spent**: 
1 houres
---

### Entry 5 - [2 may, 7:00pm]
**What I implemented**: 
Tested the program and verified final results.

**Challenges encountered**: 
Checking correctness of waiting time calculations.

**How I solved it**: 
Compared expected vs actual output.

**Testing approach**: 
Analyzed output statistics.

**Time spent**: 
1 hour
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:

[The original code contained two race situations.

First, several threads share the contextSwitchCount variable. One update could be missed if two threads increment it simultaneously, producing inaccurate results.


Second, there is a shared executionLog list. Inconsistent logs or even runtime problems could result from many threads adding entries at the same time.

Because threads run independently and changes may overlap in the absence of synchronization, concurrent access is challenging.

Inaccurate counters, missing log entries, or damaged data could arise from this.]

---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:

[To ensure that only one thread can access shared resources at a time, ReentrantLock is utilized for mutual exclusion.

The amount of threads that can access a resource at once is managed by a semaphore.

I utilized ReentrantLock in my code to safeguard the execution log and shared counters.

To simulate CPU access and make sure that only one process runs at a time, I utilized a semaphore with a single permit.

Both controlled execution and data safety are guaranteed by this combination.]

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

[When two or more threads wait endlessly for resources owned by one another, this is known as deadlock.

Using try-finally blocks to guarantee that locks are always released is one preventative strategy.

Avoiding nested locks is another tactic.

To avoid deadlocks in my code, I used try-finally to release both the lock and the semaphore.]

---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

[I used one lock (coarse-grained locking) for all shared counters.

This simplifies the design and reduces complexity.

The advantage is easier implementation and lower risk of deadlocks.

The disadvantage is reduced concurrency because only one thread can access any counter at a time.

Fine-grained locking allows better concurrency but is more complex.

Since the counters are independent, fine-grained locking would provide better performance, but for this assignment, simplicity was preferred.]

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 

**Why they need protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java
**Which variables**: contextSwitchCount, completedProcessCount, totalWaitingTime

**Why they need protection**: 
They are shared between threads and can be updated simultaneously.

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
```java
sharedLock.lock();
try {
    contextSwitchCount++;
} finally {
    sharedLock.unlock();
}
```

**Justification**: 

---

## Critical Section 2

```md
**What resource**: executionLog

**Why it needs protection**: 
Multiple threads write to it simultaneously.

**Synchronization mechanism used**: ReentrantLock

**Code snippet**:
```java
sharedLock.lock();
try {
    executionLog.add(message);
} finally {
    sharedLock.unlock();
}
---

### Critical Section #2: Execution Log

**What resource**: 

**Why it needs protection**: 

**Synchronization mechanism used**: 

**Code snippet**:
```java

---

## Critical Section 3

```md
**Purpose of semaphore**: 
Control CPU access

**Number of permits and why**: 
1 permit to simulate single CPU

**Where implemented**: 
Inside run() method

**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
try {
    // execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Justification**: 

---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 

**Number of permits and why**: 

**Where implemented**: 

**Code snippet**:
```java

---

## Critical Section 3

```md
**Purpose of semaphore**: 
Control CPU access

**Number of permits and why**: 
1 permit to simulate single CPU

**Where implemented**: 
Inside run() method

**Code snippet**:
```java
SharedResources.cpuSemaphore.acquire();
try {
    // execution
} finally {
    SharedResources.cpuSemaphore.release();
}
```

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
ran the program 5 times
```

**Results**: 
14 processes completed
  Correct waiting times
**Why synchronization is necessary**: 
Synchronization ensures stable results.
**Conclusion**: 

---

### Test 2: Exception Testing
**What I tested**: Checking for ConcurrentModificationException

**Testing procedure**: 

**Results**: 

**What this proves**: 
No ConcurrentModificationException occurred.

This proves executionLog is properly synchronized.
---

### Test 3: Correctness Verification
**What I tested**: Verifying correct final values (total burst time, context switches, etc.)

**Expected values**: 

**Actual values**: 

**Analysis**: 
Expected: 14 processes completed  
Actual: 14 processes completed  

Analysis: Results match expected behavior.
---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]

**Purpose**: 
Changed time quantum.
**Results**: 
Observed different scheduling behavior.

**What I learned**: 
Learned how quantum affects performance.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

Through this assignment, I learned that synchronization is essential when multiple threads access shared resources. Without synchronization, race conditions can occur, leading to incorrect results and unpredictable behavior. I understood how ReentrantLock provides mutual exclusion and ensures that only one thread can access critical sections at a time. I also learned how Semaphore can control access to limited resources, such as simulating a single CPU. One challenge I faced was identifying all critical sections in the code, especially shared counters and logs. Another challenge was ensuring that locks and semaphores are always released, which I solved using try-finally blocks. This assignment helped me understand the importance of thread safety and proper synchronization design. Overall, I gained practical experience in managing concurrency in Java programs.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: 
Banking systems where multiple users access the same account balance. Synchronization ensures that deposits and withdrawals are processed correctly without data corruption.

**Example 2**: 
Operating systems CPU scheduling, where multiple processes compete for CPU time. Synchronization ensures fair and controlled access to the CPU.
---

### How I would explain synchronization to others:

[I would explain synchronization as a way to control access to shared resources when multiple threads are running. For example, imagine a single bathroom that multiple people want to use. If everyone tries to enter at the same time, it will cause problems. Synchronization works like a lock on the door, allowing only one person inside at a time. Similarly, in programming, synchronization ensures that only one thread can access a shared resource at a time, preventing conflicts and errors.]

---

## Part 6: GitHub Repository Information

**Repository URL**: 
https://github.com/nourah131/OS-Assignment3-Nourah-alogaily
**Number of commits**: 
8
**Commit messages**: 
1. change my uni id number
2. add synchronization imports
3. add shared lock cpu semaphore
4. protact shared resoures with reentrantlock

---

## Summary

**Total time spent on assignment**: 
2 days
**Key takeaways**: 
1.  Synchronization prevents race conditions in multi-threaded programs
2. Locks and semaphores are essential tools for thread safety
3. Proper design avoids deadlocks and ensures correct execution



**Most challenging aspect**: 
Identifying all critical sections and ensuring proper synchronization without causing deadlocks

**What I'm most proud of**: 
Successfully implementing synchronization using both ReentrantLock and Semaphore and achieving correct and stable output
---

**End of Documentation**




