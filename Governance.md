`ALTER securable_object SET OWNER TO principal
more on ALTER: https://docs.databricks.com/aws/en/sql/language-manual/sql-ref-syntax-ddl-alter-table
`GRANT privilege_types ON securable_object TO principal`
`REVOKE privilege_types ON securable_object FROM principal`

privilege_types: https://docs.databricks.com/aws/en/data-governance/unity-catalog/manage-privileges/privileges

securable_object: https://docs.databricks.com/aws/en/sql/language-manual/sql-ref-privileges#securable-objects

principal: https://docs.databricks.com/aws/en/sql/language-manual/sql-ref-principal

| Privilege      | Purpose                | Scope                               | Usecase                                   | Restriction                         |
| -------------- | ---------------------- | ----------------------------------- | ----------------------------------------- | ----------------------------------- |
| SELECT         | Read data              | Query tables or views               | Grant read-only access to analysts        | Can't modify or create data         |
| USAGE          | Access schema/database | Use metadata without accessing data | Grant metadata access for exploration     | Can't query or modify data          |
| MODIFY         | Modify table contents  | Insert, update, delete data         | Grant write access to ETL processes       | Can't create or alter objects       |
| CREATE         | Create objects         | Define tables, view, or functions   | Allow developers to create schema objects | Can't query or modify existing data |
| ALL PRIVILEGES | Full control           | Combines all privileges             | Assign to admins or power users           | Risk of excessive access            |
Data Explorer -> Catalog Explorer

https://docs.databricks.com/en/sql/user/alerts/index.html