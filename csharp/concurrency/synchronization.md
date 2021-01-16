# Synchronization

There are two major types of synchronization: *communication* and *data protection*. Communication is used when one piece of code needs to notify another piece of code of some condition (e.g., a new message has arrived). I’ll cover communication more thoroughly in this chapter’s recipes; the remainder of this introduction discusses data protection.

You need to use synchronization to protect shared data when all three of these conditions are true:

* Multiple pieces of code are running concurrently.
* These pieces are accessing (reading or writing) the same data.
* At least one piece of code is updating (writing) the data.
