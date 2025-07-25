# Hooks vs Subagents: The Ultimate Decision Guide

## 🎯 Core Principle
**Hooks** = Enforcement & Automation (deterministic)  
**Subagents** = Intelligence & Analysis (reasoning)

## 🔍 Quick Decision Framework

Ask yourself these questions in order:

1. **Does it require understanding context or making judgment calls?**  
   → YES: Subagent  
   → NO: Continue to #2

2. **Can it be defined as simple if/then rules?**  
   → YES: Hook  
   → NO: Subagent

3. **Should it block or modify actions automatically?**  
   → YES: Hook  
   → NO: Subagent

4. **Does it need to generate creative output?**  
   → YES: Subagent  
   → NO: Hook

## 📋 When to Use Hooks

### Perfect For:
- **Enforcement**: Rules that must never be broken
- **Validation**: Checking against defined criteria  
- **Automation**: Repetitive tasks with clear triggers
- **Blocking**: Preventing specific actions
- **Formatting**: Enforcing consistent patterns

### Examples:

```markdown
✅ HOOK: Commit Message Validator
- Trigger: On commit
- Logic: Check format matches "type: description"
- Action: Block if invalid

✅ HOOK: Security Scanner
- Trigger: On file write containing "password" or "secret"
- Logic: Regex pattern matching
- Action: Block and alert

✅ HOOK: Auto-formatter
- Trigger: On save of .js/.ts files
- Logic: Run prettier
- Action: Modify file

✅ HOOK: Test Runner
- Trigger: On file change in src/
- Logic: Run related tests
- Action: Display results

✅ HOOK: Branch Protection
- Trigger: On push to main
- Logic: Check if current branch == main
- Action: Block with error
```

## 🤖 When to Use Subagents

### Perfect For:
- **Analysis**: Understanding complex code patterns
- **Generation**: Creating boilerplate, tests, documentation
- **Research**: Finding best practices, examples
- **Refactoring**: Identifying improvement opportunities
- **Review**: Providing contextual feedback

### Examples:

```markdown
✨ SUBAGENT: API Design Consultant
- Task: Review REST endpoints for consistency
- Reasoning: Understands REST principles, naming conventions
- Output: Suggestions for improvement

✨ SUBAGENT: Test Scenario Generator  
- Task: Create edge case tests from requirements
- Reasoning: Analyzes business logic, identifies boundaries
- Output: Comprehensive test suite

✨ SUBAGENT: Performance Analyst
- Task: Find bottlenecks in codebase
- Reasoning: Understands O(n) complexity, database patterns
- Output: Optimization recommendations

✨ SUBAGENT: Documentation Writer
- Task: Generate API docs from code
- Reasoning: Infers purpose from implementation
- Output: Human-readable documentation

✨ SUBAGENT: Migration Planner
- Task: Plan upgrade from library v1 to v2
- Reasoning: Understands breaking changes, dependencies
- Output: Step-by-step migration guide
```

## 🔄 Hybrid Patterns

Sometimes you need both:

```markdown
🎭 EXAMPLE: Comprehensive Code Review
- HOOK: Block commits with console.log
- HOOK: Enforce eslint rules
- SUBAGENT: Review architecture decisions
- SUBAGENT: Suggest performance improvements

🎭 EXAMPLE: Test-Driven Development
- HOOK: Block implementation without failing tests (tdd-guard)
- SUBAGENT: Generate comprehensive test scenarios
- HOOK: Auto-run tests on save
- SUBAGENT: Suggest refactoring opportunities

🎭 EXAMPLE: Security Workflow
- HOOK: Block commits with hardcoded secrets
- HOOK: Enforce dependency scanning
- SUBAGENT: Analyze authentication flow
- SUBAGENT: Review security best practices
```

## ⚡ Quick Reference Table

| Scenario | Hook | Subagent | Why |
|----------|------|----------|-----|
| Block commits without tests | ✅ | ❌ | Simple rule enforcement |
| Generate test cases from story | ❌ | ✅ | Requires understanding |
| Format code on save | ✅ | ❌ | Deterministic transformation |
| Review code architecture | ❌ | ✅ | Needs context & judgment |
| Prevent push to main | ✅ | ❌ | Simple condition check |
| Optimize SQL queries | ❌ | ✅ | Requires analysis |
| Add ticket # to commits | ✅ | ❌ | Pattern matching |
| Explain legacy code | ❌ | ✅ | Needs comprehension |

## 🎯 Decision Checklist

When you have a new automation idea, ask:

1. **Can a regex handle it?** → Hook
2. **Does it need to understand meaning?** → Subagent
3. **Is the output always the same for the same input?** → Hook
4. **Does it require creativity or judgment?** → Subagent
5. **Should it happen automatically without asking?** → Hook
6. **Does it need to explain its reasoning?** → Subagent

## 💡 Pro Tips

1. **Start with hooks** for workflow enforcement
2. **Add subagents** for intelligence layers
3. **Combine both** for comprehensive workflows
4. **Hooks are free** (no tokens), subagents cost tokens
5. **Hooks are instant**, subagents need thinking time
6. **Version control both** in your project

Remember: Hooks are your **guardrails**, subagents are your **copilots**.