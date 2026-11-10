# Personal AI Archive

An editable, Git-backed personal memory archive designed to be readable by AI assistants across devices and platforms.

## Repository map

- `identity/`: user-approved identity, work, values, and long-term goals.
- `workflow/`: tools, communication preferences, and task rules.
- `memory/`: durable preferences, decisions, and lessons from completed work.
- `skills/`: current skills, evidence, and learning goals.
- `archive/`: completed projects and historical material.
- [`ai-integration.md`](ai-integration.md): connection and write-back guidance for AI tools.

All profile files are plain UTF-8 Markdown. Replace `{{placeholders}}` with facts you want to retain. Keep this public repository free of secrets, private credentials, sensitive personal data, and confidential work information.

## Quick start

1. Edit the Markdown files in the relevant directory.
2. Commit each meaningful update with a concise message.
3. To undo an update, use GitHub's file history or revert the commit. Do not rewrite shared history with force-push.
4. Connect an AI tool using the instructions in `ai-integration.md`.

## Update policy

AI assistants may propose and, when explicitly authorized by the connected integration, commit durable non-sensitive preferences and lessons. They should preserve existing facts, avoid copying entire conversations, and never infer personal facts from uncertain context. See `ai-integration.md` for the write-back protocol.
