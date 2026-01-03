# The Essence of TDD

This repository documents an observation.

Sometimes tests pass, coverage improves, refactoring feels safe —
and yet, the design stops changing.

This is not a failure of testing tools.
It is often a failure to notice which assumptions quietly became settled.

This document attempts to make that moment visible.

---

## Why this exists

Test-Driven Development is often described as a way to *discover* design.

In practice, many teams reach a point where tests mainly confirm
what has already been decided, rather than revealing anything new.

When that happens, green tests stop asking questions.
They begin to preserve assumptions instead of challenging them.

This repository exists to explore:
- when that shift occurs
- why it is hard to notice
- how some teams detect it earlier

---

## What this is

This is not a framework or a methodology.

It is a collection of observations about:
- design discovery
- test scope and boundaries
- invariants and assumptions
- the moment design decisions become hard to reverse

The focus is not on *how to write tests*,
but on *what tests actually settle*.

---

## What this is NOT

This is NOT:
- a replacement for unit testing
- an argument against incremental development
- a checklist or a process guide
- a claim that “most people are doing TDD wrong”

If your current practice works, this repository does not argue against it.

---

## Read the document

→ **Read the full text**
- [GitHub](ESSENCE_OF_TDD.md)
- [Leanpub (EPUB/PDF)](https://leanpub.com/theessenceoftdd)


---

## Discussion

Discussion, counterexamples, and failure cases are welcome.

This repository uses **GitHub Discussions** as its primary interaction surface.
The goal is to surface patterns and shared experiences,
not to defend a single position.

I may not actively respond to every thread.

---

## What this repository is not intended for

- This is not a library or framework.
- There are no releases, packages, or deployable artifacts.
- Nothing here is meant to be imported or executed.

This repository exists to sharpen how we *think* about TDD,
not to provide code to run.

---

## Security

This repository does not ship executable code, libraries, or services.
As such, there is no security advisory process or vulnerability reporting channel.

---

## License & Attribution

License: **Creative Commons Attribution–NoDerivatives 4.0 International (CC BY-ND 4.0)**

Attribution is required.  
Redistribution is permitted.  
Modified versions may not be distributed without permission.
