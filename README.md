
#Introduction

This project involves designing and implementing a Minimum Viable Product (MVP) for a Linux Cluster Monitoring solution. The goal is to help the LCA (Linux Cluster Administration) team monitor and gather metrics from their Linux cluster nodes. This MVP will allow users to write SQL queries to retrieve and analyze data such as memory usage, CPU utilization, and other hardware specifications and usage statistics.

The primary users are system administrators and developers who need insights into their Linux clusters. The technologies used in this project include Bash scripting for automation, Docker for containerization, PostgreSQL for data storage, and Git for version control.


#Quick Start

-Start a PostgreSQL instance using psql_docker.sh
./scripts/psql_docker.sh start

-Create tables using ddl.sql
psql -h localhost -U postgres -f ./sql/ddl.sql

-Insert hardware specs data into the DB using host_info.sh
./scripts/host_info.sh

-Insert hardware usage data into the DB using host_usage.sh
./scripts/host_usage.sh

-Set up crontab to run host_usage.sh at regular intervals
crontab -e
Add the following line to run the script every minute
* * * * * /path/to/scripts/host_usage.sh > /tmp/host_usage.log 2>&1

#Implementation

#Architecture

The architecture consists of three Linux hosts, each running a monitoring agent. These agents collect hardware and usage data and store it in a central PostgreSQL database. The setup is containerized using Docker for ease of deployment and management.

#Scripts

psql_docker.sh
This script manages the lifecycle of the PostgreSQL Docker container.

bash

Usage
./scripts/psql_docker.sh start|stop|create
host_info.sh
This script collects static hardware information and inserts it into the host_info table in the database.

bash

Usage
./scripts/host_info.sh
host_usage.sh
This script collects dynamic usage data (e.g., memory, CPU) and inserts it into the host_usage table in the database.

bash

Usage
./scripts/host_usage.sh
Crontab
Crontab is used to schedule the host_usage.sh script to run at regular intervals.

bash

Open crontab configuration
crontab -e

Add the following line to run the script every minute
* * * * * /path/to/scripts/host_usage.sh > /tmp/host_usage.log 2>&1

queries.sql
This file contains SQL queries to analyze the data. For example, to show average memory usage as a percentage over a 1-minute interval for each node:

sql
SELECT
host_id,
AVG(memory_usage) AS avg_memory_usage
FROM
host_usage
WHERE
timestamp >= NOW() - INTERVAL '1 minute'
GROUP BY
host_id;

Test

Bash scripts and SQL DDL were tested on a local development environment. The scripts successfully collected and inserted data into the database, and SQL queries returned expected results. Manual tests were conducted to verify data accuracy and script functionality.

Deployment

The app was deployed using Git for version control, Docker for container management, and crontab for scheduling tasks. The repository was pushed to GitHub, and Docker containers were managed using the psql_docker.sh script.

Improvements

Handle hardware updates: Automatically update the host_info table when hardware changes.
Data visualization: Integrate with tools like Grafana for real-time data visualization.
Alerting system: Implement an alerting mechanism to notify administrators of critical issues (e.g., high CPU usage).
