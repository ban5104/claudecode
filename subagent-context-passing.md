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

## The Context Degradation Solution

### The Problem
As conversations grow longer, early context gets buried:
- Messages 1-3: Agent reads important config files
- Messages 4-8: Discussion continues
- Message 9: Need something from those early files
- Result: Details are forgotten or deprioritized

### The Insight
Subagents provide "fresh context windows" that solve this problem:
- **Fresh start** - New context window, not buried under conversation history
- **Curated relevance** - Only files that were important enough to read
- **Focused attention** - Subagent sees ONLY relevant files + specific task
- **Perfect recall** - Early file details are as fresh as recent ones

## Effective Workflow Pattern

### Best Practice: Hybrid Approach

```yaml
description: "Review code changes. Always include:
1. Any project standards files (STANDARDS.md, CONTRIBUTING.md, etc)  
2. Config files the parent has seen (.eslintrc, .prettierrc, tsconfig)
3. Example code files that demonstrate project patterns
4. All code files that need review
If no explicit standards exist, analyze 2-3 existing files to infer patterns"
```

### Why This Works
- Leverages exploration already done in main conversation
- No redundant file reading or token waste
- Combines explicit standards with discovered patterns
- Subagent gets concentrated, relevant context

## Key Distinctions

### Description vs System Prompt
- **System Prompt**: Defines the subagent's role and approach
- **Description**: Controls what context flows from parent → subagent
- **Together**: Role + Context = Optimal Performance

### Context Passing vs Self-Discovery
- **With Description**: "Here's CLAUDE.md content we discussed + insights"
- **Without Context**: "Go find and read CLAUDE.md yourself"

## Practical Benefits

1. **Combat Context Degradation** - Fresh windows with curated context
2. **Efficient Token Usage** - No re-reading files already explored
3. **Preserve Insights** - Conversation learnings transfer to subagents
4. **Flexible Control** - Natural language instructions for context flow

This pattern essentially uses subagents as "context-aware specialists" that receive a fresh, focused view of exactly what matters for their specific task.