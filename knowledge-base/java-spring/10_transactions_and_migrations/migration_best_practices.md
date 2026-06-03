# Migration Best Practices

Database migrations should be designed for safe deployment.

This matters especially when services run in multiple instances or deployments
must avoid downtime.

## Prefer Small Migrations

Small migrations are easier to review, test, and troubleshoot.

## Be Careful With Blocking Changes

Operations such as adding indexes, changing column types, or adding non-null
columns with defaults can lock large tables depending on the database.

Test heavy migrations with realistic data volume.

## Expand And Contract

For breaking schema changes, use a staged approach:

```text
1. add new nullable column
2. deploy app that writes both old and new shape
3. backfill data
4. deploy app that reads new shape
5. remove old column later
```

This reduces deployment risk.

## Do Not Edit Applied Migrations

Once a migration has been applied to shared environments, create a new migration
instead of editing the old one. Editing applied migrations breaks history and
checksums.

## Key Idea

Migration safety is deployment safety. Design schema changes so they can run
predictably across environments.
