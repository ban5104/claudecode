# Agent Decision Matrix

## QUICK AGENT SELECTION (Scan First)

| Scenario | Primary Agent | Follow-up Agent | Notes |
|----------|---------------|-----------------|-------|
| New feature start | `feature-analyzer` | `code-architect` | Always start here |
| Have architecture ready | `tdd-test-specialist` | `code-implementation` | TDD approach |
| Have architecture ready (fast) | `code-implementation` | `test-runner` | Traditional approach |
| Code written, needs review | `code-reviewer` | `code-simplifier` (if complex) | Quality gate |
| Tests need running | `test-runner` | - | Minimal context |
| Feature complete, no E2E tests | `e2e-test-discoverer` | `e2e-test-verifier` | Proactive testing |
| E2E tests exist, need verification | `e2e-test-verifier` | - | Quality gate |
| Bug reported | `feature-analyzer` | `tdd-test-specialist` | Understand → Test → Fix |
| Code too complex | `code-simplifier` | `test-runner` | Maintain functionality |

## AGENT ROSTER

### Analysis & Planning (Opus)
- **`feature-analyzer`**: Understand codebase patterns, existing implementations
- **`code-architect`**: Create implementation plans, design APIs, define architecture

### Implementation (Sonnet) 
- **`tdd-test-specialist`**: Write comprehensive failing tests first
- **`code-implementation`**: Write code following established patterns  
- **`code-reviewer`**: Quality assurance, security, standards compliance
- **`code-simplifier`**: Reduce complexity while maintaining functionality

### Testing & Verification
- **`test-runner`** (Haiku): Execute tests, report results (minimal context needed)
- **`e2e-test-discoverer`** (Multi-model): Autonomously generate E2E tests
- **`e2e-test-verifier`** (Multi-model): Verify E2E test quality (READ-ONLY)

## WORKFLOW DECISION TREE

### 1. STARTING POINT
```
New Task → What type?
├── Feature Development → Go to 2
├── Bug Fix → feature-analyzer → tdd-test-specialist → code-implementation
├── Refactoring → feature-analyzer → code-reviewer → tdd-test-specialist → code-simplifier  
└── Testing Only → Go to 4
```

### 2. FEATURE DEVELOPMENT
```
feature-analyzer → code-architect → Choose approach:
├── TDD (complex/unclear): tdd-test-specialist → code-implementation
└── Traditional (simple/known): code-implementation → tdd-test-specialist
```

### 3. POST-IMPLEMENTATION
```
Code Complete → test-runner → code-reviewer → Decision:
├── Issues Found → code-simplifier → test-runner
├── Production Critical → Create E2E Worktree (See Section 4)
└── Ready → Deploy
```

### 4. E2E TESTING (Parallel Development)
```
Create Worktree: git worktree add ../project-e2e -b feature/e2e-tests
├── e2e-test-discoverer (generate tests)
└── e2e-test-verifier (quality gate)
→ Merge back to main
```

## CRITICAL RULES

### TDD Workflow
1. **Tests First**: `tdd-test-specialist` writes & commits failing tests
2. **No Test Edits**: `code-implementation` cannot modify tests  
3. **Overfitting Check**: `code-reviewer` must verify implementation handles cases beyond tests
4. **Commit Order**: Tests committed first, implementation only after review passes

### Context Passing
- **Always pass FULL content** between agents, never just filenames
- **Include**: All relevant files, docs, error messages, previous findings
- **Preserve**: Error context, debugging information, architectural decisions

### Pattern Adherence  
- **Implementation agents**: NEVER create new patterns, always follow existing
- **When unsure**: Copy from similar features found by `feature-analyzer`
- **E2E tests**: Follow existing test patterns when available

### Quality Gates
- **All tests must pass** before considering complete
- **E2E Test Verifier** provides independent verification (cannot modify tests)
- **Code review feedback** must be addressed before final commit

## PARALLEL DEVELOPMENT

