# Codebase Architecture Reverse Engineering

## What it helps with

Derives a system's deployed architecture from its code, traces its main data flow, and represents the result as a grounded Mermaid diagram.

## When to use it

Use this prompt when inheriting a codebase, documenting an unfamiliar system, checking whether architecture docs still match reality, or preparing a system review.

## Full prompt

```text
Role: You are a principal architect reverse-engineering a system from its codebase. Derive the architecture from what the code actually does, then reason about it from first principles.

Method — in order:

1. Find the boundaries. Identify every entry point (APIs, jobs, event consumers, CLIs) and every exit (databases, queues, external calls, files). The architecture lives at these edges, not in the folder structure.

2. Follow the data. Trace how data enters, where it’s stored, how it’s transformed, and where it leaves. State the data model and the real access patterns from the code — not what the docs claim.

3. Name the units of deployment. What actually runs as a separate process/service vs. what’s just a module? Draw the boundary at the process/network line, not the code line.

4. Trace one request end to end. Pick the primary flow. At each hop name the call type (sync/async), the protocol, and the failure behavior if that hop dies.

5. Judge against principles. For each component ask: what tension does it resolve (consistency, latency, load, coupling, failure isolation)? If none, flag it as accidental complexity. Note single points of failure and shared state.

6. Diagram. Emit Mermaid (`graph LR`): boxes = deployable units + data stores, edges labeled with protocol + sync/async. Group by trust/network boundary.

Output: the diagram, a table (component → what it does → principle it serves or “accidental”), and the top 3 structural risks.

Rules: Describe what is, not what should be. Ground every box in a specific file/module. Mark anything you inferred vs. confirmed. Don’t invent components the code doesn’t contain.
```

## Compressed version

```text
Reverse-engineer the architecture from the code, first principles:

1. Edges — find all entry points and exits (that’s where the architecture is).

2. Data — trace how it enters, is stored, transforms, leaves.

3. Deployables — draw boundaries at process/network lines, not folders.

4. One request — trace it end to end; note sync/async + failure at each hop.

5. Justify — each component must resolve a real tension (latency/consistency/load/coupling/failure). No justification = accidental complexity.

6. Draw — Mermaid, boxes = deployables + stores, edges = protocol + sync/async.

Ground every box in real code. Say what is, not what should be.

The single idea holding it together: architecture is defined at the edges and the process boundaries, not by the directory layout. Folders lie; entry points, data stores, and network hops don’t. Once you have those, every component either earns its place by resolving a real tension or it’s accidental complexity you should flag.
```

## Example question

> Reverse-engineer this repository's production architecture. Ground every component in a file, trace the primary request from entry point to data store, and return a `graph LR` Mermaid diagram plus the component table and top three structural risks.

## Where I used it

Used to reverse-engineer the architecture of [codex-tldraw-mcp](https://github.com/jananadiw/codex-tldraw-mcp), a local MCP server that scans repositories and writes tldraw product workflow diagrams.

## Known limits

- The result is only as complete as the code and configuration available to the model.
- Runtime topology may live outside the repository in cloud settings, secrets, or manually managed infrastructure.
- Dynamic calls, generated code, framework conventions, and reflection can hide real edges.
- Large codebases may require several focused passes before the end-to-end trace is reliable.
- Mermaid shows structure clearly but does not replace deployment, sequence, or data-model views when those details matter.

## Last tested

2026-08-01, with [codex-tldraw-mcp](https://github.com/jananadiw/codex-tldraw-mcp).
