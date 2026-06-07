# Code Teacher Skill

A reusable teaching skill for AI coding agents and command-line assistants.

The skill helps developers understand code while an AI assistant inspects, explains, debugs, reviews, or modifies a project. Instead of only producing a solution, it guides the learner through the problem, root cause, execution flow, design decisions, edge cases, testing, debugging, and broader application impact.

It is intentionally platform-neutral and can be used with compatible tools that support reusable agent skills or `SKILL.md`-based instructions.

## What this skill does

When explicitly invoked, `code-teacher` turns a normal coding task into an interactive learning session.

It helps the learner:

- Understand existing code before changing it
- Find the root cause of a bug instead of only applying a patch
- Trace requests, data, and side effects through an application
- Understand business logic and architectural decisions
- Compare reasonable implementation approaches and tradeoffs
- Identify important branches, edge cases, and failure scenarios
- Learn how to test and debug the implementation
- Build enough understanding to safely modify similar code independently

The skill instructs the AI assistant to ask focused questions and wait for the learner's response before continuing.

## Interactive teaching behavior

The skill is designed to work incrementally.

When the assistant asks the learner to:

- Restate their current understanding
- Answer a quiz
- Predict code behavior
- Trace an execution path
- Identify an edge case
- Use logs, tests, or a debugger
- Complete a small exercise

the assistant should stop and wait for the learner's next response.

It should not continue the lesson or answer its own question in the same message.

### When the learner answers correctly

The assistant should:

1. Confirm specifically what was correct
2. Add any important missing detail
3. Update the mastery checklist
4. Continue to the next learning stage

### When the learner answers partially correctly

The assistant should:

1. Identify what was correct
2. Explain the missing or inaccurate part
3. Use relevant code, an example, trace, or analogy
4. Ask a smaller follow-up question
5. Stop and wait again

### When the learner answers incorrectly

The assistant should:

1. Clearly and respectfully state that the answer is incorrect
2. Provide the correct answer
3. Explain why the correct answer is correct
4. Explain the misunderstanding
5. Show relevant code, logs, queries, tests, or debugger evidence
6. Ask a new verification question
7. Stop and wait before proceeding

## Teaching workflow

The skill guides the learner through these stages:

1. Establish the learner's current understanding
2. Understand the problem and visible symptom
3. Identify the root cause
4. Map the relevant files and execution flow
5. Trace a realistic example
6. Explain the proposed solution and tradeoffs
7. Implement the requested change when applicable
8. Explore relevant edge cases
9. Explain testing and debugging
10. Review the broader application impact
11. Perform a final mastery check

The session is considered complete only after the learner demonstrates practical understanding of the relevant checklist items.

## Learning document

The skill maintains a running Markdown learning document inside the current project:

```text
docs/learning-session.md
```

It may contain:

- The topic and learning objective
- Current understanding
- Relevant files
- Execution flow
- Root cause
- Solution
- Design decisions and tradeoffs
- Edge cases
- Testing and debugging notes
- Broader application impact
- Remaining knowledge gaps
- A mastery checklist
- The learner's final explanation

Checklist items should be marked complete only after the learner demonstrates understanding.

## Repository structure

```text
code-teacher-skill/
├── SKILL.md
├── README.md
└── LICENSE
```

## Compatibility

This repository is designed for AI coding tools that support one of the following:

- Reusable agent skills
- A `SKILL.md` instruction file
- Global or project-level skill directories
- Explicit skill invocation from a prompt

Directory locations and invocation syntax vary between tools.

Check your tool's documentation to determine:

1. Where global skills should be installed
2. Whether the skill folder must have a particular name
3. How a skill is invoked
4. Whether the CLI must be restarted after installation

The skill folder should normally be named:

```text
code-teacher
```

and contain:

```text
code-teacher/
└── SKILL.md
```

## Generic installation

Clone the repository:

```bash
git clone https://github.com/TazShetu/code-teacher-skill.git
```

Then copy or link the repository, or at minimum its `SKILL.md`, into the global or project-level skill directory required by your AI coding tool.

A typical result looks like:

```text
<global-skills-directory>/
└── code-teacher/
    └── SKILL.md
```

Restart the CLI if it does not detect the newly installed skill automatically.

## Example: global installation on Windows

