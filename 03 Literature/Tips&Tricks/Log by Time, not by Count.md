---
tags:
  - tipsntricks
source: https://johnscolaro.xyz/blog/log-by-time-not-by-count
links:
  - "[[logging]]"
---
# Log by Time, not by Count

> [!abstract] Summary
> When logging we should try to log across time not volume of messages. A time-based system is reliable regardless of load size. It allows for consistent behaviour across environments and reduces ambiguity of what the right "size" (# messages) should be.
## Why

1. **Log rate.** It is important to log the right amount so that you don't pollute your systems with too many logs, making them hard to parse. Or too few where they cannot tell you anything.
2. **Consistent throughput.** Logging every X seconds allows for a consistent stream whether you're handling 10,000 requests in prod or 5 locally while testing.
3. **What is X.** When trying to log over X messages, it is difficult to impossible to know what X should be while keeping a good log rate.
## How

```
last_time_logged = time.time()
num_events_processed_since_last_log = 0

while True:
    event = read_event_from_queue()
    process_event(event)
    num_events_processed_since_last_log += 1

    current_time = time.time()
    if current_time - last_time_logged >= 1.0:
        module_logger.info(f"Processed {num_events_processed_since_last_log} events.")
        num_events_processed_since_last_log = 0
        last_time_logged = current_time
```
