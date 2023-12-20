# Debugging DAGs

This section addresses common issues associated with DAG code that developers may encounter during development.

## DAG Import Error

Encountering an `Import Error` in the Airflow UI often indicates that Airflow is unable to parse the DAG. To identify import errors in your terminal, execute `airflow dags list-import-errors` using the Airflow CLI.

If there is no import error message, and your DAGs still do not appear in the UI, consider the following debugging steps:

- Confirm that all DAG files are situated in the designated `dags` folder.
- Verify your permissions to access DAGs and ensure the correct permissions on the DAG file.
- Examine Python syntax errors using a linter like pylint.
- Execute `python3 dags/file_name.py` in the CLI to identify errors in a specific file.
- Run `airflow dags list` with the Airflow CLI to verify DAG registration in the metadata database. If the DAG appears in the list but not in the UI, attempt restarting the Airflow webserver.
- Utilize IDE debugging tools to debug your DAG code with the `dag.test()` method, introduced in Airflow 2.5.

## Slow Appearance of DAG in UI

Airflow scans the `dags` folder for new DAGs every `dag_dir_list_interval` (environment variable in Docker Compose: `AIRFLOW__SCHEDULER__DAG_DIR_LIST_INTERVAL`), defaulting to 5 minutes but modifiable. You may need to wait until this interval elapses or restart your Airflow environment for a new DAG to appear in the UI.

## DAG/Task Execution Failure

Ensure your code adheres to the SOLID principles and generates informative Python log messages.

## Scheduler Inactivity

If an error in the Airflow UI indicates that the scheduler is not running, inspect the scheduler logs for potential DAG file errors causing the scheduler to crash.

To check the status of the Airflow scheduler container, execute `docker ps` to view all container statuses. If the scheduler container is missing or unhealthy, restart all containers by running `docker compose down` followed by `docker compose up -d`. Then, recheck the status of the scheduler container.

# References

- [5 MUST KNOW Airflow Debug Tips and Tricks](https://www.youtube.com/watch?v=5QxqqeOxJhI) by coder2j
- [Debugging DAGs](https://docs.astronomer.io/learn/debugging-dags) by astronomer
