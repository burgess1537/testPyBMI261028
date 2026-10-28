---
name: ready-items-user-stories
description: Write or complete User Story and Acceptance Criteria for GitHub Project items in the Ready status, using each item's title and preserving existing description content.
---

# Ready items: User Story and Acceptance Criteria

Use this skill when asked to draft or complete User Story and Acceptance Criteria for work items in a GitHub Projects board's **Ready** column.

## Workflow

1. Identify the GitHub Project from the user's link or instructions. Resolve its owner and project number; if either is ambiguous, ask before making changes.
2. List the project's items and statuses with `gh project item-list <project-number> --owner <owner> --limit 1000 --format json`. Check `totalCount` in the JSON output; if fewer items were returned than the total, rerun with a higher `--limit` until all items are included.
3. Process only items whose status is exactly `Ready`. Do not change their status or any other project fields.
4. For each Ready item, inspect its current description/body. For linked issues, use `gh issue view <number> --repo <owner>/<repo> --json title,body,url`; for pull requests, use `gh pr view <number> --repo <owner>/<repo> --json title,body,url`.
5. Treat `User Story` and `Acceptance Criteria` as separate required sections. If both already contain meaningful content, leave the item unchanged. If one or both are missing or empty, draft only the missing section(s), drawing requirements from the title and any existing description. Preserve all existing text and sections.
6. Write in the language of the item's title, unless the user asks for another language. Keep the story in a concise “As a [persona], I want [capability], so that [benefit]” form. Make acceptance criteria observable and testable; do not invent product behavior, integrations, policies, or implementation details that the title and description do not support.
7. Update the linked item's body with the completed description (`gh issue edit` or `gh pr edit`, using a UTF-8 body file where needed to preserve non-ASCII text). Do not replace existing content with only the newly drafted sections.
8. Re-read each changed item and verify both sections are present and the original description content remains intact. Report which Ready items were updated and which were skipped, including why.

## Safety and scope

- Never edit items outside the Ready status.
- Never overwrite meaningful existing User Story or Acceptance Criteria content. Complete only missing sections.
- If an item is not a linked issue or pull request (for example, a draft project item), do not silently skip or attempt an unsupported mutation. Explain the limitation and ask how the user wants it handled.
- If an item's title and description do not provide enough information to write useful, non-speculative criteria, ask a focused clarification rather than fabricating requirements.
- Do not create a commit or change the project's status as part of this workflow.
