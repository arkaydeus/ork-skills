---
name: perf
description: 'Use when the user reports slowness, high memory, cost or throughput problems, or asks to speed something up. Measure a baseline, find the real bottleneck, fix it, re-measure, and report the numbers.'
---

# Perf

A performance change without numbers is a refactor you have not justified. Measure, find, change, measure.

## 1. Name the metric

Agree what "faster" means: p95 latency, time to first byte, heap peak, allocation count, queries per request, CPU seconds, pounds on the bill. Pick the unit and the workload (which endpoint, which fixture, which machine class). If the user has not said, propose one and wait.

Look up how the repo already measures this. Reuse that harness if it exists.

## 2. Baseline

Run the measurement before you touch code. Same command you will use later. Prefer several runs over a single noisy sample. Write down:

- the command
- the environment (commit, flags, data size)
- the numbers, not a vibe

If the run cannot see the slow path (tiny fixture, cache always hot, mocked network), you do not have a baseline. Fix the harness first.

## 3. Find the bottleneck

Do not guess the hot function from the source. Use a profiler, an explain plan, a log of timings, or an allocation sample, whichever matches the metric. Read the output.

The bottleneck is the frame or query that accounts for the time or memory you named. If the profile is flat, say so: a micro-optimisation will not move the number, and you should stop rather than shuffle code.

Attack the premise if a previous speed-up failed. Two patches that shared "it must be N+1" and did nothing mean you have not seen N+1.

## 4. One change

Change the thing the profile pointed at. Keep the behaviour the same unless the user agreed a trade-off (weaker consistency, bigger cache, less work on the cold path). Record that trade-off in `docs/decisions/` if reversing it would be expensive.

Do not sprinkle caches "in case". Do not rewrite a module because it looks busy.

## 5. Measure again

The same command, the same workload, the same environment story. Report before and after in one table:

| Metric | Before | After | Command |
| --- | --- | --- | --- |
| <name, unit> | <n> | <n> | `<cmd>` |

If the number did not move enough to matter, revert. A slower result is a failed experiment, not a moral victory for cleaner code.

## 6. Reply

- the metric and workload
- where the time or memory actually went
- what you changed
- the table
- what you did not change, and why

Follow `verify` for correctness. A faster wrong answer is a bug.