For tools that use a global `.agents/skills` directory:

```powershell
New-Item -ItemType Directory -Force "$HOME\.agents\skills"

git clone https://github.com/TazShetu/code-teacher-skill.git `
    "$HOME\.agents\skills\code-teacher"
```

The skill file will be located at:

```text
C:\Users\TazShetu\.agents\skills\code-teacher\SKILL.md
```

## Example: global installation on macOS or Linux

For tools that use a global `.agents/skills` directory:

```bash
mkdir -p "$HOME/.agents/skills"

git clone https://github.com/TazShetu/code-teacher-skill.git \
    "$HOME/.agents/skills/code-teacher"
```

The skill file will be located at:

```text
~/.agents/skills/code-teacher/SKILL.md
```

## Manual installation

1. Download `SKILL.md`.
2. Create a folder named `code-teacher` inside your tool's global or project-level skills directory.
3. Place `SKILL.md` inside that folder.
4. Restart the CLI when required.
5. Confirm that the skill appears in the tool's available skills.

## Usage

Write your normal coding prompt and invoke the skill using the syntax supported by your tool.

A common invocation style is to add the skill name at the bottom of the prompt:

```text
Fix the N+1 query issue when loading templates with fonts and categories.

Inspect the current relationships, identify the source of the repeated
queries, implement a readable framework-native solution, and add a test.

$code-teacher
```

The assistant should begin by asking about your current understanding and then wait for your answer.

For tasks where teaching mode is not needed, omit the skill invocation:

```text
Fix the typo in the validation message.
```

No permanent activation is required when the tool supports per-prompt skill invocation.

## Example interaction

```text
You:
Explain and fix the duplicate order creation issue.

$code-teacher
```

```text
Assistant:
Before we inspect the complete flow, what do you currently think causes
the duplicate orders, and under what condition do they appear?
```

The assistant should stop and wait for your response.

When your answer is incorrect, it should explain the correct reasoning and verify your understanding with another focused question before proceeding.

## Explanation levels

During a session, the learner can request:

- `ELI5` — a simple analogy with minimal terminology
- `ELI14` — a beginner-friendly technical explanation
- `ELII` — an explanation suitable for an intern or junior developer
- `Senior` — architecture, tradeoffs, performance, security, maintainability, and failure modes

After using an analogy, the assistant should reconnect it to the actual code.

## Laravel and Livewire guidance

The skill includes additional guidance for Laravel and Livewire projects, including:

- Routes and route model binding
- Middleware
- Form Requests and validation
- Policies and authorization
- Controllers and Livewire components
- Services and action classes
- Eloquent relationships and eager loading
- N+1 query detection
- Transactions
- Events, listeners, queues, retries, and idempotency
- Cache keys and invalidation
- API Resources
- Exceptions and configuration
- Feature and unit testing
- Livewire state, lifecycle hooks, hydration, events, and rerendering

The skill prefers readable, framework-native solutions and modern conventions.

## Updating

Pull the latest repository changes from the installed skill directory.

### Windows PowerShell

```powershell
git -C "$HOME\.agents\skills\code-teacher" pull
```

### macOS or Linux

```bash
git -C "$HOME/.agents/skills/code-teacher" pull
```

Restart the CLI if updated instructions are not detected automatically.

## Uninstalling

Remove the `code-teacher` folder from the skill directory used by your tool.

### Windows PowerShell example

```powershell
Remove-Item -Recurse -Force "$HOME\.agents\skills\code-teacher"
```

### macOS or Linux example

```bash
rm -rf "$HOME/.agents/skills/code-teacher"
```

## Important limitation

This skill provides strong behavioral instructions, but it is not a technically enforced state machine.

AI assistants may occasionally continue after asking a question. When that happens, remind the assistant:

```text
Follow the mandatory interaction rules in $code-teacher.
You asked a question, so stop and wait for my answer.
```

## Contributing

Contributions are welcome.

Useful improvements may include:

- Better mastery checks
- Additional debugging exercises
- Support for more languages and frameworks
- More effective question patterns
- Tool-specific installation examples
- Reduced repetition during long sessions

Keep changes practical, incremental, and focused on demonstrated understanding.

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

## Disclaimer

This is a community-created skill. It is not an official project of any AI tool vendor.
