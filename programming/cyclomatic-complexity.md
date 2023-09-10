# Cyclomatic complexity

**Cyclomatic complexity** is a software metric used to indicate the complexity of a program. It is a quantitative measure of the number of linearly independent paths through a program's source code. It was developed by Thomas J. McCabe, Sr. in 1976.

Cyclomatic complexity is computed using the control-flow graph of the program: the nodes of the graph correspond to indivisible groups of commands of a program, and a directed edge connects two nodes if the second command might be executed immediately after the first command. Cyclomatic complexity may also be applied to individual functions, modules, methods or classes within a program.

[↑ Cyclomatic complexity](https://en.wikipedia.org/wiki/Cyclomatic_complexity)

## Table of contents

- [Cyclomatic complexity](#cyclomatic-complexity)
  - [Table of contents](#table-of-contents)
  - [Example](#example)
  - [Basis path testing](#basis-path-testing)
  - [Limiting complexity during development](#limiting-complexity-during-development)
  - [Correlation to number of defects](#correlation-to-number-of-defects)

## Example

If the source code contained no control flow statements (conditionals or decision points), the complexity would be 1, since there would be only a single path through the code. If the code had one single-condition `IF` statement, there would be two paths through the code: one where the `IF` statement evaluates to `TRUE` and another one where it evaluates to `FALSE`, so the complexity would be 2. Two nested single-condition `IF`s, or one `IF` with two conditions, would produce a complexity of 3.

[↑ Rigorous mathematical definition](https://en.wikipedia.org/wiki/Cyclomatic_complexity#Definition).

## Basis path testing

The **basis path testing**, or the **structured testing**, is a [↑ white box](https://en.wikipedia.org/wiki/White-box_testing) method for designing test cases.

The method analyzes the control-flow graph of a program to find a set of linearly independent paths of execution. The method normally uses McCabe cyclomatic complexity to determine the number of linearly independent paths and then generates test cases for each path thus obtained.

Basis path testing guarantees complete branch coverage (all edges of the control-flow graph), but achieves that without covering all possible [↑ paths](https://en.wikipedia.org/wiki/Path_(graph_theory)) of the control-flow graph – the latter is usually too costly. Basis path testing has been widely used and studied.

## Limiting complexity during development

One of McCabe's original applications was to limit the complexity of routines during program development; he recommended that programmers should count the complexity of the modules they are developing, and split them into smaller modules whenever the cyclomatic complexity of the module exceeded 10. This practice was adopted by the NIST Structured Testing methodology, with an observation that since McCabe's original publication, the figure of 10 had received substantial corroborating evidence, but that in some circumstances it may be appropriate to relax the restriction and permit modules with a complexity as high as 15. As the methodology acknowledged that there were occasional reasons for going beyond the agreed-upon limit, it phrased its recommendation as "For each module, either limit cyclomatic complexity to the agreed-upon limit or provide a written explanation of why the limit was exceeded."

## Correlation to number of defects

A number of studies have investigated the correlation between McCabe's cyclomatic complexity number with the frequency of defects occurring in a function or method. Some studies find a positive correlation between cyclomatic complexity and defects: functions and methods that have the highest complexity tend to also contain the most defects. However, the correlation between cyclomatic complexity and program size (typically measured in lines of code) has been demonstrated many times.

Les Hatton has claimed that complexity has the same predictive ability as lines of code. Studies that controlled for program size (i.e., comparing modules that have different complexities but similar size) are generally less conclusive, with many finding no significant correlation, while others do find correlation.

Some researchers who have studied the area question the validity of the methods used by the studies finding no correlation. Although this relation is probably true, it isn't easily utilizable. Since program size is not a controllable feature of commercial software, the usefulness of McCabes's number has been called to question. The essence of this observation is that larger programs tend to be more complex and to have more defects.

Reducing the cyclomatic complexity of code is not proven to reduce the number of errors or bugs in that code. International safety standards like ISO 26262, however, mandate coding guidelines that enforce low code complexity.
