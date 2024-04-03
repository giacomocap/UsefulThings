Get the solutions that contains the table by name
```sql
SELECT s.uniquename AS SolutionName
FROM   solution AS s
       INNER JOIN
       solutioncomponent
       ON s.solutionid = solutioncomponent.solutionid
       INNER JOIN
       entity
       ON solutioncomponent.objectid = entityid
WHERE  entity.name = 'tb_keyproject';
```
