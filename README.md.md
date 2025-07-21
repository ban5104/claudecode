## General Workflow Tips

* **Context** Create a `CLAUDE.md` file (gets brought into context AT THE START of your converstation) in your project root with:
    * Quick reference commands (e.g., "run tests: npm test", "start app: npm run dev")
    * Project quirks and warnings (e.g., "Don't modify config.json - it's auto-generated")
    * Your team's code preferences (e.g., "We use tabs not spaces", "Always add error handling")
    * Setup gotchas (e.g., "Must use Node 18+", "API key goes in .env file")
    * What files do what (e.g., "main logic is in src/app.js", "database queries in lib/db.js")
    * **Tip:** Press `#` to have Claude automatically add instructions to CLAUDE.md


* **Think Strategy:** For better, more thoughtful responses, ask Claude to think first:
    * `think` - Quick planning before acting
    * `think hard` - Detailed analysis for complex problems
    * `think harder` - Deep dive into tricky issues
    * `ultrathink` - Maximum depth (uses more tokens, costs more)

* **Context Management:** Use `/clear` consistently to manage and reset the conversation context.

* **Interrupt Controls:**
    * `Escape` - Interrupt Claude to redirect
    * `Double Escape` - Jump back in history

* **Custom Slash Commands:** Create custom commands to streamline repetitive tasks. Use `!` prefix to execute bash commands directly within slash commands.

* **Tool Permissions:** Use `/permissions` to manage Claude's tool access efficiently. or manually edit `settings.local.json`

* **Parallel Work:** Use git worktrees for working on multiple features simultaneously.

* **Testing Iterations (Docker):**
    For test iterations, especially within Docker containers, you can use:
    
    ```bash
    claude --dangerously-skip-permissions
    ```


---

## Quick Tips

* **Be specific:** Claude performs best with clear targets (visual mock, test case, or expected output)
* **Course correct early:** Small adjustments early save time later
* **Use visuals:** Screenshots and design mocks greatly improve UI implementation
* **Reference files:** Use tab-completion to quickly mention specific files Claude should look at
* **Share URLs:** Paste URLs directly in prompts for Claude to fetch and read documentation
* **Leverage subagents:** Break complex problems into focused subtasks

---

## Workflows

*The beauty of Claude Code is you're not limited to just one approach—combine workflows as needed. Use Workflow A's exploration for research, then switch to B for implementation, or use C for UI polish.*

## A. Explore → Plan → Code → Commit

**Best for:** New features, integration work, debugging mysteries, learning unfamiliar codebases.

**When you don't know exactly what to build or how it fits into existing code.**

### Steps:

1. **Explore**: `"Read auth-related files. Use subagents to understand token flow. Don't code yet—just explain."`

2. **Plan**: `"Think hard about adding 2FA. Create a plan with: integration approach, database changes, API modifications, acceptance criteria. Save as markdown."`

3. **Code**: `"Implement the 2FA plan. Start with database schema changes."`

4. **Verify & Commit**: `"Create PR 'feat: add two-factor authentication', update README."`

**Perfect for:** Exploration-heavy work where you need to understand before building.

---

## B. Test-Driven Development (TDD)

**Best for:** Clear specifications, bug fixes with reproduction steps, API endpoints, refactoring with existing tests.

**When you have clear requirements and want bulletproof implementation.**

### Steps:

1. **Write Tests**: `"Generate test cases from acceptance criteria. TDD only—no implementation. Cover: setup, verification, edge cases."`

2. **Commit Tests**: `"Run tests to confirm they fail. Commit with: 'test: add TDD tests for user login validation'"`

3. **Code**: `"Implement to pass all tests. Don't modify tests. Focus on one test at a time."`

4. **Iterate**: `"Run tests. Fix failures. Iterate until all pass. Show output after changes."`

5. **Verify**: `"Use a subagent to verify: acceptance criteria met, no test overfitting, edge cases handled."`

6. **Commit**: `"Commit implementation, create PR."`

**Perfect for:** When you want rock-solid code with comprehensive test coverage.

---

## C. Visual Development

**Best for:** UI components, landing pages, email templates, dashboard layouts, any work where you need to SEE the result.

**For visual work where immediate feedback is crucial.**

### Steps:

1. **Initial Implementation**: `"Here's the design [attach image]. Implement rough version of this dashboard component."`

2. **Screenshot**: `"Take screenshot of current implementation."`

3. **Compare & Iterate**: `"Compare to mock. Adjust spacing and colors to match exactly. Screenshot after each change."`

4. **Polish**: `"Add hover states and animations shown in design. Make it feel premium."`

**Perfect for:** UI work requiring visual precision and immediate feedback loops.

---


