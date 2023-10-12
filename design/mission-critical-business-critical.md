# Mission critical, business critical, business operational, office productivity

## Table of contents

- [Mission critical, business critical, business operational, office productivity](#mission-critical-business-critical-business-operational-office-productivity)
  - [Table of contents](#table-of-contents)
  - [Mission critical, MC](#mission-critical-mc)
    - [Signs](#signs)
    - [Uptime](#uptime)
    - [Examples](#examples)
  - [Business critical plus, BC+](#business-critical-plus-bc)
    - [Signs](#signs-1)
    - [Uptime](#uptime-1)
    - [Examples](#examples-1)
  - [Business critical, BC](#business-critical-bc)
    - [Signs](#signs-2)
    - [Uptime](#uptime-2)
    - [Examples](#examples-2)
  - [Business operational, BO](#business-operational-bo)
    - [Signs](#signs-3)
    - [Uptime](#uptime-3)
    - [Examples](#examples-3)
  - [Office productivity, OP](#office-productivity-op)
    - [Signs](#signs-4)
    - [Uptime](#uptime-4)
    - [Examples](#examples-4)

## Mission critical, MC

### Signs

- In case of prolonged problems, risk losing the entire business
- Risk of license revocation due to prolonged downtime and failure to provide services
- Risk of huge fines for the company
- Mandatory regulatory requirements: anti-fraud, blocking, reporting, etc.
- Systems on which MC, BC+ and BC systems depend

### Uptime

99.95% or 43 seconds daily.

<https://en.wikipedia.org/wiki/High_availability#Percentage_calculation>.

<https://uptime.is>.

### Examples

- SSO
- Money transfers
- Payments

## Business critical plus, BC+

### Signs

- Base platforms on which many other BC systems depend
- Risk of tangible fines for the company
- Strong customer service impact, e.g. chats

### Uptime

99.9% or 1 minute 26 seconds daily.

### Examples

- Customer support chats
- Log aggregation tool like ELK
- GitLab
- Employees' VPN

## Business critical, BC

### Signs

- Base business line services
- Risk of financial loss that is tangible to the business line
- Risk of serious customer dissatisfaction with widespread discussion on social networks

### Uptime

99.8% or 2 minutes 53 seconds daily.

### Examples

- Providing certificates and statements
- Office WiFi

## Business operational, BO

### Signs

- Systems important for employees not involved in direct customer services: call center, company's representatives

### Uptime

99.5% or 7 minutes 12 seconds daily.

### Examples

- Jira
- Confluence

## Office productivity, OP

### Signs

- Systems that have a full-fledged alternative solutions
- Downtime affects employee convenience, but not customer convenience
- Downtime affects internal background processes that are acceptable to stop for a weekend or 1-2 business days

### Uptime

95% or 1 hour 12 minutes daily.

### Examples

- HR portal
