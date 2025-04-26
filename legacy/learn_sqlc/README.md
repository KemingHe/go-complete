# Getting Started with SQLC and PostgreSQL

This tutorial demonstrates how to use [sqlc](https://sqlc.dev/) to generate Go code for interacting with a PostgreSQL database.

## Prerequisites

- Latest version of sqlc [installed](https://docs.sqlc.dev/en/stable/overview/install.html)
- Go toolchain (if you want to compile and run the example code)
- PostgreSQL database (for running the final example)

## Project Structure

```text
.
├── go.mod
├── go.sum
├── main.go        # Example program using generated code
├── query.sql      # SQL queries to generate code from
├── schema.sql     # Database schema definition
├── sqlc.yaml      # sqlc configuration
└── tutorial/      # Generated Go code
    ├── db.go
    ├── models.go
    └── query.sql.go
```

## Setting Up

1. Initialize a new Go module (done automatically in this project):

    ```shell
    go mod init tutorial.sqlc.dev/app
    ```

2. Create a configuration file named `sqlc.yaml`:

    ```yaml
    version: '2'
    sql:
    - schema: schema.sql
      queries: query.sql
      engine: postgresql
      gen:
        go:
          package: tutorial
          out: tutorial
          sql_package: 'pgx/v5'
    ```

## Database Schema and Queries

1. We have a simple schema in `schema.sql`:

    ```sql
    CREATE TABLE authors (
      id BIGSERIAL PRIMARY KEY,
      name text NOT NULL,
      bio text
    );
    ```

2. Our queries are defined in `query.sql`:

    ```sql
    -- name: GetAuthor :one
    SELECT * FROM authors
    WHERE id = $1 LIMIT 1;

    -- name: ListAuthors :many
    SELECT * FROM authors
    ORDER BY name;

    -- name: CreateAuthor :one
    INSERT INTO authors (
      name, bio
    ) VALUES (
      $1, $2
    )
    RETURNING *;

    -- name: UpdateAuthor :exec
    UPDATE authors
      set name = $2, bio = $3
    WHERE id = $1
    RETURNING *;

    -- name: DeleteAuthor :exec
    DELETE FROM authors
    WHERE id = $1;
    ```

Each query is annotated with a comment that defines its name and return type:

- `:one` - Returns a single row
- `:many` - Returns multiple rows
- `:exec` - Doesn't return any rows (for INSERT, UPDATE, DELETE operations)

## Generating Code

Run the following command to generate code:

```shell
sqlc generate
```

This creates the following files in the `tutorial` directory:

- `db.go` - Database interface definition
- `models.go` - Go structs representing database tables
- `query.sql.go` - Generated methods for each query

## Using the Generated Code

Check the [`main.go`](./main.go) file for a complete example of how to use the generated code. To run this example, you'll need to:

1. Install the PostgreSQL driver:

    ```shell
    go get github.com/jackc/pgx/v5
    ```

2. Update the connection string in `main.go` with your database credentials

3. Compile and run:

    ```shell
    go build ./app
    ```

## Benefits of SQLC

- Type-safe database operations
- No need to write boilerplate SQL mapping code
- Compile-time SQL validation
- Excellent IDE integration with autocomplete
- Generated code is readable and follows Go idioms

For more information, visit the [official sqlc documentation](https://docs.sqlc.dev/).
