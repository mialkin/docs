# Terminology: context switch, critical section, spinning

## Table of contents

- [Terminology: context switch, critical section, spinning](#terminology-context-switch-critical-section-spinning)
  - [Table of contents](#table-of-contents)
  - [Context switch](#context-switch)
  - [Critical section](#critical-section)
  - [Spinning](#spinning)

## Context switch

A **context switch** is the process of storing the state of a process or thread, so that it can be restored and resume execution at a later point.

## Critical section

A **critical section** is a segment of code which is not reentrant; that is, it does not support concurrent access by multiple threads. Often, a critical section is used to protect shared resources.

## Spinning

A **spinning** or **busy-waiting** is a technique in which a process repeatedly checks to see if a condition is true, such as whether keyboard input or a lock is available.
