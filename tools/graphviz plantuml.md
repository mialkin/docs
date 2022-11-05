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
- [↑PlantUML Server](https://github.com/plantuml/plantuml-server)

Visual Studio code settings:

```json
"plantuml.server": "http://localhost:8080"
```
