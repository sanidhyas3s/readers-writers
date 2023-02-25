# Readers-Writers Problem and its Solution

A starve-free solution to Readers-Writers problem with explanation.

### What is the Readers-Writers problem?

It is a classical example problem of handling synchronisation between programs accessing the same data. Here processes are of two types, **Reader** and **Writer**, how both function is explained below.

#### Readers
- They have read-only access to the shared data, which means they cannot modify the data.
- Multiple readers can simultaneously access the data as it won't cause any issues.

#### Writers
- They have the ability to modify the data.
- A writer process must execute individually, as it will be modifying the data and simultaneous access can cause data inconsistency.

---

### Terminology and Discussion
- **Critical Section**: The critical section is the segment of code which access the shared resource. In Readers-Writers problem, it is the part of the code which read and write data, as data is the shared resource.

- **Starvation**: It is the situation when a process is kept waiting for an extended period of time for a resource because of other processes being allocated the resource contiously over the process which is waiting. For example in the case of Readers-Writers problem, starvation can happen when a writer process is kept waiting while readers keep entering the critical section. Starvation is an issue which can have varying impacts on the performance of the system.

- **Semaphores**: Semaphores are a synchronisation technique used to control access to critical section. They have two operations mainly, `wait()` and `release()`. They are generally of two types, Binary and Counting, we will only be using Binary Semaphores in our implementation.

---

### Solution
#### Idea
We just need a synchronisation mechanism which lets a reader/writer enter critical section if no process is inside the critical section yet. A new reader can enter the critical section if there already are one or more reader processes inside the critical section. A writer can enter the critical section only if there is no other process inside the critical section. If we just implement this mechanism (which would need two semaphores, namely `rupdate` and `check`, and a variable `rd` which will hold count of reader processes in critical section) it would maintain exclusion and function but would cause *starvation*. So to solve starvation, we just need to ensure that a write process does get executed without starvation by continous entry of reader processes. So for that we would need an extra semaphore(`rwsync`) that would not let the writer wait behind readers indefinitely.

#### Pseudocode
Initialisation
```cpp
  semaphore check = 1;
  semaphore rupdate = 1;
  semaphore rwsync = 1;
  int rcount = 0;
```
Writer Implementation
```cpp
Writer:
{
  wait(rwsync);
  wait(check);
  //  write data - critical section
  write();
  // end of critical section
  release(check);
  release(rwsync);
} 
 
```
Reader Implementation
```cpp
Reader:
{
  wait(rwsync);
  wait(rupdate);
  rcount++;
  if(rcount == 1) 
  {
    // if the process is the first process entering the critical section 
    // if it is not the first then this must have been already set to wait
    wait(check);
  }
  release(rupdate);
  release(rwsync);
  // read data - critical section
  read();  
  // end of critcal section
  wait(rupdate)
  {
    rcount--;
  }
  if(rcount == 0)
  {
    release(check);
  }
  release(rupdate);
}
```

#### Explanation
The above pseudocode implements a mechanism which provides a starve-free solution to the Readers-Writers problem. `rcount` is present to check for first reader process entering the critcal section, which when is the case then calls `wait(check)` to check if any writer process is not executing and then `rcount` is again used to check if the current reader process was the only one in the critical section, when when true calls `release(check)` which can then let any waiting writer to move into the critical section. The purpose of semaphore `rupdate` is to ensure that the value of `rcount` is never accessed at multiple places simultaneously. Then the last semaphore `rwsync` is used to ensure that any reader process do not enter critical section if any Writer is already waiting hence solving the problem of *starvation*.

---
Sanidhya Singh 
(21114091)
