# Go Syntax Reference Structure

## Basic Essentials

Start with the "boilerplate" that every Go file needs.

Package Declaration: package main

Imports: Single vs. grouped imports.

The Entry Point: The func main() structure.

## Variables & Constants

Go is picky about how you declare things. Use this section to distinguish between the various styles.

Standard Declaration: var name string = "Gemini"

Short Declaration: name := "Gemini" (only inside functions!)

Constants: Using the const keyword.

Zero Values: A table showing that int is 0, string is "", etc.

## Data Types & Collections

This is where the syntax gets interesting. Focus on the differences between these three:

Arrays: Fixed length, e.g., [5]int.

Slices: Dynamic length, e.g., []int. Include the append() syntax.

Maps: Key-value pairs, e.g., map[string]int.

## Control Flow

Go keeps it simple—one loop to rule them all.

If/Else: Note that Go doesn't use parentheses () around conditions.

For Loops: Standard C-style, "while" style, and infinite loops.

Switch: No break needed by default!

Range: Iterating over slices and maps.

## Functions & Methods

Go functions can do things other languages can't.

Multiple Return Values: func swap(x, y string) (string, string)

Named Return Parameters: Improving readability.

Variadic Functions: Using the ... syntax.

Methods: Functions with a "receiver" (how Go handles "classes").

## Pointers & Structs

Since Go isn't purely object-oriented, these are your bread and butter.

Structs: Custom data types.

Pointers: Using & (address of) and * (dereference).

## Interfaces & Concurrency

The "Boss Level" of Go syntax.

Interfaces: Implicit implementation (no implements keyword).

Goroutines: The go keyword.

Channels: Sending and receiving data with <-.
