# Case-variant route bypass exposes a protected synthetic record

## Asset

`http://127.0.0.1:18763`

## Weakness

CWE-287: Improper Authentication

## Severity

Medium

## Summary

The lowercase admin route requires authentication. The uppercase case variant
returns the same protected synthetic record without authentication. No account,
credential, victim, state change, or external host is required.

## Fastest proof

```bash
curl -sS -i --max-time 5 http://127.0.0.1:18763/API/admin/record
```

Expected: HTTP 200 containing `fixture-owned` and `protected-record`.

## Negative control

```bash
curl -sS -i --max-time 5 http://127.0.0.1:18763/api/admin/record
```

Expected: HTTP 401 containing `authentication required`.

## Impact

An unauthenticated caller crosses the route authentication boundary and reads a
protected record.

## Recommended fix

Canonicalize route casing before authorization and enforce authentication in the
handler.

