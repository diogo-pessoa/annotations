---
title: "Avoiding Regex pattern, instead use value matching"
date: 2026-01-27
featured: false
draft: false
toc: true
tags:
  - python
  - sysAdmin
series:
  - diary-of-a-flawed-pythonista
---

Option to avoid crazy Regex to “redact by *value* matching” (e.g., detect emails/IPs/tokens anywhere). 
Storing simple examples to save time in troubleshooting.

practical exercises with value matching in python: [my GitHub](https://github.com/diogo-pessoa/coding-exercises/tree/main/LogParsingSnippets/PatternMatchByValues)

## Heuristic validators (no regex)

### Email 

* One `@`
* Non-empty local and domain parts
* Domain contains at least one `.`
* No spaces
* Optional: basic allowed chars

```python
def looks_like_email(s: str) -> bool:
    if " " in s or s.count("@") != 1:
        return False
    local, domain = s.split("@")
    if not local or not domain or "." not in domain:
        return False
    if domain.startswith(".") or domain.endswith("."):
        return False
    return True
```

### IPv4 (use stdlib, no regex)

Python has `ipaddress`:

```python
import ipaddress


def looks_like_ipv4(s: str) -> bool:
    try:
        return isinstance(ipaddress.ip_address(s), ipaddress.IPv4Address)
    except ValueError:
        return False
```

### Account ID (12-digit AWS style)

```python
def looks_like_aws_account_id(s: str) -> bool:
    return len(s) == 12 and s.isdigit()
```

### Token-ish strings (avoid over-redacting)

Token detection is the hardest without false positives. A good approach is:

* only scan keys that are likely to contain tokens (`authorization`, `auth`, `bearer`, `token`, `session`, `cookie`)
* or require multiple signals: long length + high entropy-ish + mostly URL-safe chars.

Example heuristic:

```python
import string

_URLSAFE = set(string.ascii_letters + string.digits + "-_.~+/=")


def looks_like_token(s: str) -> bool:
    if len(s) < 20:
        return False
    if any(ch.isspace() for ch in s):
        return False
    # Require mostly URL-safe chars
    ok = sum(ch in _URLSAFE for ch in s)
    if ok / len(s) < 0.95:
        return False
    # Avoid redacting normal sentences
    if s.count(".") >= 2:  # could be JWT-ish
        return True
    if s.startswith(("Bearer ", "bearer ")):
        return True
    return True
```

## 3) Use stdlib parsers for known formats

Avoid regex by parsing:

* URLs → `urllib.parse.urlparse`
* query strings → `urllib.parse.parse_qs`
* headers → split on `:` and `;`
* JWT → split on `.` and base64url-decode segments (careful not to *log* decoded payloads)

Example: redact `token` in query params:

```python
from urllib.parse import urlsplit, parse_qsl, urlencode, urlunsplit


def redact_url_query(url: str, redaction_counts: dict) -> str:
    parts = urlsplit(url)
    qs = parse_qsl(parts.query, keep_blank_values=True)
    new_qs = []
    for k, v in qs:
        if k.lower() in {"token", "access_token", "id_token"}:
            redaction_counts[k.lower()] = redaction_counts.get(k.lower(), 0) + 1
            new_qs.append((k, "[REDACTED_TOKEN]"))
        else:
            new_qs.append((k, v))
    return urlunsplit((parts.scheme, parts.netloc, parts.path, urlencode(new_qs), parts.fragment))
```


## An alternative is to user Structured matching when you know the schema, Redact by key
Best-case: the event already has typed fields (e.g., `client_ip`, `user_email`). No Need to pattern match
redact by key, rough hands-on snippet on [my GitHub](https://github.com/diogo-pessoa/coding-exercises/blob/main/LogParsingSnippets/LogRedactingByKey/logRedactorMain.py).
