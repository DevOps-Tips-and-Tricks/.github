<h1 align="center">DevOps Tips and Tricks</h1>

<p align="center"><em>The thinking behind the YAML.</em></p>

<p align="center">
  <a href="https://www.youtube.com/@DevOpsTipsAndTricks">YouTube</a>
</p>

---

Most DevOps content stops at *how to install it*. This organisation is the layer
above that: **why the design was chosen, what broke at 2am, and what we would do
differently now.**

Everything here comes out of running self-hosted Kubernetes in production, and
everything here is open to contribution. If you have carried a pager, you have
something to add.

## Repositories

| Repository | What it is |
|---|---|
| [`war-stories`](https://github.com/DevOps-Tips-and-Tricks/war-stories) | Anonymised incident postmortems in a fixed schema — including the wrong hypotheses, not just the root cause |
| [`debug-playbooks`](https://github.com/DevOps-Tips-and-Tricks/debug-playbooks) | Diagnostic decision trees: symptom → hypotheses → commands → root cause |
| [`decision-records`](https://github.com/DevOps-Tips-and-Tricks/decision-records) | ADRs for infrastructure choices, with consequences and how they aged |
| [`production-readiness`](https://github.com/DevOps-Tips-and-Tricks/production-readiness) | The checklist a service passes before it ships, with the reasoning behind every line |
| [`broken-clusters`](https://github.com/DevOps-Tips-and-Tricks/broken-clusters) | kind/k3d environments deliberately broken in one specific way. Fix them |
| [`episodes`](https://github.com/DevOps-Tips-and-Tricks/episodes) | Manifests and labs from each video, one folder per episode |

## Start here

New to the org? These issues are scoped small and reviewed quickly:

- [`good first issue`](https://github.com/search?q=org%3ADevOps-Tips-and-Tricks+label%3A%22good+first+issue%22+state%3Aopen&type=issues) across all repositories
- [How to contribute](https://github.com/DevOps-Tips-and-Tricks/.github/blob/main/CONTRIBUTING.md) — read the worked example, skip the rules

## Why contribute

Every merged entry is credited by GitHub handle in the file itself, listed in
the repository index, and named in the description of any video that draws on
it. That is the whole economy here: your name on work that other engineers
actually read.

## What we will not merge

- Anything containing a real employer name, customer name, internal hostname, or
  routable IP address
- Vendor marketing, tool announcements, or affiliate links
- Postmortems that blame a person rather than a system

## Licence

Code is Apache-2.0. Written content is CC BY-SA 4.0. By contributing you agree
your work is published under those terms.
