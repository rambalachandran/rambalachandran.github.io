---
title:  "Git Pre-Commit Hooks for Linting and Styling SQL Codes"
image:  /assets/images/blog_posts/docker_logo.png
author: Ram Balachandran
# comments: false
# date:   2020-08-23 08:30:00 +0530
categories:
    - Git
    - Hooks
    - Commit
    - SQL
    - sqlfluff
    - Python
    - Toml
layout: post
---

Git hooks are a simple, easy way to do small local scripting to improve code quality and readability. Using consistent git hooks across projects can reduce the load on CI/CD process which can focuse more on unit and integration testing.

## Pre-commit 
A pre-commit git hook is a shell file located inside the git repository in the following location
 `.git/hooks/pre-commit`. Infact, when you create a git repository, a sample file is available at `.git/hooks/pre-commit.sample`. In this blog, we will look at how to use pre commit hook to automatically format and lint python and SQL code.

## SQLFluff
[sqlfluff](https://github.com/sqlfluff/sqlfluff) is a python base formatter and linter for SQL files that is used in most dbt-sql models. The configuration of the sqlfluff can be achieved by updating the `pyproject.toml` a configuration format supported by python. The configuration corresponding to `sqlfluff` for `bigquery` in `dbt` is as follows,

```toml
[tool.sqlfluff.core]
templater = "jinja"
dialect = "bigquery"
# sql_file_exts = .sql,.sql.j2,.dml,.ddl

[tool.sqlfluff.rules]
comma_style = "leading"
tab_space_size = 4
max_line_length = 80

[tool.sqlfluff.indentation]
indented_joins = false
indented_using_on = true
template_blocks_indent = false

[tool.sqlfluff.templater]
unwrap_wrapped_queries = true

[tool.sqlfluff.templater.jinja]
apply_dbt_builtins = true
```
## Formatting and Linting with SQLFluff
To format sql code before commit, you need to get a list of SQL files that are staged for commit. This script assumes, all your sql files end with `*.sql`. However, its simple to change the script if you have any other custom file extension.

```sh
# Get the list of SQL files staged to be committed
sql_files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.sql$')
```
Once you get the list of staged files, you can iterate over the list of files and fix them as given below

```sh
# fix sql files with sqlfluff
for file in $sql_files
do
    sqlfluff fix  "$file"
done
```
Upon formatting and fixing SQL files, they need to be staged again for commit

```sh
# stage files again with extension .sql
git add -u '*.sql'
```
Once staged, the sql code needs to be linted again for any errors. 

```sh
for file in $sql_files
do
    sqlfluff lint  "$file"
    if [ $? -ne 0 ]; then
        echo "sqlfluff found errors in $file. Commit aborted."
        exit 1
    fi
done
```

So, the full script appears as follows

```sh
#!/bin/bash

# Get the list of SQL files staged to be committed
sql_files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.sql$')

# fix sql files with sqlfluff
for file in $sql_files
do
    sqlfluff fix  "$file"
done

# stage modified files again
git add -u

# Lint each sql file staged with sqlfluff
for file in $sql_files
do
    sqlfluff lint  "$file"
    if [ $? -ne 0 ]; then
        echo "sqlfluff found errors in $file. Commit aborted."
        exit 1
    fi
done
```

## Conclusion
`sqlfluff` is a package that can be used to format and lint sql code for many dialects for dbt. In this blog post, we discuss how to use `pre-commit` hooks of git to automatically format and lint sql codes. 


