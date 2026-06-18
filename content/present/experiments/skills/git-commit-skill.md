# Git Commit Skill

SKILL.md

```
---
name: git-commit
description: Mandatory rules for committing changes. Always follow when using git commit. 
---

# Git Commit Skill
Every time you need to commit changes, use this skill to analyze the staged changes and generate a professional commit message.

1.  **Diff Analysis**: Run `git status && git diff` to check the current changes. If nothing is staged, add the relevant files.
2.  **Classification**: Identify the type of change. Only use the following types: FEAT for new additions, FIX for bug fixes and MISC for all other changes.
3. **Title and Description**: Follow the format: `[<TYPE>] <Short description in English>`. For example: `[FEAT] add new endpoint for user authentication`. The description should be concise and informative.
4. **Body and Footer**: The commit message should not include any content in the body or footer sections. Only the title is required.
5.  **Confirmation**: Present the final message to the user and ask: "Would you like to commit with this message?".
```
