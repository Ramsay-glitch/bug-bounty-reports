# Incorrect Oracle Equality Assertion Causes Permanent Configuration Lock

## Summary

The functions `handle_set_oracle()` and `handle_set_oracle2()` validate oracle updates incorrectly by comparing stored values instead of validating incoming parameters.

This allows oracle addresses to become equal, after which future updates permanently revert.

---

## Vulnerable Location

Functions:

handle_set_oracle()

handle_set_oracle2()

---

## Root Cause

The assertion checks:

assert_ne!(global_config.oracle, global_config.oracle2)

Instead of validating the new oracle parameter.

This allows both oracle values to become identical.

---

## Proof of Concept (POC)

Step 1:

Set oracle normally

Step 2:

Set oracle2 equal to oracle

Step 3:

Future updates revert permanently

Configuration becomes locked.

---

## Impact

Permanent configuration lock preventing oracle updates.

Severity: Medium

---

## Recommended Fix

Validate new oracle input instead of stored oracle values.

Example:

assert_ne!(incoming_oracle, global_config.oracle2)

---

## Disclosure Timeline

Responsible disclosure submitted.

---

## Status

Invalid / Duplicate
