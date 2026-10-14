# Implementation Decisions

## 1. Indexing Strategy

I chose indexes based on the queries the API performs most often, rather than indexing every field. A unique index on `employees.emp_code` prevents duplicate employee codes, while a unique compound index on `attendance_logs.emp_code` and `date` enforces one attendance record per employee per day. I also added indexes to support date and status filtering, employee lookups, and department-based queries. I avoided a separate status-only index because the compound status/date index already supports the relevant query patterns. I used the explain endpoint on a dataset of 100,000 attendance records to check whether the queries used indexes instead of collection scans.

## 2. Handling Concurrent Punch-ins

Checking for an existing attendance record before inserting it is useful for an early response, but it cannot prevent two simultaneous requests from passing the check. I therefore rely on the unique compound index as the final safeguard. If concurrent requests attempt to create the same employee-date record, MongoDB accepts only one insert. The duplicate-key error is handled by the API and returned as HTTP 409 Conflict. This keeps the integrity rule enforced by the database, not just by application code.

## 3. Leaderboard Ranking and Ties

I used competition ranking because employees with the same total late minutes should receive the same rank. For example, the ranks are `1, 2, 2, 4` when two employees tie for second place. I apply the requested limit after calculating ranks, so all employees tied at the boundary can be included even if the response contains more rows than the limit. This preserves the ranking rule instead of arbitrarily excluding an employee with the same score.

## 4. Departments With No Attendance Records

I start the department summary from employees eligible for the reporting period and then look up their attendance records. Starting from attendance alone would omit employees and departments with no logs, which could make headcounts and summaries misleading. This approach includes eligible employees even when they have no attendance records and excludes employees who joined after the reporting period. Departments with no eligible employees are not included.

## 5. Scaling the System

If the dataset grew to 100 times its current size, I would first use query execution statistics to identify which endpoints and aggregation stages had become bottlenecks. I would review index effectiveness before adding more indexes, since extra indexes also increase storage and write costs. If repeated historical analytics became expensive, I would consider maintaining daily or monthly summary records. The detailed attendance records would remain the source of truth, allowing summaries to be verified or rebuilt when needed. I would make this change based on measured performance rather than introducing extra complexity before it is necessary.
