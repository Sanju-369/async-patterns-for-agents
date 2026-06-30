# Async Patterns for Agen Harness

Six async/await patterns every AI agent needs in production — 
built and timed to prove they actually work, not just explained in theory.

## Why This Exists

AI agents call multiple tools, models, and APIs that all run at 
different speeds. Get the async patterns wrong and your agent is 
either too slow (everything runs sequentially) or too fragile 
(one failure crashes everything).

This project is six small, focused experiments — each one isolates 
a single pattern and proves it works with real timing output.

## The 6 Patterns

### 1. Concurrent Calls — `asyncio.gather()`
Three tools, each taking 1 second, run at the same time instead 
of one after another.

**Result:** 1.00s instead of 3.00s

### 2. Partial Failure Handling — `return_exceptions=True`
One tool fails. The other two still return successfully instead 
of crashing the whole batch.

**Result:** 2 successes, 1 caught failure, zero crash

### 3. Timeout Handling — `asyncio.wait_for()`
A slow call (5s) gets cut off after 2s with a clean fallback 
instead of hanging indefinitely.

**Result:** TimeoutError caught at 2.00s, fallback triggered

### 4. Rate Limiting — `asyncio.Semaphore()`
4 requests, but only 2 allowed to run concurrently — protects 
a system with limited capacity.

**Result:** 2.00s total — proves only 2 ran at once

### 5. Streaming — Async Generator
Output appears word by word instead of waiting for the full 
response, same pattern used by ChatGPT and Claude.

**Result:** Tokens streamed individually with delay between each

### 6. Fire-and-Forget — `asyncio.create_task()`
Background logging happens without blocking the main response.

**Result:** Main work completes before background task finishes

## Why This Matters

Each pattern maps to a real block in the agentic system I'm building:

| Pattern | Maps To |
|---|---|
| `gather()` | Calling multiple tools at once |
| `return_exceptions=True` | One tool failing shouldn't crash the agent |
| `wait_for()` | Falling back when a model is too slow |
| `Semaphore()` | Protecting a local model server from overload |
| Async generators | Streaming responses that feel instant |
| `create_task()` | Logging without blocking the response |

## Run It Yourself

```bash
python async_patterns.py
```

Requires Python 3.7+. No external dependencies — pure asyncio 
from the standard library.

## What's Next

These patterns are the foundation for an agent harness that 
actually reasons and acts — calling tools concurrently, handling 
failures gracefully, and streaming responses in real time.
