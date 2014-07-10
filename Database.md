# Transaction Management

How the DBMS handles concurrent executions is an important aspect of transaction management and the subject of concurrency control.

[Two-phase locking](http://en.wikipedia.org/wiki/Two-phase_locking) is the most common transaction control method in DBMSs, used to provide both serializability and recoverability for correctness. **2PL** may be subject to deadlocks that result from the mutual blocking of two or more transactions. A simple way to identify deadlocks is to use a timeout mechanism. If a transaction has been waiting too long for a lock, we can assume that it is in a deadlock cycle and abort it.

## The ACID Properties
- A : Atomic
- C : Consistency
- I : Isolation
- D : Durability

## Transaction Isolation

Of the four ACID properties in a DBMS, the [isolation](http://en.wikipedia.org/wiki/Isolation_%28database_systems%29) property is the one most often relaxed. When attempting to maintain the highest level of isolation, a DBMS usually acquires locks on data or implements multiversion concurrency control, which may result in a loss of concurrency.

### Four Isolation Levels
- Serializable: with a lock-based concurrency control, serializability requires read and write locks to be released at the end of the transaction. Also range-locks must be acquired to avoid the **phatom reads** phenomenon.
- Repeatable reads: a lock-based concurrency control keeps read and write locks util the end of the transaction. However, range-locks are not managed.
- Read commited: a lock-based concurrency control keeps write locks util the end of the transaction, but read locks are released as soon as the **SELECT** operation is performed (so the non-repeatable reads phenomenon can occur in this isolation level).
- Read uncommited: dirty reads are allowed.
                                            
### Read Phenomena

- Dirty reads
- Non-repeated reads
- [Phantom reads](http://en.wikipedia.org/wiki/Isolation_%28database_systems%29#Phantom_reads): A phantom read occurs when, in the course of a trasaction, two identical queries are executed, and the collection of rows returned by the second query is different from the first. This can occur when range locks are not acquired on perform a **SELECT...WHERE** operation. The phantom reads anomaly is a special case of Non-repeatable reads when Transaction 1 repeats a ranged **SELECT...WHERE** query and, between both operations, Transaction 2 creates new rows which fulfill that **WHERE** clause.