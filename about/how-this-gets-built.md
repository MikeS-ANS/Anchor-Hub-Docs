# How Anchor Hub Gets Built

Anchor Hub — the platform our team uses every day for billing, contracts, and vendor integrations — is built and maintained by a dedicated internal engineering division at Anchor Network Solutions. Like any small software team, we have defined roles, a documented workflow, and a review process before anything ships.

What makes this team unusual is how three of the seats are staffed: by specialized AI systems (Claude, from Anthropic), each doing a distinct, focused job. The roles, the accountability, and the review process are exactly what you'd expect from any professional software team — the tools doing the work are just different.

---

## The Team

| Seat | Role |
|---|---|
| **Mike Stewart** | Director of Product Engineering & Security Reviewer |
| **Requirements & Architecture Analyst** (Claude Chat) | Turns a rough idea into a clear, written specification |
| **UX/UI Designer** (Claude Design) | Designs screens and interactions, keeps the app's visual language consistent |
| **Software Engineer, QA & Technical Writer** (Claude Code) | Implements each feature, tests it, and maintains this help center |

**Mike Stewart** owns the roadmap and prioritization, makes the judgment calls that requirements can't answer on their own, and reviews every feature for security and data governance before it ships. As ANS's CISO — the role behind our CIS Level 3 and GITA Security Trustmark achievements — Mike applies the same security discipline to Anchor Hub as to the rest of ANS's infrastructure.

**The Requirements & Architecture Analyst** works with Mike to turn a rough idea into a clear, written specification — the same function a business analyst or systems architect plays on any dev team. Asks clarifying questions, documents decisions, and produces the spec that engineering builds from.

**The UX/UI Designer** designs the screens and interactions for new features, reviews existing screens for usability issues, and keeps the app's visual language consistent as it grows.

**The Software Engineer, QA & Technical Writer** implements each feature from its written spec, tests it, reviews its own code for issues, and flags risks or recommendations in real time. Also maintains this help center as features change.

---

## How a Feature Gets Built

1. **Idea & requirements** — Mike and the Analyst work through what's needed and why, until the requirements are clear.
2. **Design** — For features with a user-facing component, the Designer mocks up the screens and checks them against our existing patterns.
3. **Specification** — A written spec is produced covering scope, constraints, and what's explicitly not included.
4. **Security review** — Mike reviews the spec for data handling, access, and risk, before a line of code is written.
5. **Build** — The Engineer implements the spec, one piece at a time, testing and reviewing along the way.
6. **Sign-off** — Mike reviews and approves before the feature reaches the team. Anything touching billing or client data always requires a human confirmation step — no exceptions, no automation shortcuts.

---

## Why We Work This Way

This structure lets us build and ship internal tools at a pace and quality bar that would normally require a multi-person engineering team, while keeping a single accountable owner (Mike) in the loop on every decision, every security review, and every production change.

It is not "AI running unsupervised." It's a disciplined, spec-first engineering process where AI systems fill defined roles, and a human makes every call that matters.

If you have questions about how this works, what data is involved, or how a specific feature was built, ask Mike directly.