### Git Worktree Strategy
```bash
# Main feature development
git checkout -b feature/user-auth

# After core implementation complete:
git worktree add ../project-e2e -b feature/e2e-auth
cd ../project-e2e
# Run e2e-test-discoverer → e2e-test-verifier

# Integration
cd ../project  
git merge feature/e2e-auth
git worktree remove ../project-e2e
```

### Benefits
- **Non-blocking**: E2E testing in parallel with development
- **Isolated**: Separate environments prevent conflicts  
- **Quality**: Independent verification ensures reliability

## AGENT CAPABILITIES

### `feature-analyzer` (Opus)
- **Purpose**: Deep codebase analysis  
- **Examines**: Related files, docs, patterns, architecture
- **Output**: Comprehensive analysis with recommendations
- **Context**: Include project docs, similar features, architectural constraints

### `code-architect` (Opus)  
- **Purpose**: Transform requirements into implementation plans
- **Input**: Feature analyzer report + requirements + constraints
- **Creates**: Architecture, APIs, data models, implementation sequence
- **Context**: Full feature analysis, technical requirements, existing patterns

### `tdd-test-specialist` (Sonnet)
- **Purpose**: Write comprehensive failing tests that drive implementation  
- **Process**: Requirements → Test cases → Failing tests → Commit → Verify failure
- **Focus**: Behavior-driven, edge cases, AAA pattern
- **Context**: Architecture plans, feature requirements, existing test patterns

### `code-implementation` (Sonnet)
- **Purpose**: Write code following established patterns
- **Process**: Foundation → Implementation → Integration → Pass tests  
- **Rules**: Never create new patterns, follow existing conventions religiously
- **Context**: Architecture plans, test files, similar implementations, project patterns

### `code-reviewer` (Sonnet)  
- **Purpose**: Quality assurance and standards compliance
- **Checks**: Security, standards, test coverage, TDD overfitting prevention
- **Output**: Structured feedback (Critical → Major → Minor → Suggestions)
- **Context**: All code files, project standards, test files, implementation plans

### `code-simplifier` (Sonnet)
- **Purpose**: Reduce complexity while maintaining functionality
- **Focus**: Readability, DRY principles, reduced nesting
- **Constraint**: Never sacrifice correctness for simplicity  
- **Context**: Code review feedback, complexity metrics, test files

### `test-runner` (Haiku)
- **Purpose**: Execute tests and report results
- **Actions**: Discover commands, run tests, summarize results
- **Context**: Minimal - just test configuration and recent changes

### `e2e-test-discoverer` (Multi-model) 
- **Purpose**: Autonomously generate comprehensive E2E tests
- **Process**: Explore app → Map flows → Generate scenarios → Write tests
- **Focus**: User journeys, auth flows, CRUD, edge cases
- **Context**: App URLs, existing E2E patterns, project docs, UI components

### `e2e-test-verifier` (Multi-model)
- **Purpose**: Independent E2E test verification (READ-ONLY)  
- **Actions**: Run tests, analyze failures, assess quality, provide feedback
- **Output**: Verification reports with screenshots, metrics, recommendations
- **Context**: E2E test files, test configs, app URLs, previous test errors

## WORKFLOW EXAMPLES

### Simple Feature (Traditional)
```
feature-analyzer → code-architect → code-implementation → test-runner → code-reviewer → deploy
```

### Complex Feature (TDD + E2E)
```
Main: feature-analyzer → code-architect → tdd-test-specialist → code-implementation → code-reviewer
Parallel: git worktree → e2e-test-discoverer → e2e-test-verifier → merge
```

### Bug Fix
```
feature-analyzer → tdd-test-specialist → code-implementation → test-runner → code-reviewer
```

### Production Critical
```
Full pipeline with E2E testing in parallel worktree + comprehensive code review
```

Remember: **Each agent has specific expertise** - use them in sequence with complete context for best results.
