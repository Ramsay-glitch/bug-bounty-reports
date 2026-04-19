# Delimiter Injection in JsonFormatter.formatJsonPathRequest()

## Summary

The function `formatJsonPathRequest(string path, string content)` concatenates user-controlled inputs using parentheses delimiters without validation or escaping.

This allows delimiter injection that may manipulate structured parsing behavior.

---

## Vulnerable Location

Library: JsonFormatter.sol

Function:

formatJsonPathRequest(string path, string content)

---

## Root Cause

The function concatenates attacker-controlled inputs directly:

(path)(content)

Without escaping parentheses characters, crafted inputs can alter delimiter structure.

---

## Proof of Concept (POC)

Example input:

path = "issuer)(fake"
content = "payload"

Output:

(issuer)(fake)(payload)

This modifies expected delimiter structure.

---

## Impact

Delimiter injection may cause parsing inconsistencies in verification pipelines relying on delimiter integrity.

Severity: Informational / Low

---

## Recommended Fix

Escape parentheses before concatenation OR replace delimiter-based formatting with structured JSON encoding.

---

## Disclosure Timeline

Report submitted via responsible disclosure.

---

## Status

Duplicate / Informational
