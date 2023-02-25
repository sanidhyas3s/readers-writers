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

### Terminology and Discussion
- **Critical Section**: The critical section is the segment of code which access the shared resource. In Readers-Writers problem, it is the part of the code which read and write data, as data is the shared resource.

- **Starvation**: It is the situation when a process is kept waiting for an extended period of time for a resource because of other processes being allocated the resource contiously over the process which is waiting. For example in the case of Readers-Writers problem, starvation can happen when a writer process is kept waiting while readers keep entering the critical section. Starvation is an issue which can have varying impacts on the performance of the system.
