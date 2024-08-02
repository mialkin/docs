# Air transportation database

## Table of contents

- [Air transportation database](#air-transportation-database)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Common diagram](#common-diagram)
  - [Tables, functions](#tables-functions)
    - [`bookings`](#bookings)
    - [`now` (function)](#now-function)
    - [`tickets`](#tickets)
    - [`tickets_flights`](#tickets_flights)
    - [`flights`](#flights)
    - [`airports`](#airports)
    - [`aircrafts`](#aircrafts)
    - [`seats`](#seats)
    - [`boarding_passes`](#boarding_passes)

## Installation

Run Postgres with [docker-compose.yaml](docker-compose.yaml) file:

```bash
docker-compose up --detach
```

Restore database:

```bash
cat /Users/aleksei/Downloads/demo-big-20170815.sql | docker exec --interactive air-transportation \
psql -U postgres
```

## Common diagram

```plantuml
@startuml

' configuration
hide circle
skinparam linetype ortho

skinparam class {
    BackgroundColor #ffffff
    BorderColor Black
    FontStyle bold
 FontSize 13
}

entity bookings {
 book_ref
 --
}

entity tickets {
 ticket_no
 --
}

entity ticket_flights {
 ticket_no
 flight_id
 --
}

entity flights {
 flight_id
 --
 departure_airport
 arrival_airport
 aircraft_code
}

entity airports {
 airport_code
 --
}

entity boarding_passes {
 ticket_no
 flight_id
 --
}

entity aircrafts {
 aircraft_code
 --
}

entity seats {
 aircraft_code
 seat_no
 --
}


bookings --|{ tickets
tickets --|{ ticket_flights
ticket_flights -- boarding_passes
flights --|{ ticket_flights
airports --|{ flights
airports --|{ flights
aircrafts --|{ flights
aircrafts --|{ seats

@enduml
```

## Tables, functions

### `bookings`

```sql
SELECT *
FROM bookings
WHERE book_ref = '0824C5';
```

| book_ref | book_date                         | total_amount |
| :------- | :-------------------------------- | :----------- |
| 0824C5   | 2017-07-25 20:36:00.000000 +00:00 | 112400.00    |

|                |                |                                           |
| -------------- | -------------- | ----------------------------------------- |
| _Booking code_ | _Booking date_ | _Total sum of all tickets inside booking_ |

### `now` (function)

```sql
SELECT bookings.now();
```

| now                               |
| :-------------------------------- |
| 2017-08-15 15:00:00.000000 +00:00 |

```sql
SELECT bookings.now() - b.book_date AS "Date difference"
FROM bookings b
WHERE b.book_ref = '0824C5';
```

| Date difference                                  |
| :----------------------------------------------- |
| 0 years 0 mons 20 days 18 hours 24 mins 0.0 secs |

### `tickets`

```sql
SELECT *
FROM tickets;
```

| ticket_no     | book_ref | passenger_id | passenger_name  | contact_data              |
| :------------ | :------- | :----------- | :-------------- | :------------------------ |
| 0005432000284 | 1A40A1   | 4030 855525  | MIKHAIL SEMENOV | {"phone": "+70110137563"} |

### `tickets_flights`

| Column | Description |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |
|        |             |

### `flights`

| Column | Description |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |
|        |             |
|        |             |
|        |             |
|        |             |

### `airports`

| Column | Description |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |
|        |             |
|        |             |

### `aircrafts`

| Column | Description |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |
|        |             |
|        |             |

### `seats`

| Column | Description |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |

### `boarding_passes`

| Column | Description |
| ------ | ----------- |
|        |             |
|        |             |
|        |             |
|        |             |
