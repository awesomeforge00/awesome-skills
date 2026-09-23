# Concurrency & Async (Python)

## 1. `threading` — protecting shared state

```python
# BAD: two threads can both read the old value before either writes the new one (race condition)
counter = 0

def increment():
    global counter
    counter += 1  # not atomic: read, add, write

# GOOD: a lock makes the read-modify-write atomic
import threading

counter = 0
_lock = threading.Lock()

def increment():
    global counter
    with _lock:
        counter += 1
```

Use `threading` for I/O-bound work (waiting on files/network) that benefits from running
concurrently. It does not speed up CPU-bound work in standard CPython, due to the GIL.

## 2. `asyncio` — many I/O-bound tasks without threads

```python
# BAD: sequential requests, each waits for the previous to finish
import requests

def fetch_all(urls: list[str]) -> list[str]:
    return [requests.get(url).text for url in urls]

# GOOD: run requests concurrently
import asyncio
import aiohttp


async def fetch_all(urls: list[str]) -> list[str]:
    async with aiohttp.ClientSession() as session:
        tasks = [_fetch_one(session, url) for url in urls]
        return await asyncio.gather(*tasks)


async def _fetch_one(session: aiohttp.ClientSession, url: str) -> str:
    async with session.get(url) as response:
        return await response.text()
```

## 3. Don't mix blocking calls into async code

```python
# BAD: time.sleep blocks the entire event loop, freezing every other task
async def wait_and_process():
    time.sleep(2)  # blocks everything
    ...

# GOOD: use the async equivalent
async def wait_and_process():
    await asyncio.sleep(2)  # lets other tasks run meanwhile
    ...
```

Same idea applies to blocking I/O libraries (e.g. `requests`) inside `async def` functions —
use their async counterparts (`aiohttp`, `asyncpg`, etc.) or run them in a thread pool via
`asyncio.to_thread(...)`.

## When to use which

| Situation | Use |
| --- | --- |
| A few background tasks, simple shared state | `threading` + `Lock` |
| Many concurrent I/O operations (HTTP calls, DB queries) | `asyncio` |
| CPU-heavy work (parsing, image processing) | `multiprocessing` (sidesteps the GIL) |
| Simple "do this later" without true concurrency needs | Sequential code — don't add concurrency you don't need |
