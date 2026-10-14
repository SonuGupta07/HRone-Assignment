# Implementation Decisions — Employee Attendance & Analytics API

## 1. Indexes

I used a unique index on `employees.emp_code` to prevent duplicate employees and a unique compound index on `attendance_logs.emp_code` and `date` to prevent duplicate daily attendance records. I added indexes for attendance date and status filters, employee department and joining-date filters, and employee-based attendance lookups. I did not add a separate status-only index because the compound status/date index supports the relevant filtered queries. I checked query plans using the explain endpoint and a dataset containing 100,000 attendance records.

## 2. Punch-in race condition

Checking for an existing record before insertion cannot prevent two simultaneous requests from passing the check. The unique attendance index is the final safeguard. If another request inserts the same employee-date record first, MongoDB rejects the duplicate, and the API returns a conflict response.

## 3. Leaderboard ties

I used competition ranking so employees with equal late-minute totals receive the same rank. The next rank skips the tied positions, such as `1, 2, 2, 4`. The limit is applied after ranking, so tied employees can make the response contain more rows than the requested limit.

## 4. Departments with no attendance

The department summary starts with eligible employees and looks up their attendance. This preserves employees with zero attendance records while excluding employees who joined after the reporting period.

## 5. Scaling

If the dataset became 100 times larger, I would first profile the slow aggregation pipelines and query plans. I would then consider maintaining pre-aggregated daily or monthly attendance statistics to reduce repeated calculations over historical records, while preserving the correctness of the detailed attendance data.

