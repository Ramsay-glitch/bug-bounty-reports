# EndpointV2 send() Refund Logic Uses Contract Balance Instead of Caller-Supplied Fee

## Summary

The EndpointV2::send() implementation calculates the supplied messaging fee using the contract’s entire token balance rather than the amount provided by the caller for the transaction.

This allows unrelated tokens stored inside the contract to be refunded to arbitrary callers.

Severity (final): Low
Original assessment: Medium (downgraded)

---

## Vulnerable Location

File:

endpoint_v2.rs

Function:

send()

---

## Root Cause

Refund logic reads:

native_token_client.balance(&this_contract)

instead of tracking the caller’s supplied payment.

As a result:

refund_amount = contract_balance − required_fee

instead of:

refund_amount = sender_supplied_amount − required_fee

---

## Proof of Concept

Attack flow:

1. Victim tokens exist inside Endpoint contract
2. Attacker calls send()
3. Attacker supplies only required messaging fee
4. refund_address set to attacker
5. Contract refunds full stored balance

Observed result:

attacker_final_balance == 1000
endpoint_final_balance == 0

Tokens successfully swept.

---

## Impact

Allows permissionless withdrawal of unrelated tokens already stored inside the contract balance.

Attack characteristics:

• No privileged role required  
• Single transaction exploit  
• Deterministic execution path  
• Direct asset extraction possible  

---

## Recommended Fix

Track sender-supplied fee amount per transaction.

Replace:

contract_balance − required_fee

With:

sender_supplied_amount − required_fee

---

## Disclosure Timeline

Reported through responsible disclosure process.

Severity downgraded from Medium to Low during review.

---

## Status

Valid vulnerability  
Severity downgraded
