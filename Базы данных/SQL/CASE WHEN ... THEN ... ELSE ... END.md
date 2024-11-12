```sql
UPDATE world_population 
SET migrants = CASE WHEN id = 1 THEN -348399 
					WHEN id = 2 THEN -532687 
					WHEN id = 3 THEN 954806 
				END;
```