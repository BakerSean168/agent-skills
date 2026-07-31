# Learning Doc / Project Tutorial

Use this when the user asks for a learning document, tutorial, or "teach me X by building Y" — especially when they want to understand a real project end to end instead of collecting API facts.

This reference exists because the naive output ("here are the libraries: `os`, `bufio`, `strings`, `fmt`" with one line each) is technically correct but pedagogically useless. It skips the three layers a learner actually needs: *why* a thing exists, *how* it connects to the next step, and *where* it appears in the running program.

## When to use

Trigger phrases include: "写篇教程", "整理成学习笔记", "教我做一个 X", "把这个项目讲清楚", "generate a learning doc", "tutorial for ...". Also self-trigger the moment you notice your own explanation collapsing into a list of library names or one-line API descriptions.

## Core principle: mental model before API

The single most important rule, stated up front in every such doc:

> Do NOT write the doc as a standard library cheat sheet. Write it as a project tutorial organized around a real data flow.

A tutorial that opens with "you will use `os`, `bufio`, `strings`, `fmt`" and one-lines each ("`bufio.Scanner` 用于逐行读取") fails even when every sentence is true. It omits:
1. **Why this exists** — calling `Read` directly forces you to manage buffers and chunked reads yourself; `Scanner` wraps that into line-splitting.
2. **How it connects** — `os.Open → *os.File → io.Reader → Scanner`.
3. **Where it appears in the flow** — get path → open file → build scanner → loop → check empty → check error.

## Document spec (state these up front)

Before any teaching, open the doc with a short spec so the reader knows what they are holding (this is the "Scribe" framing: type, reader, tools, level):
- **Document type**: project tutorial / concept note / exercise set.
- **Target reader**: what they are assumed to know already.
- **Tools involved**: language, standard libraries, commands.
- **Output level**: what the reader should be able to do at the end (e.g., "independently rewrite the program").

## The four required adjustments

1. Organize by **program execution flow**, not by library name. Sequence content as the program actually runs: user input → open file → data flows in → process line by line → output. Do not open with "Here are the libraries you will use."
2. For every new concept, explain **why it is needed** *before* showing the API. Order: problem → why-this-project-needs-it → API.
3. **Make the type connections explicit.** State the chain, e.g. `file path (string) → os.Open → *os.File → io.Reader → bufio.NewScanner → *bufio.Scanner → each line text`. Directly answer "why can a file be passed where an `io.Reader` is expected."
4. Set the goal as **independent rewrite**, not "a runnable copy." The reader should be able to rebuild the program from scratch without looking back.

## Required document structure (8 parts)

1. **Analyze the problem** — restate the requirement as concrete work steps (user types command → program gets path → opens file → reads line by line → ignores empty lines and counts → outputs result → closes file and handles errors). For each step, state what problem it solves.
2. **Introduce the relevant libraries, by flow** — for each library/API the project actually touches, give the full treatment (see Per-Concept format below). No one-liners.
3. **Type relationship diagram** — show the chain and answer the connection questions: why `*os.File` is returned, why it satisfies `io.Reader`, why `Scanner` beats manual `Read`, what `Scan`/`Text`/`Err` each own. Use a "plug/socket" or "pipeline" analogy plus the precise technical reason.
4. **Stepwise implementation with fading support** — split into small runnable versions, each adding one capability:
   - v1: hardcode a file path, just verify you can open it
   - v2: print each line
   - v3: count all lines
   - v4: skip `""`
   - v5: skip whitespace-only lines via `strings.TrimSpace`
   - v6: take the path from `os.Args`
   - v7: handle errors (missing arg, file not found, no permission, scan error)
   - v8: extract a testable function `CountNonEmptyLines(r io.Reader) (int, error)`; `main` only does arg / Open / call / print
   Use full worked code for the first one or two versions only. Then progressively switch to changed fragments, incomplete skeletons, tests, and requirement-only tasks. Put any final reference implementation after the independent task or in a clearly separated answer key. For every version: state what changed, explain key lines and why they are shaped this way, give a run command, expected behavior, and one small exercise.
5. **Final code walkthrough** — explain data flow; label concrete types vs interface types; separate business logic from OS-interaction code; explain dependency inversion (why the function takes `io.Reader`, not a path or `*os.File`); explain why tests can pass `strings.NewReader`.
6. **Testing** — write table-driven tests for the core function covering at least: empty file, one line, multiple lines, interior blank line, whitespace-only line, tab-only blank line, no trailing newline, Windows `\r\n`, Chinese content. Explain what table-driven testing is, why no real file is needed, what `strings.NewReader` does and why it qualifies as `io.Reader`.
7. **Common errors** — list beginner traps with a wrong snippet and the correct version: forgetting to check `os.Open` error; forgetting `defer file.Close()`; placing `defer` before the error check; using `line == ""` instead of `TrimSpace`; forgetting `scanner.Err()`; thinking `Scan()` returns the current line; treating `io.EOF` as a crash; putting all logic in `main`; depending on `*os.File` directly.
8. **Knowledge summary** — knowledge map, stdlib call relationships, the 10 key points to memorize, 5 exercises from easy to hard, extension directions (count total/empty/non-empty, multiple files, stdin, `-ignore-comments`, JSON output, large files, `flag` package), and an Obsidian-ready Markdown note.

## Per-concept explanation format

For each library/API, cover all of:
1. What it is.
2. What problem it solves in *this* project.
3. Its most important functions/types/methods, with signatures.
4. What each parameter means.
5. What each return value means.
6. A minimal example.
7. The common beginner misconception.
8. How it relates to the adjacent types in the flow.

## Expression requirements

- Default to the learner being a beginner.
- Use plain, concrete language (match the user's language); prefer analogies (socket/plug, pipeline) but never at the cost of technical accuracy.
- For every new concept, first say what problem it solves.
- Do not pile up terminology; whenever a term appears, follow it with code and the actual data flow.
- Never omit explanation of function parameters and return values.
- "It runs" is not "it is taught." Make the reader understand *why*.
- When multiple approaches exist, teach the simplest first, then the advanced option.
- End each section with a short self-check question. In a static note, continue without waiting; in an interactive coaching session, let the learner answer before revealing the next scaffold.

## Anti-patterns (do not do these)

- Listing libraries up front as "you will use X, Y, Z."
- One-line intros: "`bufio.Scanner` 用于逐行读取."
- Dumping API signatures without the problem they solve.
- Organizing by package name instead of by execution step.
- Treating "produces runnable code" as the finish line.
- Skipping the type-connection diagram.
- Showing complete code for every stage so that later stages become copy-and-edit exercises.
