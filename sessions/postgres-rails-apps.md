# Puny to Powerful Postgresql Rails Apps

Andy Atkinson (Fountain)

[slides](https://github.com/cameron-hicks/RailsConf22/blob/main/sessions/postgres-rails-apps.md)

About challenges and solutions for database scaling. He talked pretty fast so I wasn't able to write much down. The slides are thorough, though.

## Safe migrations

- summary of how to do migrations safely: change new rows and existing rows separately, in separate migration
- unsafe variation: \_\_\_
- safe variation: add the check constraint without validating rows and validate separately.
  This is less disruptive, but you should run offpeak or in downtime.
- Data backfills:

  - unsafe variation: backfilling in the same transaction that alters a table keeps the table locked for the duration of the backfill
  - three keys to backfilling safely:
    - \_\_
    - \_\_
    - \_\_
  - example of a safe variation of a migration with backfill:
    ```ruby
    # something
    ```

- migrataions best practices recap:
  - covered potentially unsafe migrations and alternatives
  - \_\_
  - \_\_
- [Strong Migrations docs](https://www.rubydoc.info/gems/strong_migrations/0.3.1)

## Connections

- use a connection pooler like PgBouncer

## Slow queries

- fix the most resource-intensive slow queries
- use PgHero (performance dashboard) to analyze & optimize
  - slow queries
  - unused and invalid indexes
  - high connections
- bloat = symptom of fragmented data. On tables that are heavily updated, (something) happens with the indexes

## Maintenance

- rebuild bloated indexes
- cron jobs for databases

## Replication & partitioning

- do writes on al primary database and reads on a replica
- partition very large tables
- example problem #1: you do an analysis and discover you're doing 10x more reads than writes, but all writes and reads are happening on a single database
  - a solution: set up a read replica for the read workload using Postgres streaming replication and Rails' Multiple Databases feature to create a primary and a replica
  - Rails will not run migrations against the replica
  - consider replication lag
- example problem #2: \_\_\_\_
  - solution: can use Postgres Table Partitioning
  - implementation challenges:
    - requires downtime or dual writing
    - need to test queries on partitioned table before rollout
    - \_\_\_
  - gems you can use for this:
    - pgslice
    - \_\_\_
- when to adopt partitions? when is the pain worth it?
  - up to you, but some candidates: high-write tables, most heavily written & queried tables where there's a natural way to batch the data together

## Summary slides
