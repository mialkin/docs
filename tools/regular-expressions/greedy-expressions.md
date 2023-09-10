# Greedy expressions

Standard repetition quantifiers are greedy — expression tries to match the longest possible string:

`/\d+\w+\d+/` matches **01_FY_07_report_99**.xls

`/".+", ".+"/` matches **"Milton", "Waddams", "Initech, Inc."**

Greedy strategy defers to achieving overall match. Preceding expression part gives back to the next expression part as little as possible:

`/.*[0-9]+/` matches "Page 266"

`/.*/` matches "Page 26" while `/[0-9]+/` matches "6"

`/.+\.jpg/` matches "filename.jpg"

The `+` is greedy, but "gives back" the ".jpg" to make the match.
