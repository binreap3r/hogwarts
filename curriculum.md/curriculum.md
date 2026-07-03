# Systems & Security Research Apprenticeship
### v4 | Formerly "Malware Analysis & Reverse Engineering Curriculum" — renamed because that's what this actually became | Starting point: Python basics (print/variables/constants), light CS fundamentals | ~14–16 hrs/week (4 work + 9–12 off days) | Sun–Wed night SOC shifts, training hour usable for reading OR coding, whichever the day calls for | Destination: macOS/iOS security research

---

## Reality Check (read this before anything else)

You want to be world-class. World-class RE and exploit dev people have one thing in common: they spent 1-2 *years* becoming fluent in things that feel unrelated to malware — memory layout, calling conventions, how a compiler turns C into instructions, how an OS loads a process. Skipping to "malware analysis" without that foundation produces someone who can run a tool and read a blog post's conclusions, not someone who can reason about unfamiliar code from scratch. That's the ceiling this curriculum is built to get you past.

**On Apple security research being the actual dream job:** that changes the destination, not the route. Windows stays the training ground through Phase 6 — it has the deepest malware corpus, the best-documented internals (the Windows Internals books have no real macOS equivalent), and virtually all RE tooling assumes it, so it's still the fastest way to become genuinely good at reverse engineering *anything*. The macOS/iOS specialization (Phase 8) is the explicit endpoint this whole plan is aimed at — but it stays last, after general RE fluency is real, not pulled forward just because it's the goal. Reverse engineering fundamentals transfer across executable formats and processors; the reasoning process is what you're actually building.

**On ARM64 timing (revised in v4):** earlier versions had you learning x86-64 and ARM64 simultaneously from the start of Phase 4. That's been walked back — mastering one instruction set deeply first, then transferring that fluency to a second one, is faster than splitting attention across two from zero (the same reason learning a second spoken language goes faster once you're actually fluent in a first one). So: **x86-64 to real fluency first in Phase 4, then ARM64 as a faster second pass at the end of Phase 4** — not simultaneous, but also not pushed all the way out to Phase 8, since that would mean relearning assembly reasoning from scratch right when Apple specialization actually needs it.

Rough shape, gated by demonstrated mastery, not by the calendar:
- **Programming + architecture + OS internals.** This will *not* feel like malware work. That's correct.
- **Assembly (x86-64 first, ARM64 second pass) + Windows Internals + intro RE** — static/dynamic analysis of clean binaries, then simple malicious ones.
- **Real malware analysis** — unpacking, deobfuscation, tool building (your own disassembler/PE parser pieces).
- **Exploit development, vulnerability research, macOS/iOS specialization begins in earnest**, portfolio + publishing.
- **Original research, open-source tooling, writing/speaking, career positioning — increasingly Apple-security-focused.**

**On the Ghidra/Volatility/Immersive Labs exposure you confirmed:** that's real and useful — you won't be meeting these tools for the first time in Phase 5, and Sprint 3 onward lets you use Ghidra as an optional viewer for your own compiled code specifically because you already have some comfort with its interface. What it doesn't do is change the sequencing. Guided lab exercises build tool familiarity, not the underlying model of what a compiler, a stack frame, or a calling convention actually is — and that gap is precisely what turns "can follow a walkthrough" into "can independently reason about an unfamiliar binary," which is the actual skill this whole plan is built around.

If you try to jump ahead because it's more exciting, I will tell you to stop. That's the deal.

---


## How This Works Now (v2 changes)

A few structural changes from v1, and why:

**1. Mastery-gated, not calendar-gated.** "Week 1" and "Month 1" become **Sprint 1**, **Sprint 2**, etc. A sprint ends when you can pass its mastery checklist — not when a certain number of days pass. Some sprints take you 5 days, some take 3 weeks. That's fine. Malware doesn't care how many calendar months you've studied; it cares whether you actually understand what you're looking at.

**2. Every sprint ends in something tangible, and sprints build toward a monthly-scale capstone.** Not "read chapter 3" — "build X, which chapter 3 makes possible." Capstones turn a pile of disconnected exercises into an actual portfolio.

**3. Your training hour is fully flexible, but still interruptible.** Since you're remote, it's not a "work vs. home" split anymore — same environment either way, take it whenever the shift allows. The one real constraint left is that a live incident can still pull you away mid-session, so a session that survives being cut off (reading, notes, review) is a safer default than a debugging session you can't easily resume — but that's your judgment call each shift, not a fixed rule.

**4. Your SOC job is part of the curriculum, not separate from it.** When a sprint covers files, you enumerate real files with a script. When it covers processes, you actually open Process Explorer at work (where appropriate) and look at real Windows processes instead of a diagram. When it covers networking, you connect socket concepts to traffic you already see on shift. You have access to real systems every week — that's not a distraction from the plan, it's part of it.

