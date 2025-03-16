# ACID

## A -> Atomicity

The idea comes from the atom it self that cannot be divided. This ensures that transactions
are just finished if it doesn't occur any errors in the process. If a error
occurs nothing will be completed.

### Atomic operators

- `$set`
- `$unset`

### Example

![[Pasted image 20250316172645.png]]

## C -> Consistency

Consistency is used to ensure Schemas constraints, foreign key constraints and
compliance to ACID. For noSQL databases this is kind of different because we
don't have a obligation to have schemas neither foreign keys

## I -> Isolation

Concurrent transactions will not interfere with each other. It's related to
atomicity in some way. Transaction are executed by a defined order to ensure
data is not corrupted.

## D -> Durability

Data will not be lost after written.
