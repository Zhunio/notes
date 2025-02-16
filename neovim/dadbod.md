# DADBOD

Dadbod is a Neovim plugin that provides database integration, allowing users to execute SQL queries directly from the editor. It supports multiple database systems and offers a seamless workflow for database management within Neovim.

## Databases

Use the following environment variables to configure the databases you want to connect to:

MySQL:

```bash
export DB_UI_PEOPLE=mysql://root:password@localhost:3306/people?protocol=tcp
```

Postgres:

```bash
export DB_UI_PEOPLE=postgres://myuser:mypassword@localhost:5432/people
```
