## Vamsi Atluri

I build infrastructure tooling around one question:

> **The change was applied. Is the outcome actually true?**

Most tooling answers a different question — did the API accept the call, does the state
match the file, does the config pass the linter. Every one of those can come back clean
while the thing you actually care about is broken:

- A load balancer reports healthy and serves nothing.
- A deploy goes green and applies nothing.
- A failed `aws` query inside a process substitution returns an empty result, which reads
  as "no findings", which reads as clean. `set -euo pipefail` does not catch it.
- An AMI is deregistered; the API returns `{"Images": []}` and exit `0`. Not an error —
  just an empty answer to "does this exist", indistinguishable from a successful call.

So the tools I write probe the invariant from outside the control plane, and they are
allowed to return a third answer: **"I could not tell."** That is a failure, not a warning.
A check that can't distinguish *clean* from *couldn't tell* is worse than no check, because
it manufactures confidence.

### Public work

**[aws-baseline-audit](https://github.com/vamsiatluri/aws-baseline-audit)** — read-only AWS
posture check in a single file. Public SSH/RDP, unhardened load balancers, missing WAF,
TLS 1.0/1.1 — and it separates findings that are actually *reachable* from findings that
merely match a pattern, because severity should reflect exploitability, not grep.

### How I work

Assertions get measured against a live system with a control that can distinguish a real
result from a broken instrument. If the control fails the same way the test does, the test
proved nothing. Most of the bugs I find in my own work are found that way, including the
ones inside the same day's fixes.

<sub>Las Vegas · infrastructure, AWS, and the gap between "it ran" and "it worked"</sub>
