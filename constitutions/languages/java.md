---
id: language.java
title: Java
category: language
version: 1.0.0
status: active
owners:
  - java-platform
applies_to:
  - java
depends_on:
  - universal.code-quality
  - universal.testing
  - universal.dependency-management
---

# Java

## Purpose

Provide Java-specific engineering guidance.

## Mandatory requirements

- Java projects MUST define the supported Java runtime version.
- Builds MUST use a reproducible tool such as Maven or Gradle with committed configuration.
- Public APIs MUST avoid exposing mutable internals.
- Exceptions MUST carry actionable context without leaking sensitive data.
- Dependency scopes MUST be intentional.
- JDBC code MUST bind runtime values with prepared statements or equivalent safe APIs.
- Dynamic SQL identifiers such as table names, column names, or schema suffixes MUST be allowlisted or format-validated before SQL construction.
- I/O, JDBC, and other closeable resources MUST have clear ownership and deterministic cleanup.
- New static mutable factories, thread-local state, or process-wide registries MUST NOT be introduced unless required by an existing project contract and documented with lifecycle and test guidance.

## Recommended practices

- Prefer immutable value types for shared data.
- Use static analysis and formatting consistently.
- Keep concurrency primitives explicit and reviewed.
- Prefer try-with-resources for closeable resources.
- Keep thread pools, queues, and asynchronous workers named, bounded, monitored, and shut down cleanly.

## Examples

Good:

```java
String cycle = validateCycle(request.billingCycle());
String sql = "SELECT * FROM processed_bill_" + cycle + " WHERE account_no = ?";
try (PreparedStatement statement = connection.prepareStatement(sql)) {
    statement.setString(1, request.accountNo());
    return readBills(statement.executeQuery());
}
```

Bad:

```java
String sql = "SELECT * FROM processed_bill_" + request.billingCycle()
        + " WHERE account_no = '" + request.accountNo() + "'";
return statement.executeQuery(sql);
```

## Verification

Reviewers check build configuration, Java version, dependency scopes, exception handling, resource cleanup, JDBC value binding, dynamic SQL identifier validation, global mutable state, concurrency ownership, and static-analysis output.
