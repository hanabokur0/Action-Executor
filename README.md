# ⚡ GO/NO Action Executor

A minimal, standalone execution layer.  
**Input comes in. GO or NO goes out. That's it.**

---

## What this does

```
input → validate → resolve → execute → GO ✅
                                     → NO ❌ (return to human)
```

No ambiguity. No silent failures.  
Every execution either succeeds or returns to a human with a reason.

---

## Quick Start

```bash
python run.py
```

Output:
```
--- [1] ---
[OK] validation passed
[EXEC] copy → ['C001', 'C002', 'C003']
[GO] executed successfully

--- [2] ---
[OK] validation passed
[EXEC] notify → hello world
[GO] executed successfully

--- [3] ---
[NO] validation failed → return to human: input_state required

--- [4] ---
[OK] validation passed
[NO] execution failed → return to human: target_not_found

--- [5] ---
[OK] validation passed
[NO] execution failed → return to human: unknown_action
```

---

## Structure

```
go-no-executor/
  execution/
    action_executor.py   ← core logic
  run.py                 ← entry point + demo
  README.md
```

---

## Customize

**Add your own targets** in `resolve_target()`:
```python
mapping = {
    "customer_id_list": ["C001", "C002", "C003"],  # replace with CSV/API/DB
    "note_text": "hello world",
}
```

**Add your own actions** in `execute()`:
```python
if action == "send_email":
    # your logic here
    return {"status": "ok"}
```

---

## NO conditions (built-in)

| Condition | Reason returned |
|-----------|----------------|
| Target not found | `target_not_found` |
| Data too long (>100 chars) | `data_too_long` |
| Unknown action | `unknown_action` |
| Missing input fields | `input_state / action / target required` |

---

## Use with Protocol Engine

This repo is the execution layer of the [LoPAS Protocol Engine](https://github.com/hanabokur0/Protocol-Engine-LoPAS-Runtime-Core-).

Use standalone, or plug in as Layer 3:

```
RNC (validate) → Protocol Engine (classify) → GO/NO Executor (execute)
```

---

## Design principle

> Execution must be explicit.  
> Unknown is not failure — it's a signal to return to human.

---

## License

MIT
