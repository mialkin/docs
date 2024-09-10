# OLTP, OLAP

The word "online" in definitions means that systems respond to user requests and process transactions in real-time.

- [OLTP, OLAP](#oltp-olap)
  - [OLTP](#oltp)
  - [OLAP](#olap)
  - [OLTP vs OLAP](#oltp-vs-olap)

## OLTP

**Online transaction processing** or **OLTP** is a type of database management system that is designed to support the processing of transactions in real-time.

OLTP is the most common type of database used in business applications and is widely used in financial institutions, retail stores, and other businesses.

OLTP systems are designed to handle a large volume of data transactions and ensure that all updates are completed quickly and efficiently. They typically use a client-server architecture, where a client application connects to a server that contains the database. The server then processes the transaction and updates the database accordingly.

OLTP databases are typically used for storing and retrieving data in a structured manner, which makes them ideal for use in business applications.

## OLAP

**Online analytical processing** or **OLAP** is an analytical tool that allows businesses to analyze large amounts of data quickly and easily.

OLAP is used to generate reports, charts, and graphs that help businesses make informed decisions.

OLAP databases are optimized for heavy read, low write workloads which allows them to extract business intelligence information in a highly performant way.

OLAP is part of the broader category of [↑ business intelligence](https://en.wikipedia.org/wiki/Business_intelligence).

## OLTP vs OLAP

The main difference between OLTP and OLAP is that OLTP is used for transaction processing, while OLAP is used for analysis. OLTP databases store and retrieve data in a relational manner, while OLAP databases store data in a multidimensional manner.
