## Skills vs Agents — mental model

- **Skill = Recipe.** A SKILL.md is only instructions and knowledge: "what to do and how to do it." It never runs by itself.
- **Agent = Chef.** An agent does the actual work, with its own context (memory), tools and model.
- **Loading a skill does not create a new agent.** The same agent puts on the skill's "hat" and follows it. Java analogy: skill = Strategy class, agent = the thread that executes it.
- **One-to-many both ways:** one skill can be used by many agents, and one agent can use many skills.
- **When "each skill = an agent" becomes true:** wrap a skill inside a dedicated subagent (or a skill that runs in a forked context). Then it behaves like a separate agent.

## Parallel agents and file clashes

- Skills loaded in the main agent run one after another, so files don't clash.
- Skills wrapped as subagents and run in parallel can cause race conditions and lost updates: two agents read the same file, both write, and the last write wins silently.
- **Fixes:**
  1. Give each agent its own files.
  2. Use git worktree isolation per agent, then merge.
  3. Run as many readers in parallel as you like, but only one writer (the orchestrator).
  4. Have each agent write to its own output file; the orchestrator merges them.
  5. Make shared files append-only, use a lock file, or use SQLite.
  6. Run dependent tasks one after another.
- Spring Boot analogy: like threads changing a shared object without a lock. The fix is ownership, isolation, or a single write path.

**One-liner:** Skill = what and how. Agent = who does it, and in which context.