**5. Research Notebook, separate from your code repo.** Every non-trivial concept gets an entry in this format:

```
Topic:        Pointers
Question:     Why do pointers exist?
Hypothesis:   A pointer stores the memory address of another object,
              so you can reference data without copying it.
Experiment:   Write a C program that prints the addresses of several
              variables using &.
Observation:  [what you actually saw]
Conclusion:   [what you now believe, in your own words, after seeing it]
```
This isn't class notes. It's you doing science on your own understanding — form a hypothesis, test it, record what actually happened. Storage doesn't matter (Apple Notes, plain markdown in the repo, whatever you'll actually use consistently) — a `research-notebook/` folder of dated markdown files in the same GitHub repo is the default unless you tell me otherwise, since it's versioned automatically and searchable.

**6. First-principles checklist per major concept.** Before moving on from any concept, you should be able to answer its checklist unaided. Example for *variables*:
- What is a variable?
- Where does it physically exist while the program runs?
- Who allocates it, and who destroys it?
- Can two variables reference the same underlying data?
- Why do types exist at all?

Every phase below will include one of these for its core concept.

**7. GitHub from the first exercise, not "eventually."** Every project, every exercise, every failed attempt gets committed. The commit history is the record of your growth — including the mistakes, which is the point.

**8. The Three Outputs, cadence adjusted for reality.** Every sprint you produce:
- **Code** — the project/exercise itself.
- **Notes** — a Research Notebook entry.
- **Writing** — this ramps up, it doesn't start at full speed. Weeks 1-8: one short reflection post *per capstone* (monthly), not weekly — you won't have anything worth writing about yet, and forcing weekly articles from zero just produces guilt, not skill. Once you're in real RE work (Phase 5+), this moves to biweekly, then weekly as you accumulate things worth writing about.

**9. *Computer Systems: A Programmer's Perspective* gets used earlier, but selectively** — as a reference pulled in whenever a topic calls for it (e.g., its chapter on machine-level representation once you hit assembly), not as a cover-to-cover read done in one block. It stays on the reading list from Phase 1 onward instead of appearing only as a later milestone.

**One thing I'm deliberately *not* doing:** writing you a 300-page, five-volume program for the next four years right now. That's not rigor, it's a planning mistake — nobody can accurately spec Year 3's weekly content before Year 1 has told us how you actually learn, what you struggle with, and what the field looks like by then. You'd spend your first weeks reading about the plan instead of doing Sprint 1. Instead: near-term sprints are detailed and specific; the phase map stays a phase map until you're closer, then it gets the same level of detail Sprint 1 has now. This document grows with you instead of being obsolete on arrival.

### v4 additions

**10. Every project is security-flavored — no generic software-engineering exercises.** A to-do list or a student database teaches you nothing that transfers to reading a binary. A hex viewer, a file hasher, an entropy calculator, a PE header inspector — same programming concepts (structs, file I/O, parsing, byte-level data), but every one of them maps directly onto something you'll do again in Phase 5+. From here on, every exercise and capstone gets evaluated against: *does this teach me something I'll use to understand binaries, or is it just programming for its own sake?* If the answer is the latter, it's cut.

**11. Reverse-engineer your own code, before anyone else's.** Starting Sprint 3 (once C and a compiler exist), every capstone ends with the same loop: compile it, disassemble/step through it, and try to explain what you're seeing in terms of the C you actually wrote. This is deliberately "RE before RE" — by the time you open a real malicious binary, you'll already have done the core move (source → compiled form → reasoning about behavior from the compiled form) dozens of times on code where you already know the ground truth.

