# Graphviz, PlantUML, Mermaid

## Graphviz

- [↑ Graphviz](https://graphviz.org).

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

[↑ PlantUML](https://plantuml.com).

[↑ PlantUML Server](https://github.com/plantuml/plantuml-server).

```bash
docker run -d -p 7077:8080 plantuml/plantuml-server:jetty
```

Visual Studio code settings:

```json
"plantuml.server": "http://localhost:7077"
```

An `example.puml` file:

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

## Mermaid

[↑ Mermaid](https://mermaid.js.org) is a JavaScript based diagramming and charting tool that renders Markdown-inspired text definitions to create and modify diagrams dynamically.

GitHub now supports Mermaid syntax in Markdown:

```text
flowchart TB
    c1-->a2
    subgraph one
    a1-->a2
    end
    subgraph two
    b1-->b2
    end
    subgraph three
    c1-->c2
    end
```

which renders to:

```mermaid
flowchart TB
    c1-->a2
    subgraph one
    a1-->a2
    end
    subgraph two
    b1-->b2
    end
    subgraph three
    c1-->c2
    end
```
