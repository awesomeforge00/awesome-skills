# Awesome Skills

A growing collection of reusable skills for AI coding assistants and agentic development workflows.

`awesome-skills` helps you give agents focused, repeatable expertise for planning, coding, refactoring, testing, and more. Each skill is a small Markdown-based instruction package that can be adapted to the agent or coding tool you use.

[![GitHub stars](https://img.shields.io/github/stars/awesomeforge00/awesome-skills?style=flat-square)](https://github.com/awesomeforge00/awesome-skills/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/awesomeforge00/awesome-skills?style=flat-square)](https://github.com/awesomeforge00/awesome-skills/network/members)
[![GitHub issues](https://img.shields.io/github/issues/awesomeforge00/awesome-skills?style=flat-square)](https://github.com/awesomeforge00/awesome-skills/issues)

## Why use agent skills?

General-purpose AI agents are capable, but they often need explicit context to produce consistent results. A focused skill can provide:

- A clear operating procedure for a recurring task
- Domain-specific rules, checks, and decision criteria
- Supporting reference material loaded only when needed
- A portable format that is easy to read, review, and customize

Use these skills with AI coding assistants such as Claude Code, GitHub Copilot, OpenAI Codex, Antigravity, and other agentic tools that support Markdown instructions or skill files.

## Skill Directory

### Planning and decision-making

- [Co-Plan](coplan/) — Ask focused questions to stress-test a plan, decision, or technical choice before implementation.

### Python development

- [Python Refactor & Standards](python-refactor-and-standards/) — Write and refactor Python code to be clean, safe, well-typed, and testable without changing behavior.

More skills are added regularly. Browse the repository directories for the latest collection.

## How skills are structured

A skill normally contains a `SKILL.md` file with:

1. YAML frontmatter describing the skill and when to use it
2. Clear instructions for the agent
3. Optional reference files for deeper, topic-specific guidance

Example:

```text
my-skill/
├── SKILL.md
├── README.md
└── reference/
    └── topic.md
```

Keeping the main skill file concise makes it easier for an agent to load the right context and easier for people to review and improve.

## Use a skill with your agent

The exact installation path depends on the agent or editor. The general workflow is:

1. Choose a skill directory from the [Skill Directory](#skill-directory).
2. Read its `SKILL.md` and any relevant reference files.
3. Copy or link the skill into the instruction or skills directory used by your agent.
4. Invoke the skill when its description matches the task.
5. Customize the wording and references for your team's workflow when needed.

Because agent configuration conventions change between tools, always check the current documentation for your chosen client. The files in this repository are plain text and can also be used as a starting point for project-level instructions.

## Example prompts

After adding a skill to your agent's available instructions, prompts like these can activate the relevant workflow:

```text
Use the Python Refactor & Standards skill to review this module and propose a behavior-preserving refactor.
```

```text
Use the Co-Plan skill to help me resolve the architectural decisions for this feature before writing code.
```

## Contributing a skill

Contributions are welcome. A useful skill should be focused, reusable, and specific about when it applies.

1. Create a directory with a short, descriptive name.
2. Add a `SKILL.md` with valid frontmatter, a concise description, and practical instructions.
3. Add a `README.md` explaining the skill's purpose and structure.
4. Put detailed, optional material in a `reference/` directory when appropriate.
5. Test the skill with at least one realistic agent workflow.
6. Open a pull request describing the problem the skill solves and how to use it.

Please keep skills composable, avoid duplicating general agent behavior, and document assumptions or tool-specific limitations.

## Design principles

- **Focused:** one skill should solve one coherent class of problems.
- **Actionable:** instructions should guide an agent's next decisions and actions.
- **Portable:** use plain Markdown and avoid unnecessary tool-specific dependencies.
- **Progressive:** keep core instructions short and move deep guidance into references.
- **Reviewable:** make rules, scope, and stopping conditions easy for humans to inspect.

## Roadmap

- Expand the collection across software engineering, product, research, and operations workflows
- Add more skills for testing, debugging, documentation, security, and code review
- Improve cross-agent installation examples as tool conventions evolve
- Collect community feedback and real-world usage patterns

## Stay updated

Star the repository to follow new skills and improvements:

https://github.com/awesomeforge00/awesome-skills

## License

No license has been specified for this repository yet. Until a license is added, treat the contents as all rights reserved and ask before redistributing them.
