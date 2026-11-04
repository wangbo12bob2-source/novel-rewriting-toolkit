# Novel Rewriting Toolkit

An agent-executed skill for analyzing, redesigning, writing, reviewing, and revising Chinese novels and serialized screenplays.

[Skill entry](SKILL.md) · [Chinese guide](README_CN.md) · [MIT license](LICENSE)

The repository contains instructions and references, not a Python rewriting engine or hosted generation service. It preserves the upstream distill → restructure → write approach while adding format routing, internal story probes, continuity tracking, and editorial review.

## Use

Make this skill directory available to your agent and ask it to read SKILL.md. Keep source manuscripts and generated work in a separate project directory. State the source path, format, intended genre, and authorized stage when they are not already known.

For example: “Analyze this Chinese short-drama DOCX and propose rewrite directions; do not write the screenplay yet.” Later: “Continue the selected direction and finish the full screenplay, including natural-language revision.”

New instructions update the authorized scope. Internal probes and review do not introduce another approval gate after full writing is already authorized. An analysis-only request must remain analysis-only.

## Workflow

1. Recover the current project agreement, scope, and version.
2. Read and distill the source, preserving the original and import anomalies.
3. Design new motivations, relationships, causal structure, names, and ending.
4. Probe the opening, contrasting units, and important confrontations before expanding.
5. Write and review meaningful story segments; track actual continuity changes.
6. Revise from specific findings, preserve versions, export, and read back the output.

Read only the relevant references: project setup, story design, screenplay, novel, editorial review, and state/delivery. Existing MANUAL_*.md filenames remain compatible stage entry points.

## Editorial changes

- No universal two-to-one chapter merging, fixed chapter length, mandatory world escalation, or sentence-length quotas.
- Natural prose is evaluated through character goals, dialogue, scene function, and repetition; ordinary words and punctuation are not globally banned.
- Review checks why the opening arrangement is necessary, whether units repeat the same solution, whether opponents act, and whether promised answers actually appear in the text.
- Completion, structural validation, and editorial judgment are reported separately. A complete, structurally valid draft can still require major revision.

These changes were informed by a hundred-episode writing trial and subsequent editorial findings. They are not evidence of audience performance or comparative model quality. Private trial manuscripts are not included. See the synthetic [behavioral regression cases](references/regression-cases.md) for maintenance checks.

## Tools and boundaries

The repository does not bundle an importer, model API, naming checker, originality detector, or document exporter. Use tools actually available in the execution environment; create small project-local helpers only when necessary and validate them before reporting their results. Python is optional for such helpers, not the skill runtime.

Do not equate renamed entities, word counts, or successful exports with originality, detection scores, legal guarantees, production readiness, or reader approval. DOCX extraction and export require suitable tools; validate complete exported text by reading it back.

Source material, outputs, private samples, credentials, and project state stay outside the skill repository. External uploads, paid calls, publication, and additional agents require authorization. The .gitignore file is a safeguard, not a substitute for inspecting staged files.

Upstream history and the MIT license are retained. Natural-language editing draws on Humanizer-zh and the original writing materials, adapted to fictional narrative rather than applied as generic nonfiction rules.
