# Implementation Decisions

## 1. Indexes

I used a unique index on `employees.emp_code` to prevent duplicate employees and a unique compound index on `attendance_logs.emp_code` and `date` to prevent duplicate daily attendance records. I also added indexes for attendance date and status filters, employee department and joining-date filters, and employee-based attendance lookups. I did not add a separate status-only index because the compound status/date index supports the relevant filtered queries. I checked query plans using the explain endpoint and a dataset containing 100,000 attendance records.

## 2. Punch-in race condition

Checking for an existing attendance record before insertion is not enough because two simultaneous requests can both pass the check. The unique compound index on employee code and date is the final safeguard. If one request inserts first, MongoDB rejects the duplicate from the other request, and the API returns HTTP 409 Conflict.

## 3. Leaderboard ties

I used competition ranking so employees with equal late-minute totals receive the same rank. The next rank skips the tied positions, for example, `1, 2, 2, 4`. The limit is applied after ranking, so the response can contain more rows than the requested limit when employees are tied at the boundary.

## 4. Departments with no attendance

The department summary starts with employees eligible for the reporting period and looks up their attendance records. This approach preserves employees with zero attendance records while excluding employees who joined after the reporting period. It also allows the summary to include departments with eligible employees but no attendance activity.

## 5. Scaling

If the dataset became 100 times larger, I would first profile the aggregation pipelines and inspect query execution plans to identify bottlenecks. I would then consider pre-aggregating daily or monthly attendance statistics to reduce repeated calculations over historical records. Detailed attendance records would remain the source of truth so that summaries could be checked and rebuilt when necessary.
