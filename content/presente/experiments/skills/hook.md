# Hook

```python
#!/usr/bin/env python3
"""
PreToolUse hook to prevent dangerous git operations (force push and pushing protected branches).

This hook blocks:
- git push --force
- git push -f
- git push --force-with-lease
- Pushing to protected branches (main, master, develop)

Exit codes:
- 0: Allow the command to proceed
- 2: Block the command and show error message to Claude
"""

import json
import sys
import re
import subprocess

def get_current_branch():
    """Get the current git branch name."""
    try:
        result = subprocess.run(
            ["git", "rev-parse", "--abbrev-ref", "HEAD"],
            capture_output=True,
            text=True,
            check=True
        )
        return result.stdout.strip()
    except subprocess.CalledProcessError:
        return None

def block(reason: str):
    print(json.dumps({"decision": "block", "reason": reason}))
    sys.exit(2)

def main():
    input_data = json.load(sys.stdin)

    tool_name = input_data.get("tool_name", "")
    tool_input = input_data.get("tool_input", {})
    command = tool_input.get("command", "")

    if tool_name != "Bash":
        sys.exit(0)

    # Check for force push patterns
    force_push_patterns = [
        r'\bgit\s+push\s+.*--force\b',
        r'\bgit\s+push\s+.*-f\b',
        r'\bgit\s+push\s+.*--force-with-lease\b'
    ]

    for pattern in force_push_patterns:
        if re.search(pattern, command):
            block(
                "Force push is not permitted. Force pushing can overwrite history and cause data loss. "
                "If you really need to force push, do it manually in your terminal."
            )

    # Check for git push commands to protected branches
    if re.search(r'\bgit\s+push\b', command):
        current_branch = get_current_branch()
        protected_branches = ['main', 'master', 'develop']

        if current_branch in protected_branches:
            block(
                f"Cannot push directly to protected branch '{current_branch}'. "
                f"Protected branches: {', '.join(protected_branches)}. "
                "Create a feature branch and open a pull request instead, or do it manually."
            )

    sys.exit(0)

if __name__ == "__main__":
    main()

```
