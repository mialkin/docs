# Graphviz & PlantUML

## Graphviz

- [↑ Graphviz](https://graphviz.org)

```dot
digraph DiagramName {

  labelloc="t";
  label="Diagram title";

  A [label="This is A"]
  B [label="This is B"]
  C [label="This is C"]
  D [label="This is D"]

  A -> B
  B -> { C, D }

}
```

## PlantUML

- [↑ PlantUML](https://plantuml.com)

- [↑ PlantUML Server](https://github.com/plantuml/plantuml-server)

```bash
docker run -d -p 8080:8080 plantuml/plantuml-server:jetty
```

Visual Studio code settings:

```json
"plantuml.server": "http://localhost:8080"
```

Example:

```plantuml
@startuml
:Main Admin: as Admin
(Use the application) as (Use)

User -> (Start)
User --> (Use)

Admin ---> (Use)

note right of Admin : This is an example.

note right of (Use)
  A note can also
  be on several lines
end note

note "This note is connected\nto several objects." as N2
(Start) .. N2
N2 .. (Use)
@enduml
```
