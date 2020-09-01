# PL/SQL

Function or operator | Description
-|-
`\|\|` | Сoncatenates 2 or more strings together.<br>'Tech on' \|\| ' the Net'
`CONCAT(string1, string2)` | Concatenates two strings together.<br>`CONCAT('Tech on', ' the Net')`
NVL | Substitutes `Null` value with other value.<br>`SELECT NVL(supplier_city, 'n/a')`<br>`FROM suppliers;`
ROWNUM | Works like `TOP` in T-SQL.<br>`SELECT *`<br>`FROM MY_TABLE`<br>`WHERE ROWNUM <= 80;`
SQL % ROWCOUNT | Number of rows affected by an `UPDATE`<br>`DECLARE`<br>`I NUMBER;`<br>`BEGIN`<br>`UPDATE MY_TABLE`<br>`SET USR_CORRECTOR = 'Aleksei'`<br>`WHERE ID = 2739875;`<br>`I := SQL % ROWCOUNT;`<br>`END;`
TO_DATE | `SELECT TO_DATE('2012-06-05', 'YYYY-MM-DD')`<br>`FROM DUAL;`
