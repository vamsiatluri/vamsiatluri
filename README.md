## Vamsi Atluri

Twenty-four years in IT and operations. Most of it spent on the other end of a pager from
somebody's green deployment.

The failure I keep meeting is always the same shape:

> **Everything reported success. The system was down anyway.**

A load balancer says healthy and serves nothing. A deploy goes green and applies nothing.
An autoscaling group runs fine for months, then the first instance refresh fails because
the AMI its launch template points at was deregistered and nobody was told. A compliance
check comes back clean because the query behind it errored, and an error that returns
nothing looks exactly like nothing to find.

None of those are exotic. They're Tuesday. And they all share one root cause: **the thing
doing the checking asked the control plane whether the control plane had succeeded.**

So I build tools that go and look instead — that probe the outcome a change was supposed to
produce, from outside the system that made it, and that are allowed to answer **"I could
not tell."** That third answer is treated as a failure, not a warning. A check that can't
distinguish *working* from *couldn't reach it* doesn't give you confidence, it manufactures
it, and that's worse than having no check at all.

### Public work

**[aws-baseline-audit](https://github.com/vamsiatluri/aws-baseline-audit)** — read-only AWS
posture check in a single file. Public SSH/RDP, unhardened load balancers, missing WAF,
TLS 1.0/1.1. It separates findings that are actually *reachable* from findings that merely
match a pattern, because severity should describe your exposure, not your grep.

**[It Ran. Did It Work?](https://vatluri-tools.github.io)** — measured findings about what
cloud platforms actually do, as opposed to what they document.

### How I work

I don't record a guard as working until it has failed on a bad input *and* a control has
passed on a good one. Most of what I find in my own work gets found that way — including
the defects inside the same day's fixes. An assertion that has only ever seen valid input
has told you nothing, and it will keep telling you nothing right up until the day it matters.

<sub>Las Vegas · infrastructure and operations · the gap between "it ran" and "it worked"</sub>
