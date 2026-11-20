# Vault Maintenance Guidelines

## Security

- Do not introduce credentials, passwords, tokens, API keys, or other secrets.
- Use descriptive placeholders in examples.
- If you find a secret, report its location and type with the value masked.
- Obtain authorization before modifying existing credentials or placeholders.
- Obtain approval before running `git push` or publishing information.

## Organization

- Use Markdown that renders in both Obsidian and GitHub.
- Use relative links between documents and verify that each destination file and any referenced fragment exist.
- Reuse existing notes before creating new ones.
- Avoid unnecessary duplication and keep topic indexes up to date.
- Respect existing names, paths, and conventions.

## Documentation Quality

- Preserve all technical information, including commands, configurations,
  examples, tables, diagrams, and operational context.
- Base changes on the available material. Do not invent commands,
  configurations, or results.
- Preserve distinctions between versions, services, and certifications.
- Keep complementary material separate when it serves a different purpose.
- Propose destructive changes, moves, and consolidations and obtain approval
  before applying them.
- Verify that links still work after editing.

## Git

- Check `git status`, the current branch, and pending changes before editing.
- Preserve pre-existing changes.
- Work on a branch dedicated to the task.
- Keep changes small and reversible.
- Obtain authorization before committing or running `git push`.

## Responses

- Respond to the user in Spanish unless they request another language.
- Use Markdown that is readable in a terminal.
- Avoid wide tables.
- Present summaries and prioritize findings.
- Identify affected files and report the validation performed.