**12. Debuggers from Sprint 3 onward, alongside print-debugging, not instead of it — and lldb, not gdb, given your machine.** Sprint 1–2 kept you on `print()`-only deliberately — reasoning through state without a tool once is a real skill. But once a debugger exists (Sprint 3), you step through your own code and watch variables/registers change live. On macOS specifically, `gdb` requires jumping through code-signing hoops to get past System Integrity Protection — a real pain point for zero payoff. `lldb` ships with the Xcode Command Line Tools you already installed, works immediately, and is the actual tool you'll be using for macOS/iOS work later anyway, so there's no reason to learn gdb first and relearn lldb after. Both stay conceptually in the toolkit for the future (gdb shows up again once you're doing Linux-side work), but lldb is primary starting now.

**13. Math, woven into whatever project needs it, not a standalone module.** Binary/hex representation shows up naturally when you build a hex/binary converter. Bitwise operations and boolean algebra show up when C's `&`, `|`, `^`, `<<`, `>>` operators show up. Modular arithmetic gets introduced once something you're building actually needs it (hashing, later crypto-adjacent work). No separate "math phase" — it arrives exactly when it's load-bearing.

**14. Architecture questions seeded early, without assembly syntax.** Starting Sprint 1, expect occasional sidebar questions like "what is RAM, actually?", "why can't the CPU run Python directly?", "what is a register?" — no answers expected yet, no assembly required, just questions planted so that when Phase 4 (Assembly) actually answers them, you're resolving something you've already been wondering about instead of encountering it cold.

**15. Every project ships with a real README.** Not just "what it does" — design decisions, assumptions, known limitations, and what you'd improve with more time. This is the habit that makes a GitHub portfolio look like research work instead of a homework folder, and it costs almost nothing to build now versus retrofitting it onto years of projects later.

**16. Four-track lens per sprint (naming borrowed from a useful reframe, not a restructure):** Theory (reading), Implementation (code), Observation (debugger/disassembler/notebook), Communication (writing). Nothing here changes what you're doing — it's the same reading/coding/notebook/writing this document already had — but it's worth checking, per sprint, that you actually touched all four rather than only ever doing the one that's most comfortable.

---

## GitHub Setup — Start Here (you're new to this, so this is the actual walkthrough)

Searching and starring is browsing GitHub. What you need now is *using* it to track your own work — a different, much smaller skill than it looks like from outside. Do this once, and it becomes muscle memory.

**1. Install Git (the tool) — separate from GitHub (the website).** Git tracks changes on your machine; GitHub is where you back that history up online.
- Windows: install [Git for Windows](https://git-scm.com/download/win), or if you're using WSL2 (which Sprint 1 already has you setting up), Git is likely already there — check with `git --version` in the WSL2 terminal.
- Confirm it worked: open a terminal and run `git --version`. You should see a version number.

**2. Set your identity (one-time):**
```
git config --global user.name "Your Name"
git config --global user.email "the-email-you-used-for-github@example.com"
```

**3. Create the repo on GitHub.com** (not on your machine — start on the website):
- Click the **+** in the top right → **New repository**.
- Name it something like `malware-re-journey` (this replaces the earlier `programming-fundamentals` / `c-fundamentals` split — one repo for everything, simpler to manage while you're learning the tool itself).
- Set it to **Public** (you want this visible later as a portfolio).
- Check **"Add a README file"**.
- Click **Create repository**.

**4. Clone it to your machine** (this downloads a working copy you can edit):
- On the repo page, click the green **Code** button, copy the HTTPS URL.
- In your terminal:
```
git clone https://github.com/your-username/malware-re-journey.git
cd malware-re-journey
```

**5. The loop you'll repeat constantly** (this is 90% of the Git you need for a long time):
```
git add .                     # stage everything you changed
git commit -m "short message describing what you did"
git push                      # send it to GitHub
```
Do this after *every* meaningful chunk of work — after finishing an exercise, after fixing a bug, after writing a notebook entry. Small, frequent commits with honest messages ("min/max exercise, still buggy" is a fine commit message) are exactly what builds the growth-over-time record the curriculum wants from your portfolio.

**6. Repo structure** — create these folders inside `malware-re-journey`:
```
malware-re-journey/
├── curriculum.md            ← this document lives here
├── README.md                ← landing page, GitHub shows this automatically
├── research-notebook/       ← dated markdown files, one per entry
├── python/                  ← Sprint 1–2 exercises and Capstone 1
├── c/                       ← Sprint 3+ exercises and Capstone 2 onward
└── capstones/                ← optional: pull finished capstones here with their own READMEs once done
```
Put this curriculum file in as `curriculum.md` at the root right now — that's your first commit.

**7. Two things that trip up beginners specifically:**
- `git status` any time you're unsure what's changed or staged — it's always safe to run and tells you exactly where things stand.
- If `git push` ever fails with something about the remote having changes you don't have locally, run `git pull` first, then push again. This won't come up much since you're the only one working in this repo, but it happens if you ever edit a file directly on GitHub's website and forget to pull it down.

That's the whole toolkit needed for now. Everything else in Git (branches, merging, rebasing) is worth learning eventually but not blocking anything in Sprint 1.

---

## Phase Map

Each phase answers a real research question — worth keeping in view when a sprint feels disconnected from "malware."

| Phase | Focus | Guiding question | Gate to enter next phase |
|---|---|---|---|
| 1 | Programming for Reverse Engineering (Python → C) | How does software become machine code? | Pass Phase 1 mastery checklist |
| 2 | Computer Architecture | What does the CPU actually execute, and how? | Explain stack/heap, memory layout, ISA basics unaided |
| 3 | Operating Systems (concepts → Windows Internals) | How does the OS run and manage that machine code? | Explain process/thread/memory management unaided |
| 4 | Assembly — **x86-64 to fluency first, ARM64 as a transfer pass** | What is the CPU literally doing, instruction by instruction? | Read compiler-generated asm from your own C, on x86-64 unaided, then demonstrate the same on ARM64 |
| 5 | Static & Dynamic Analysis Fundamentals | How do I recover intent from software I didn't write? | Independently analyze a clean, unfamiliar binary end-to-end |
| 6 | Malware Analysis (real samples, tool building) | How do attackers abuse these mechanisms? | 100 samples analyzed, PE parser + partial disassembler built |
| 7 | Exploit Development & Vulnerability Research | How do I find entirely new behavior, not just recognize known patterns? | — |
| 8 | **macOS/iOS Security Research — the destination.** XNU internals, Mach-O format, Objective-C/Swift runtime, iOS security model, kernel concepts | Same questions as Phases 5–7, asked of a different OS/format | Begins in earnest here, not as an afterthought |
| 9 | Advanced Research, Publishing, Career — increasingly Apple-security-focused | — | Ongoing from Phase 6 onward |

**Executable format order, given the actual destination:** PE first (Phase 5–6, richest tooling/docs/malware corpus to learn on) → Mach-O (Phase 8, where it matters for the actual goal) → ELF picked up opportunistically rather than as its own dedicated block, since Linux malware research isn't the direction this is headed.

No skipping phases. Sprints within a phase can move at your actual pace.

---

## PHASE 1, SPRINT 1: Programming Fundamentals — Foundations

**Why this phase exists:** You cannot reverse engineer what you cannot write. Every mental model you'll build later — "why does this function prologue look like this," "why is this loop unrolled" — depends on having written and compiled that kind of code yourself and watched it behave.

**Why Python first, even though the long-term anchor language is C:** The skill you're actually missing isn't "C syntax," it's *thinking like a programmer* — breaking a problem into steps, tracing your own logic, debugging by reasoning instead of guessing. Python builds that with the least friction possible. Once it exists, C becomes "same thinking, plus manual memory management" — a much smaller jump than learning both at once from a terse reference text. C still becomes primary starting Sprint 3, and stays primary for the rest of the curriculum — Python remains your tooling/scripting language (including, soon, scripts you run against real SOC data).

**Primary resources:**
- *Automate the Boring Stuff with Python* (free online) — Sprints 1–2
- *C Programming: A Modern Approach*, 2nd ed. (K.N. King) — Sprint 3 onward, primary C text (far more scaffolding for true beginners than K&R)
- *The C Programming Language* (K&R) — held back as a **second-pass reference** in Phase 2, once you can already write C and want the sharpest, most precise description of the language
- *Computer Systems: A Programmer's Perspective* — kept on hand from now on as a reference, pulled in per-topic starting in Phase 2
- Compiler Explorer (godbolt.org) — starting Sprint 3

**GitHub:** Use the `malware-re-journey` repo you set up in the GitHub Setup section above — put Sprint 1–2 exercises in the `python/` folder. First commit: `curriculum.md` plus this folder structure. Every exercise below gets its own commit.

**Research Notebook:** Create the `research-notebook/` folder (or your notes system of choice) now. First entry goes in this sprint.

### Sprint 1 — Python: control flow, functions, debugging (moving fast past what you already know)
You're already comfortable with `print()`, variables, constants, and simple arithmetic — so this sprint skips straight past that and spends its time where you're actually weak: conditionals, loops, functions, and debugging your own logic.
- **Objectives:** Conditionals and loops used confidently, functions that take/return values, basic debugging habits (reading a traceback, using `print` to inspect state, isolating where logic breaks).
- **Reading:** *Automate the Boring Stuff*, Ch. 2–3 (skim Ch. 1 for anything unfamiliar only)
- **Setup:** Get a Linux VM (Ubuntu) or WSL2 working now — needed from Sprint 3 onward, better to have it solid before relying on it.
- **Exercises:**
  - Number-guessing game (uses conditionals + loop)
  - Simple calculator (two numbers + an operator) refactored into a function, e.g. `calculate(a, b, op)`
  - Min/max/sum/average over a hardcoded list, using a loop — no `min()`/`max()`/`sum()`, write the logic yourself
  - Deliberately break one of the above (introduce an off-by-one or wrong comparison), then find and fix it using only `print` statements and reasoning — no guessing
  - **Scope exercise (added after baseline assessment surfaced this as a real gap, not hypothetical):**
    ```python
    x = 5
    def add():
        x = 10
        return x
    print(x)
    print(add())
    print(x)
    ```
    Predict the output on paper *before* running it — write down all three lines. Then run it. If your prediction doesn't match (it's `5`, `10`, `5` — the `x` inside `add()` is a completely different variable that only exists inside that function), don't just note the right answer and move on. Change the function to `x = x + 1` instead of `x = 10` and predict again before running — this will throw a `NameError` (referencing `x` before it's locally assigned), which is the mechanism actually showing you that Python decided `x` is local *the moment it sees `x = ...` anywhere in the function body*, before that line ever executes. Write the Research Notebook entry on this one specifically — it's a real misconception now, worth actually fixing rather than shrugging past.
  - **One-line grounding exercise, before C makes this literal:** add `print(id(x))` right after each of the three `print(x)`/`print(add())` calls above. `id()` isn't something you'll use in real code, but it gives you a concrete number for "where this variable's value actually is" — you should see the outer `x` show the same `id()` both times, and the `x` inside `add()` show a *different* one, even though both are named `x`. That's the first real (if implementation-specific) glimpse of something C will make fully literal in Sprint 3: a variable isn't just a name, it's a name attached to a specific location.
- **Research Notebook entry:** Topic = "What is a loop actually doing?" Hypothesis, experiment (trace your min/max program by hand, variable by variable), observation, conclusion. Second entry this sprint: the scope exercise above.
- **First-principles checklist (variables/functions):**
  - What is a variable, and where does it live while the program runs (conceptually — Phase 2 makes this literal)?
  - What does a function actually do that copy-pasting the same code twice doesn't?
  - When you call a function, what happens to the values you pass in — copied or shared?
  - **What is variable scope, and why can a variable inside a function share a name with one outside it without conflict?**
- **SOC connection:** None yet — Sprint 2 introduces this.
- **Estimated hours:** 7–8 (lighter than before, since you're not starting from zero)
- **Mastery gate to move to Sprint 2:** Write a new small program (not one you've seen before) using a loop and a function, without looking up syntax more than once or twice — and deliberately debug a broken version of it using reasoning, not trial and error.

### Sprint 2 — Python: data structures, files, and a real SOC-flavored script
- **Objectives:** Lists, dicts, functions that take/return structured data, file I/O.
- **Reading:** *Automate the Boring Stuff*, Ch. 4–5, 8
- **Exercise 1 (security-flavored, replaces earlier "grade tracker"):** File hasher — script that takes a file path, reads it, computes and prints its MD5/SHA-256 hash using Python's `hashlib`. Then extend it to accept a directory and hash every file in it, storing results in a dict keyed by filename. This is the exact operation you'd do to fingerprint a sample or check a file against a known-hash list.
- **Exercise 2 (SOC connection):** Write a script that enumerates files in a directory (start with a folder on your own machine) and prints name, size, and modification time for each. This is deliberately the same shape as things you'll eventually do against real log/artifact directories at work.
- **Research Notebook entry:** "Why is a dictionary the right structure here and a list wrong?" — answer in terms of lookup, not convenience.
- **Estimated hours:** 10
- **Mastery gate:** Both exercises committed; you can explain, unprompted, when you'd reach for a list vs. a dict.

### Capstone 1 — Command-Line Triage Toolkit (Python)
Combine Sprints 1–2 into one small toolkit with 3+ subcommands (e.g., `triage hash file.txt`, `triage hashdir ./somedir`, `triage filescan ./somedir`). Add a fourth if you're moving fast: a simple **entropy calculator** (compute Shannon entropy of a file — high entropy is a real signal for packed/encrypted malware, and you're building the actual metric analysts use, not a toy). One repo, one README (see the README standard below) explaining what each command does, why you built it that way, and what you'd add with more time. This is portfolio entry #1 — not a throwaway exercise.
- **Estimated hours:** 5–7
- **Deliverable:** Committed, README written, one short reflection post (a few paragraphs, not a polished article) on what surprised you this month.

---

## PHASE 1, SPRINT 3: Transition to C

- **Objectives:** Map what you already know in Python onto C's stricter, lower-level version of the same ideas. Introduce pointers — including correcting a specific misconception the baseline assessment surfaced. Introduce lldb and the self-RE loop.
- **Note on your Q9 answer ("a pointer tells the CPU where the next instruction is"):** that's actually describing the *instruction pointer* — a specific CPU register that tracks what to execute next, which you'll meet properly in Phase 2/4. A pointer in the general sense is much simpler and more common than that: it's just a variable that stores the memory address of *some other piece of data* — not necessarily an instruction, usually not. Worth flagging explicitly because you already have a specific model in your head, and a vague re-explanation won't dislodge a specific wrong one — this sprint's pointer exercises should directly contradict it.
- **Two more corrections from the baseline assessment, held for Phase 2/3 where they get built properly, but worth naming now so they're not silently reinforced:** your Q10 answer ("the stack is memory ready to be processed") is close but not quite it — the stack is a specific, structured region that grows and shrinks automatically as functions call and return, holding local variables and return addresses in strict last-in-first-out order, not generic "ready" memory. And your Q6 answer treated a process like it's essentially memory pushed onto a stack for the CPU — a process is actually a running *instance* of a program, with its own address space and execution context; Phase 3 covers this properly.
- **Reading:** King Ch. 1–2 (fundamentals), Ch. 10 (pointers — introduced early on purpose; King scaffolds it well)
- **Setup:** Install `gcc`/`clang`, `make` (you already have these from the Xcode Command Line Tools). `lldb` is already present on macOS — confirm with `lldb --version`.
- **Exercises:**
  - Rewrite your Sprint 1 min/max program in C. Notice everywhere C forces explicitness that Python let you skip.
  - Compile a simple C function on godbolt.org at `-O0`. Don't try to fully read the output yet — just notice your variable names are gone, replaced by memory locations/registers.
  - Implement `swap(int *a, int *b)` with pointers, and a function that reverses a C-string in place.
  - **Pointer myth-bust exercise:** declare two ordinary `int` variables, print their addresses with `&`, then declare a pointer to one of them and print *its* address too (the pointer's own address, not what it points to) alongside the value it points to (`*ptr`). You should see three distinct addresses/values. This directly demonstrates: a pointer is data with an address of its own, that happens to store another address as its value — nothing about "next instruction" involved.
  - **Debugger, first contact:** run `lldb` on your min/max program. Set a breakpoint inside the loop (`b <function>` or `b <line>`), step through it line by line (`next`), and print the loop variable at each step (`p varname`). Watch it change in real time — this is the same tracing you did on paper in Sprint 1, now with the machine confirming it instead of you predicting it.
  - **Self-RE loop, first pass:** write a tiny `factorial(int n)` function, compile it, and look at its disassembly. Two options, use whichever: `lldb`'s built-in `disassemble` command on the compiled binary (fastest, no extra install), or open it in **Ghidra** if you'd rather use an interface you already have some comfort with from your labs — either is fine, the point is finding the loop/recursive call in the output, not the specific tool. Don't try to understand every instruction yet — just point to the instructions that correspond to what you wrote.
- **Research Notebook entry:** The pointers example from the "How This Works" section above — do that one for real.
- **First-principles checklist (pointers):**
  - What does a pointer actually store?
  - Why does `swap(int a, int b)` fail while the pointer version works?
  - What's the difference between the pointer and the thing it points to?
- **Architecture sidebar (no answers needed yet, just sit with them):** Why can't the CPU just run your C source file directly? What does "compiling" actually hand the CPU that source code doesn't?
- **Estimated hours:** 13
- **Mastery gate:** Explain the swap example in terms of memory, not syntax, unaided.

## PHASE 1, SPRINT 4: Arrays, strings, structs, the heap

- **Objectives:** Arrays as contiguous memory, the array/pointer relationship, structs, first real `malloc`/`free` use.
- **Reading:** King Ch. 8 (arrays), Ch. 9 (strings), Ch. 16 (dynamic memory), Ch. 17 (structs)
- **Exercises:**
  - Implement your own `strlen`, `strcpy`, `strcmp` without `<string.h>`.
  - **Hex/binary converter (security-flavored, replaces earlier "grade tracker"):** CLI tool that takes a number or file and prints it in hex, binary, and decimal, and can go the other direction too. This is where bitwise operators and boolean algebra get introduced naturally — `&`, `|`, `^`, `<<`, `>>` — as tools this exercise actually needs, not abstract math.
- **Research Notebook entry:** What happens in memory when you call `malloc(sizeof(struct SomeType) * 10)`? Write the hypothesis before you test it, not after.
- **Estimated hours:** 13
- **Mastery gate (Phase 1 exit):** Explain the `malloc` question above unaided. If not, stay here — do not enter Phase 2 with a shaky memory model, everything downstream depends on it.

### Capstone 2 — Hex Viewer / File Inspector (C)
A CLI tool that takes a file path and prints its contents as a hex + ASCII dump (the classic `xxd`-style two-column view), struct-based where it makes sense, using `malloc`/`realloc` for anything dynamically sized. Extend it if you have time: detect and print the file's magic bytes (the first few bytes that identify file type — this is a direct preview of PE/ELF/Mach-O header parsing coming in later phases). This replaces the earlier "contact manager" — same C concepts (structs, dynamic memory, file I/O), but every part of it maps onto something you'll do again the moment you're looking at real binaries. Finish with the self-RE loop: compile it, disassemble a small piece of it, and see if you can identify the part handling the byte-reading loop.
- **Deliverable:** Committed, README (see standard below), short reflection post.

---

## README Standard (every capstone from here on)

Every project's README should cover, briefly:
- **What it does** and how to build/run it
- **Design decisions** — why you structured it this way, not another way
- **Assumptions** — what you assumed about input, environment, etc.
- **Limitations** — what it doesn't handle
- **What you'd improve** with more time

This is what turns a folder of exercises into something that reads as research work rather than homework — and it costs a few extra minutes per project done now versus a much bigger retrofit job years from now.

---

## Phase 1 Exit Checklist (all must be true before Phase 2 starts)

- [ ] Write a new, unseen small C program without referencing syntax more than once or twice
- [ ] Explain the compilation pipeline (preprocess → compile → assemble → link) unprompted
- [ ] Explain, from memory, what `malloc`/`free` are doing conceptually
- [ ] Explain why `swap(int a, int b)` fails and the pointer version works
- [ ] Two capstones committed to GitHub with READMEs
- [ ] At least 4 Research Notebook entries
- [ ] At least 1 reflection post written

---

## Coming Phases — Capstone Preview (detail expands as you approach each one)

| Phase | Example capstone |
|---|---|
| 2 (Architecture) | Hex editor |
| 3 (OS / Windows Internals) | Mini shell |
| 4 (Assembly) | PE Explorer (read PE headers/sections by hand) |
| 5 (Static/Dynamic Analysis) | File system / artifact analyzer |
| 6 (Malware Analysis) | Malware configuration extractor, basic PE parser, partial disassembler |
| 7+ | Debugger helper/plugin, original tooling tied to your research |

Full sprint-level detail for each of these gets written once you're one phase away — not now, for the reasons in "How This Works."

---

## Reading & Practice Resources by Phase (reference — pulled in as each phase starts)

| Phase | Resources |
|---|---|
| 1 | *Automate the Boring Stuff*, *C Programming: A Modern Approach* (King) |
| 2 | *Computer Systems: A Programmer's Perspective* (selectively), K&R (2nd pass) |
| 3 | *Operating Systems: Three Easy Pieces* (free online), *Windows Internals Part 1* |
| 4 | *The Art of Assembly Language*, Intel x86-64 manuals (reference), ARM Architecture Reference Manual (reference), **pwn.college — "Program Security" and "System Security" dojos begin here** |
| 5 | *Practical Binary Analysis*, pwn.college continued (memory corruption modules, now that Phase 4 gives you the assembly/memory model to actually understand what's happening instead of pattern-matching solutions). **Tool stack:** Ghidra (free, widely used, and you already have some familiarity from labs) as primary disassembler; Binary Ninja worth learning too — its interface and intermediate representation are genuinely good for building intuition; IDA (Free or Pro) only when a specific project actually benefits from it, not by default. `lldb` stays your primary debugger. Tool choice follows the underlying concept, not the other way around — the goal is understanding what you're looking at well enough that the specific tool stops mattering much. |
| 6 | *Practical Malware Analysis* |
| 7 | pwn.college's exploitation-focused dojos become central here, alongside vulnerability research papers |
| 8 | Apple's own developer/security documentation, XNU source (public parts), relevant WWDC security talks, macOS/iOS-focused research papers and conference talks (Black Hat, Objective by the Sea) |

**On pwn.college specifically:** it's a genuinely good fit for how you want to learn — free, dependency-structured, lecture-backed rather than "here's a CTF, good luck." But it assumes C fluency and Linux comfort, and its meatier modules touch memory corruption early. Starting it before Phase 4 would put you in exactly the position this curriculum is built to avoid: running challenges pattern-matched from writeups instead of reasoning from a real model of what's happening in memory and registers. From Phase 4 onward, it becomes a standing part of the plan, not a one-off.

---

## Monthly System (from Phase 2 onward)

Each month/phase transition, you get:
- Books/papers for that stretch
- Projects (code + RE where applicable), tied to that phase's capstone
- Malware samples — **only from Phase 5+ onward**, from legitimate research sources (theZoo, MalwareBazaar, VX-Underground), always in an isolated, network-disabled VM/sandbox, never on host
- Knowledge gaps to close, based on what actually tripped you up
- Portfolio/GitHub/writing goals
- Conference recommendations when relevant (REcon, Black Hat/DEF CON talks on YouTube are free and excellent before you ever attend in person; Objective by the Sea specifically once you're in Phase 8, since it's Apple-security-focused)

---

## Long-Range Milestones (running list, objective completion criteria)

- [ ] Two Phase 1 capstones shipped
- [ ] Read King's *C Programming: A Modern Approach* completely, exercises attempted
- [ ] Read K&R as a second pass once comfortable in C
- [ ] Explain stack vs. heap with a concrete example (Phase 2 gate)
- [ ] Comfortably read compiler-generated assembly from your own C code, **on both x86-64 and ARM64**, unaided (Phase 4 gate)
- [ ] Read *Windows Internals Part 1* completely
- [ ] Independently statically analyze 25 non-malicious PE files
- [ ] Reverse engineer 100 malware samples (cumulative)
- [ ] Build a PE parser from scratch
- [ ] Build a basic disassembler (even x86 subset) from scratch
- [ ] Explain Mach-O format and how it differs from PE/ELF, unaided (Phase 8 gate)
- [ ] Analyze a macOS/iOS binary end-to-end, statically and dynamically (Phase 8)
- [ ] Publish 25 technical write-ups (ramping cadence per "Three Outputs" above)
- [ ] Open-source one non-trivial framework/tool
- [ ] Submit a conference CFP — Objective by the Sea is the natural target once Phase 8 work exists (acceptance is bonus, submission is the objective milestone)

---

## Accountability Rules

- No commit for 2+ weeks → I flag it. That's a signal you've stalled or you're passively consuming instead of building.
- Ask about something outside your current phase (e.g., exploit dev while still in Phase 1) → I evaluate it. If it doesn't serve where you are, it goes on your **Later List**, tracked and resurfaced when it's actually time.
- "I read about X" does not substitute for "I built/broke/wrote X." Reading is input; the deliverable is the unit of progress.
- Periodically, I'll ask you to explain a foundational concept from an earlier phase, unprompted — this is how we catch decayed fundamentals before they cause problems in Phase 5+.
- Mastery gates are real gates. If you haven't hit one, we don't move forward, even if the calendar says you "should" be further along.
- **On asking an LLM when you hit something you don't understand (this rule exists because of your own baseline assessment answer to Q18):** the reflex to ask isn't the problem — outsourcing the reasoning step before attempting it yourself is. Before asking, the Research Notebook format already gives you the move: write a hypothesis, try to verify it yourself (docs, experimentation, tracing by hand), *then* ask if you're still stuck — and ask for the explanation, not the answer. Anything an LLM had to explain to you that you couldn't get to yourself becomes a real checklist item to revisit later, not a solved problem to move past.

---

## Appendix: Sample Week — Sprint 1 in Practice

This is what actually following this curriculum looks like, hour by hour, for the current Sprint 1 (Python control flow, functions, debugging). Adjust the exact days to your real shift pattern — the point is the *shape*, not the specific clock times.

**Sunday night shift (training hour, whenever it lands):**
- Read *Automate the Boring Stuff* Ch. 2 (Flow Control) — full chapter, don't skim.
- Open a scratch `.py` file and retype (don't copy-paste) 2–3 of the chapter's own examples as you read, running each one. Typing it yourself is what makes the syntax stick — copy-paste lets your eyes agree without your hands actually knowing it.
- Close by writing one sentence in your own words: "A loop is ___." No notebook entry yet — that comes later once you've built something with it.

**Monday night shift (training hour):**
- Read Ch. 3 (Functions).
- Same retype-and-run habit for 2–3 examples.
- Write the number-guessing game exercise from Sprint 1 — this is a short program, doable in the hour. Get it working, don't polish it.
- Commit it to `malware-re-journey/python/` with a one-line commit message ("guessing game, sprint 1").

**Tuesday night shift (training hour):**
- Write the calculator exercise, structured as a function: `calculate(a, b, op)`.
- Test it manually with 4–5 different inputs, including one that should fail (e.g., division by zero) — notice what actually happens, don't just assume.
- Commit it.

**Wednesday night shift (training hour):**
- Write the min/max/sum/average-over-a-list exercise, using a loop, no built-ins.
- Before running it, predict on paper what the output will be for a specific test list. Then run it and compare. If it doesn't match your prediction, that mismatch is the whole point — it means your mental model and the code disagree somewhere, and finding that gap is the actual skill.
- Commit it.

**Thursday (day off, ~3 hrs):**
- Take one of this week's three programs and deliberately break it — introduce a wrong comparison operator or an off-by-one error.
- Debug it using only `print()` statements and reasoning about what you'd expect to see at each point — no guessing, no random changes to see if something works.
- Once fixed, write the Research Notebook entry: Topic = "What is a loop actually doing?" — Hypothesis, Experiment (trace the min/max program by hand, variable by variable), Observation, Conclusion.

**Friday (day off, ~3 hrs):**
- Self-administer the Sprint 1 mastery gate: write a *new* small program you haven't seen before (pick a slightly different problem — e.g., count vowels in a string using a loop and a function) without looking up syntax more than once or twice.
- If it takes more than two lookups or you're guessing at syntax, that's real signal — spend this block reinforcing Ch. 2–3 instead of moving on, and it's not a setback, it's the gate doing its job.

**Saturday (day off, ~2 hrs, buffer/catch-up):**
- If Friday's gate passed: write the short reflection note for the sprint (a few paragraphs — what surprised you, what was harder than expected) and read ahead into Sprint 2's material.
- If Friday's gate didn't pass: use this block to close the gap, not to push forward anyway.

That's ~14–16 hours for the week, matching your real availability, and it ends with either a clean pass into Sprint 2 or a clear, specific reason to stay put — never a vague "I guess I'm ready."

---

*Next: Sprint 1 this week, following the sample week above as a template. Come back with the exercises (or with where you got stuck) and we'll do Sprint 2 at the same depth. Capstone 1 detail expands once you're through Sprint 2.*
