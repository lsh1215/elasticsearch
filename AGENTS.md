# Elasticsearch Study Operating Guide

This repository is a study workspace for Elasticsearch and OpenSearch. It stores local experiments, concept notes, deep dives, and reusable knowledge gathered through ongoing conversations.

## Answering Style

When answering study questions, prefer root-cause exploration over surface-level definitions.

Use this sequence when it fits the question:

1. The original problem this technology tried to solve
2. Earlier approaches and their limits
3. The core abstraction or idea introduced by the technology
4. How it works today
5. A small way to verify it through practice
6. Official documentation or reliable references

Do not force the full structure for tiny questions. Keep short answers short, but preserve the root-cause perspective.

## Research Standard

- Prefer official docs, source code, project documentation, standards, papers, or reputable engineering writeups.
- When current facts may have changed, verify with up-to-date sources before answering.
- Separate confirmed facts from inference.
- Keep references in concept and deep-dive documents when a claim should be traceable.

## Documentation Map

Use these paths consistently:

- `docs/study-log/`: Daily study flow. Capture meaningful questions, answer summaries, experiments, unresolved points, and next study candidates by date.
- `docs/concepts/`: Final concept notes. Include root-cause exploration inside the concept document rather than splitting it into a separate research directory.
- `docs/deep-dives/`: Broad questions, comparisons, history, and design background that cross multiple concepts.
- `docs/labs/`: Practice records. Include commands, outputs, errors, fixes, and lessons learned.
- `omx_wiki/`: AI-facing reusable knowledge. Store distilled knowledge that Codex should be able to retrieve later. The user creates and manages this directory directly with Obsidian; do not create the directory unless the user explicitly asks.

## Concept Document Template

Use this shape for `docs/concepts/topic-name.md`:

```markdown
# Concept Name

## 해결하려던 문제

## 기존 방식의 한계

## 핵심 아이디어

## 동작 방식

## 실습으로 확인하기

## 자주 헷갈리는 점

## References
```

## Wiki Policy

Use `omx_wiki/` for long-lived reusable knowledge only. The directory may not exist in Git until the user creates it from Obsidian.

Good wiki material:

- Core definitions
- Why a technology appeared
- Final understanding of confusing concepts
- Facts confirmed by official docs or reliable sources
- Reusable summaries from `docs/concepts/` or `docs/deep-dives/`

Do not put these in the wiki:

- Raw conversation transcripts
- Full daily study logs
- Raw lab logs
- Unverified speculation

## Daily Wrap-Up

At the end of a study day, ask whether to summarize the day.

When approved:

1. Create or update `docs/study-log/YYYY-MM-DD.md`.
2. Promote stable concepts into `docs/concepts/`.
3. Promote broad cross-concept questions into `docs/deep-dives/`.
4. Record practice work in `docs/labs/`.
5. Ingest only reusable, verified knowledge into `omx_wiki/` when the directory exists or the user asks to create it.
6. If files changed, create a branch, commit, and PR.
7. Do not auto-merge daily wrap-up PRs unless explicitly requested.

## Git Workflow

- Use `codex/` as the default branch prefix.
- Keep study-document changes focused and reviewable.
- Prefer PR review before merging daily summaries.
