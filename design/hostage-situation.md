# Hostage situation

A **hostage situation** is a situation where progress, control, or stability is blocked because one critical element is controlled, unavailable, or fragile—so everything else is effectively "held hostage".

## Table of contents

- [Hostage situation](#hostage-situation)
  - [Table of contents](#table-of-contents)
  - [Knowledge hostage (bus factor = 1)](#knowledge-hostage-bus-factor--1)
    - [Symptoms](#symptoms)
    - [Risks](#risks)
    - [Fix](#fix)
  - [Legacy code hostage](#legacy-code-hostage)
    - [Symptoms](#symptoms-1)
    - [Risks](#risks-1)
    - [Fix](#fix-1)
  - [Dependency / vendor hostage](#dependency--vendor-hostage)
    - [Examples](#examples)
    - [Risks](#risks-2)
    - [Fix](#fix-2)
  - [Build / deployment hostage](#build--deployment-hostage)
    - [Symptoms](#symptoms-2)
    - [Risks](#risks-3)
    - [Fix](#fix-3)
  - [Data hostage](#data-hostage)
    - [Examples](#examples-1)
    - [Fix](#fix-4)


## Knowledge hostage (bus factor = 1)

One developer is the only person who understands a critical part of the system.

### Symptoms

- "Only Alex knows how this works"
- No documentation
- Fear of refactoring

### Risks

- Alex leaves → project stalls
- Bugs can't be fixed safely

### Fix

- Documentation
- Code reviews

## Legacy code hostage

A system depends on old, fragile, or poorly written code that no one dares to touch.

### Symptoms

- "Don't change this, it might break production"
- No tests
- Tight coupling

### Risks

- Slow development
- Bugs persist forever

### Fix

- Add tests before refactoring
- [↑ Strangler fig pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig)
- Incremental modernization

## Dependency / vendor hostage

Your system depends on an external library, service, or vendor you can't easily replace.

### Examples

- Abandoned open-source library
- Proprietary API with bad support
- Cloud service with lock-in

### Risks

- Security vulnerabilities
- Forced pricing or breaking changes

### Fix

- Abstraction layers
- Exit strategy
- Open standards

## Build / deployment hostage

Only one machine, script, or person can build or deploy the system.

### Symptoms

- "It only builds on my laptop"
- Manual production deployments

### Risks

- Downtime
- No scalability

### Fix

- CI/CD
- Infrastructure as code
- Automated builds

## Data hostage

Data is stored in a way that prevents change.

### Examples

- No migrations
- Hard-coded assumptions
- No backups

### Fix

- Versioned schemas
- Migration scripts
- Backups + restore tests
