# Liquibase Basics

Liquibase manages database migrations through changelogs.

Changelogs can be written in XML, YAML, JSON, or SQL. Liquibase is often chosen
when teams want structured change metadata, rollbacks, labels, contexts, or more
advanced migration control.

## YAML Changeset Example

```yaml
databaseChangeLog:
  - changeSet:
      id: 001-create-orders
      author: backend
      changes:
        - createTable:
            tableName: orders
            columns:
              - column:
                  name: id
                  type: bigint
                  constraints:
                    primaryKey: true
                    nullable: false
              - column:
                  name: status
                  type: varchar(40)
                  constraints:
                    nullable: false
```

## SQL Changesets

Liquibase can also use SQL changesets when the team prefers direct SQL.

## Flyway Versus Liquibase

Flyway is often simpler for versioned SQL migrations. Liquibase provides a more
structured migration model and additional controls. The better choice depends on
team workflow and operational requirements.

## Key Idea

Liquibase is useful when migrations need structured metadata and more migration
management features than simple versioned SQL files.
