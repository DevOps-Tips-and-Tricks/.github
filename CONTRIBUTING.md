# Contributing

The fastest way to understand what we want is to read one merged entry. The
rules below are short; the example is the real specification.

## A worked example

Here is a complete, mergeable `war-stories` entry. Copy its shape.

````markdown
---
id: "0042"
title: Ingress traffic blackholed after node drain
stack: [kubernetes, cilium, metallb]
severity: sev2
detection: alerting
time_to_detect: 3m
time_to_resolve: 41m
root_cause_category: configuration
blast_radius: cluster
contributed_by: "@your-github-handle"
---

## Symptom

External traffic to every ingress host returned connection timeouts. Pods were
Running and Ready. In-cluster service-to-service traffic was unaffected.

## Timeline

- `14:02` — Node `worker-04` cordoned and drained for a kernel upgrade.
- `14:05` — Blackbox probes for all ingress hosts started failing.
- `14:09` — Confirmed backend pods healthy; suspicion moved to the LB layer.
- `14:31` — Found the LoadBalancer address still advertised from the drained node.
- `14:43` — Traffic restored after the address pool was reconciled.

## What we thought it was

Our first hypothesis was DNS, because the failure was total and instantaneous —
that pattern usually means resolution, not routing. We spent twelve minutes
there and found nothing. The second hypothesis was that the ingress controller
had lost leader election. Both were wrong, and both were plausible enough that
we would guess them again.

The signal we should have read first: in-cluster traffic was fine. That excludes
DNS and the controller, and points at everything between the client and the node.

## Actual root cause

The load balancer advertised the service address from a fixed node list rather
than from the set of nodes currently passing health checks. Draining removed the
pods but not the advertisement, so the address kept resolving to a node with no
path to any backend.

## Fix

Reconciled the address pool so advertisement follows endpoint health rather than
a static node list.

## What would have prevented it

A pre-drain check asserting the node is not the active advertiser for any
LoadBalancer address. This is now a step in the drain runbook.

## Generalizable lesson

When a failure is total and instant, the instinct is to blame name resolution.
Ask instead which traffic *still works* — the intersection of what is broken and
what is not usually names the layer before any log does.
````

## Rules

1. **Anonymise before you write, not after.** No employer names, customer names,
   internal hostnames, or routable IPs. Use `192.0.2.0/24`, `198.51.100.0/24`,
   `203.0.113.0/24`, or RFC1918 addresses. CI rejects anything else.
2. **Every section is required**, including *What we thought it was*. An entry
   that jumps straight to the correct answer is not useful — the wrong turns are
   the content.
3. **Blame systems, not people.** "The runbook had no pre-drain check" is in
   scope. "The on-call engineer forgot" is not.
4. **Front matter must validate.** Run `python scripts/validate.py` before you
   push; CI runs the same script.
5. **One entry per pull request.** Small PRs get merged the same week.

## Mechanics

```bash
git clone https://github.com/DevOps-Tips-and-Tricks/war-stories
cd war-stories
pip install -r requirements.txt

cp stories/_TEMPLATE.md stories/0043-your-short-title.md
# edit it, then:
python scripts/validate.py
python scripts/build_index.py
```

The filename must be the zero-padded `id`, a hyphen, then the slugified `title`.
The validator prints the exact filename it expects if you get it wrong.

## Not sure your incident is interesting enough?

It is. The bar is not severity — a 20-minute sev3 with a genuinely surprising
cause is worth more than a six-hour outage caused by a full disk. Open a
discussion and describe it in two sentences; we will tell you whether it fits
and help you shape it.

## Review

Expect a first response within a week. Reviews check three things: is it
anonymised, is the *what we thought it was* section honest, and does the lesson
generalise beyond your specific stack. Style, grammar and formatting are the
maintainer's job, not yours — do not let them stop you submitting.
