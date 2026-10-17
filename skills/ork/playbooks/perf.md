# Perf

Improve a measured number. No baseline, no change.

1. Invoke `perf`. Agree the metric, measure a baseline with a named command, find the actual bottleneck, change one thing, and measure again with the same method.
2. Invoke `verify`. Include the before and after numbers and the commands that produced them. If the number did not move enough to matter, revert and say so.
3. Invoke `review` with the measurements in scope. Reject unverified "this should be faster" patches.
4. If the user wants the change proposed for merge, invoke `pr` and put the numbers in the body. Otherwise `skip: no pull request requested`.
