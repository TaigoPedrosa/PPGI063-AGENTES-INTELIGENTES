# Conventional Commit Skill

SKILL.md

```
---
name: conventional-commit
description: Specifications to follow when committing changes. Always use when saving progress. 
---

# Conventional Commit Skill

Every time you need to commit changes, use this skill to analyze the staged changes and generate a professional commit message following the Conventional Commits specification, including an architectural impact assessment.

1.  **Diff Analysis**: Run `git status && git diff` to check the current changes. If nothing is staged, add the relevant files.
2.  **Classification**: Identify the type of change:
    * `feat`: New feature.
    * `fix`: Bug fix.
    * `refactor`: A code change that neither fixes a bug nor adds a feature.
    * `test`: Adding missing tests or correcting existing tests.
    * `chore`: Changes to the build process or auxiliary tools and libraries.
    * `docs`: Documentation only changes.
    * `style`: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc).
    * `perf`: A code change that improves performance.
    * `ci`: Changes to CI configuration files and scripts.
    * Other types can be used as needed, but these are the most common ones.
3.  **Branch validation**: Ensure you are on a proper branch, and not on a protected one like `main`, `master` or `develop`. If you are on a protected branch, change to a new branch before proceeding. Use the format `<type>/<short-description>`, e.g., `feat/add-user-authentication`.
4.  **Title**: Create a concise title: `<type>[scope]: <short description in English>`. For example: `feat(api): add new endpoint for user authentication`. The scope is optional but recommended for clarity. Check the complete [Conventional Commits Specification](specification.md) for more details on formatting.
5.  **Body**: List the relevant changes in bullet points.
6.  **Impact Assessment**: Add a brief section called "Impact Assessment", describing whether the change affects stability, performance or other modules.
7.  **Footer**: Do not include any reference to the commit being co-authored by any AI Agent (e.g. Claude Code).
8.  **Confirmation**: Present the final message to the user and ask: "Would you like to commit with this message?".
```

specification.md

```markdown
The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”, “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in this document are to be interpreted as described in RFC 2119.

1. Commits MUST be prefixed with a type, which consists of a noun, feat, fix, etc., followed by the OPTIONAL scope, OPTIONAL !, and REQUIRED terminal colon and space.
2. The type feat MUST be used when a commit adds a new feature to your application or library.
3. The type fix MUST be used when a commit represents a bug fix for your application.
4. A scope MAY be provided after a type. A scope MUST consist of a noun describing a section of the codebase surrounded by parenthesis, e.g., fix(parser):
5. A description MUST immediately follow the colon and space after the type/scope prefix. The description is a short summary of the code changes, e.g., fix: array parsing issue when multiple spaces were contained in string.
6. A longer commit body MAY be provided after the short description, providing additional contextual information about the code changes. The body MUST begin one blank line after the description.
7. A commit body is free-form and MAY consist of any number of newline separated paragraphs.
8. One or more footers MAY be provided one blank line after the body. Each footer MUST consist of a word token, followed by either a :<space> or <space># separator, followed by a string value (this is inspired by the git trailer convention).
9. A footer’s token MUST use - in place of whitespace characters, e.g., Acked-by (this helps differentiate the footer section from a multi-paragraph body). An exception is made for BREAKING CHANGE, which MAY also be used as a token.
10. A footer’s value MAY contain spaces and newlines, and parsing MUST terminate when the next valid footer token/separator pair is observed.
11. Breaking changes MUST be indicated in the type/scope prefix of a commit, or as an entry in the footer.
12. If included as a footer, a breaking change MUST consist of the uppercase text BREAKING CHANGE, followed by a colon, space, and description, e.g., BREAKING CHANGE: environment variables now take precedence over config files.
13. If included in the type/scope prefix, breaking changes MUST be indicated by a ! immediately before the :. If ! is used, BREAKING CHANGE: MAY be omitted from the footer section, and the commit description SHALL be used to describe the breaking change.
14. Types other than feat and fix MAY be used in your commit messages, e.g., docs: update ref docs.
15. The units of information that make up Conventional Commits MUST NOT be treated as case-sensitive by implementors, with the exception of BREAKING CHANGE which MUST be uppercase.
16. BREAKING-CHANGE MUST be synonymous with BREAKING CHANGE, when used as a token in a footer.

```
