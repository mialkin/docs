# GoF notation

Slanted type indicates that the class or operation is abstract.

## Part-of or aggregation relationship

An object reference representing a part-of or aggregation relationship is indicated by an arrowheaded line with a diamond at the base. The arrow points to the class that is aggregated (e.g., Shape).

<img src="aggregation.svg">

## Acquaintance

An arrowheaded line without the diamond denotes acquaintance (e.g., a LineShape keeps a reference to a Color object, which other shapes may share). A name for the reference may appear near the base to distinguish it from other references.

<img src="acquaintance.svg">

When the circle appears at the head of a reference, it means multiple objects are being referenced or aggregated.


## "Creates" relationship

Another useful thing to show is which classes instantiate which others. We use a dashed arrowheaded line to indicate this. We call this the "creates" relationship. The arrow points to the class that's instantiated.

<img src="creates.svg">

