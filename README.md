# Engineering-Standards...

26 rules for AI systems building production software.

Extracted from a real conversation thread where an AI built something worth learning from. Extends Karpathy's CLAUDE.md with a domain integrity layer that the existing templates don't cover.

---

## Background

In early 2026, Andrej Karpathy complained publicly about how Claude writes code. Forrest Chang packaged the complaints into 4 behavioral rules in a CLAUDE.md file. It hit 120,000 GitHub stars.

The 4 rules are foundational. But they were written for autocomplete-style coding tasks.

I was using AI to architect a production system — dual-entity accounting, audit trails, resource guards, exact financial figures, void logic. The Karpathy rules didn't cover that territory. So I paid attention to what I was actually enforcing in my conversation threads, and named those behaviors as rules.

The result is this file.

---

## What's Different

The existing templates are **process rules** — they govern how the AI works through a task.

This adds a **domain integrity layer** — rules that govern whether what the AI builds can be trusted in production.

| Layer | What It Closes |
|---|---|
| Foundation (Karpathy's 4) | Silent assumptions, over-engineering, orthogonal damage |
| Agent layer (8-rule extension) | Token overruns, pipeline drift, weak tests, silent failures |
| **Integrity layer (this file)** | Data store verification, destructive operation guards, exact numbers, audit trails, boundary validation, over-consumption |

---

## How To Use It

Drop `engr-standards.md` in your repo root. Rename it `CLAUDE.md` if you want Claude Code to pick it up automatically, or keep it as a named standard and reference it explicitly.

Add your project-specific rules — stack constraints, test commands, known failure patterns — at the bottom. Stay under 200 lines total. Compliance drops sharply past that threshold.

Cut rules that don't map to failure modes you've actually encountered. A 15-rule file tuned to your real problems beats a 26-rule one where 11 rules never fire.

---

## The Non-Negotiables

Five rules appear in every serious template because removing any one of them reliably produces the same class of failure:

- Reason before you act
- Verify, never assume
- Touch only what you must
- Fail visibly, not silently
- The repository must always build

Don't cut these regardless of your domain.

---

## Installation

```bash
curl -O https://raw.githubusercontent.com/vehutech/engineering-standards/main/engr-standards.md
```

Or clone and copy:

```bash
git clone https://github.com/vehutech/engineering-standards.git
cp engineering-standards/engr-standards.md ./CLAUDE.md
```

---

## License

MIT. Use it, adapt it, share it.

---

*Built by [@Vehutech](https://x.com/Vehutech)*
