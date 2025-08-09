# Agent Workflow Overview

## Core Development Workflow

Your development process follows a structured pipeline with specialized agents working in sequence:

### 1. **Feature Analyzer** (Opus) → Discovery Phase
- **Purpose**: Deep-dive analysis of existing codebase patterns
- **Examines**: Related files, project docs (CLAUDE.md, README.md), similar implementations, architectural patterns
- **Output**: Comprehensive analysis report with patterns, conventions, and recommendations
- **When to use**: First step for any new feature or major modification

### 2. **Code Architect** (Opus) → Planning Phase  
- **Purpose**: Transform requirements into actionable implementation plans
- **Input**: Feature Analyzer report + user requirements + technical constraints
- **Creates**: Detailed architectural design, component breakdown, API specs, data models, implementation sequence
- **When to use**: After feature analysis, before writing any code

### 3. **TDD Test Specialist** (Sonnet) → Test-First Phase
- **Purpose**: Create comprehensive failing tests that drive implementation
- **Process**: Analyze requirements → Generate test cases → Write failing tests → Run to verify failure → Commit tests
- **Focus**: Behavior-driven tests, edge cases, error handling, AAA pattern (Arrange, Act, Assert)
- **Output**: Complete test suite that defines expected behavior
- **When to use**: After architecture, before implementation (TDD workflow)

### 4. **Code Implementation** (Sonnet) → Execution Phase
- **Purpose**: Convert architectural plans into working code that passes tests
- **Follows**: Established patterns religiously - never creates new conventions
- **Process**: Foundation → Core Implementation → Integration → Make tests pass → Debug
- **When to use**: After tests are written (TDD) or after architectural planning (traditional)

### 5. **Code Reviewer** (Sonnet) → Quality Assurance Phase
- **Purpose**: Ensure code quality, security, and standards compliance
- **Checks**: 
  - Project standards (CLAUDE.md), linting rules, security vulnerabilities
  - Test coverage and quality
  - **TDD Overfitting**: Verifies implementation handles cases beyond test scenarios
- **Output**: Structured feedback by severity (Critical → Major → Minor → Suggestions)
- **When to use**: After initial implementation, especially critical in TDD to prevent overfitting

### 6. **Code Simplifier** (Sonnet) → Refinement Phase
- **Purpose**: Reduce complexity while maintaining functionality
- **Focus**: Readability, maintainability, DRY principles, reducing nesting
- **Constraint**: Never sacrifice correctness for simplicity
- **When to use**: After code review identifies complexity issues

### 7. **Test Runner** (Haiku) → Verification Phase
- **Purpose**: Execute tests and provide clear result summaries
- **Actions**: Discover test commands, run appropriate tests, report results
- **Output**: Structured test execution summary with pass/fail stats
- **When to use**: After any code changes, minimal context needed

## Workflow Patterns

### TDD Feature Development
```
Feature Analyzer → Code Architect → TDD Test Specialist → Code Implementation → Test Runner → Code Reviewer → Final Commit
                                           ↓                        ↓                              ↓
                                    (commits tests)          (iterates until pass)    (verify not overfitted)
                                                                                           ↓ (if complex)
                                                                                    Code Simplifier → Test Runner
```

**Key TDD Steps:**
1. TDD Test Specialist writes & commits failing tests
2. Code Implementation iterates until tests pass (no test modifications allowed)
3. Code Reviewer verifies implementation isn't overfitted to tests
4. Final commit only after overfitting check passes

### Traditional Feature Development (Test-After)
```
Feature Analyzer → Code Architect → Code Implementation → Write Tests → Test Runner → Code Reviewer
                                            ↓                    ↓            ↓              ↓ (if complex)
                                    (implement feature)    (add tests)  (verify pass)   Code Simplifier → Test Runner
```

Note: Tests can be written by:
- Code Implementation agent (includes tests with feature)
- TDD Test Specialist (used retroactively for comprehensive tests)
- Developer manually

### Bug Fixing Workflow (TDD)
```
Feature Analyzer (understand context) → TDD Test Specialist (regression tests) → Code Implementation (fix) → Test Runner → Code Reviewer
```

### Refactoring Workflow (TDD)
```
Feature Analyzer → Code Reviewer (identify issues) → TDD Test Specialist (safety tests) → Code Simplifier → Test Runner
```

## TDD vs Traditional Development

### Test-Driven Development (TDD)
- **Red**: Write failing tests first (TDD Test Specialist)
- **Green**: Implement code to make tests pass (Code Implementation)
- **Refactor**: Improve code while keeping tests green (Code Simplifier)
- **Benefits**: Better design, fewer bugs, tests serve as specification

### Traditional Development
- **Implement**: Write feature code first (Code Implementation)
- **Test**: Add tests to verify implementation works
- **Refactor**: Improve code with test safety net
- **Benefits**: Faster initial development, flexibility in approach

Both approaches require tests - the key difference is **when** tests are written.

### Preventing TDD Overfitting

In TDD, there's a risk of implementing code that ONLY handles the exact test cases. To prevent this:

1. **Code Implementation Rules**:
   - Cannot modify tests once committed
   - Must think beyond test scenarios
   - Should implement general solutions, not test-specific hacks

2. **Code Reviewer Checks**:
   - Does implementation handle edge cases not in tests?
   - Is the solution generalizable?
   - Are there hardcoded values matching test data?
   - Would similar inputs work correctly?

3. **Red Flags for Overfitting**:
   - Hardcoded test values in implementation
   - Switch statements matching exact test cases
   - Logic that only works for test inputs
   - Missing validation for untested scenarios

## Key Principles

1. **Agent Selection**:
   - **Opus** for complex analysis and planning (Feature Analyzer, Code Architect)
   - **Sonnet** for execution and review (TDD Test Specialist, Implementation, Review, Simplification)
   - **Haiku** for simple tasks (Test Runner)

2. **Context Passing**:
   - Always pass full content between agents, not just file names
   - Include all relevant files, docs, and previous findings
   - Preserve error messages and debugging context

3. **TDD Workflow**:
   - TDD Test Specialist writes failing tests FIRST and commits them
   - Code Implementation iterates until tests pass (NO test modifications)
   - Code Reviewer MUST verify implementation isn't overfitted to tests
   - Implementation should handle edge cases beyond test scenarios
   - Only commit implementation after overfitting check passes

4. **Pattern Adherence**:
   - Implementation agents NEVER create new patterns
   - Always follow existing conventions found by Feature Analyzer
   - When in doubt, copy from similar features

5. **Quality Gates**:
   - Tests must pass before considering task complete
   - Code review feedback should be addressed
   - Complexity issues trigger simplification phase

## Quick Reference

- **Starting a new feature?** → Feature Analyzer first
- **Have a plan ready?** → TDD Test Specialist (write tests first) → Code Implementation
- **Fixing a bug?** → TDD Test Specialist (regression tests) → Code Implementation
- **Code complete?** → Test Runner → Code Reviewer
- **Too complex?** → Code Simplifier
- **Just need to run tests?** → Test Runner (minimal context)
- **Want test-driven approach?** → Use TDD workflow

Remember: Each agent has a specific expertise. Use them in sequence for best results, and always pass complete context between them. Choose the workflow that best fits your current needs.
