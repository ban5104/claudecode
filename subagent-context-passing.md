# Subagent Context Passing in Claude Code

## Key Discovery: Context Control Through Description Fields

### Official Explanation from Claude Code Team

> "Controlling Context Passage: You can specify in the description what context you want the sub-agent to receive. For example, if you want the sub-agent to always get the file paths of all files the parent agent has, you would specify that in the description, and the parent agent will follow that instruction when it calls the sub-agent. This gives users a lot of control over both what goes into the sub-agent and what comes out, simply by prompting."

## How It Works

The `description` field in subagents serves dual purposes:
1. **Determines WHEN to use the subagent** - Pattern matching for task delegation
2. **Controls WHAT context to pass** - Instructions to parent about information transfer

### Example Implementation

```python
Task(
    description="Review code changes. Always include CLAUDE.md, any style guides, config files, and all files the parent agent has examined",
    prompt="Review the recent changes for code quality",
    subagent_type="code-reviewer"
)
```

The parent agent:
- Reads the description instructions
- Gathers relevant content from the main conversation
- Passes both the task AND gathered context to the subagent

## Full Control Over Inputs and Outputs

Users have significant control over both what goes into sub-agents and what comes out:

### Controlling Inputs (Context Management)
- The parent agent decides when to use sub-agents and what context to pass based on the conversation
- Users directly influence this through the **description field**
- The description tells the parent agent:
  - When to call the sub-agent
  - How to call the sub-agent
  - What context to pass into it
- Example: Specifying that a sub-agent should "always receive the file paths of all files the parent agent has"

### Controlling Outputs (Reporting Back)
- Sub-agents determine what information to return when reporting back to the parent
- This is guided by the **system prompt** you define for the sub-agent
- You control what gets passed out of the sub-agent back to the parent thread through prompting

**Key Insight:** The description and system prompt fields work together - description controls information flow IN, system prompt controls information flow OUT, giving users precise control over sub-agent behavior.
