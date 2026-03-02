---
name: integrated-dev-workflow
version: "1.0.0"
description: Use when starting any new feature - AUTOMATICALLY orchestrates spec-kit, superpowers, and planning-with-files into a complete workflow
user-invocable: true
allowed-tools:
  - skill
  - Bash
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - WebFetch
  - WebSearch
hooks:
  PreToolUse:
    - matcher: "Write|Edit|Bash|Read|Glob|Grep"
      hooks:
        - type: command
          command: "cat task_plan.md 2>/dev/null | head -25 || echo '[integrated-dev-workflow] Reminder: Update task_plan.md after completing any phase'"
  PostToolUse:
    - matcher: "Write|Edit"
      hooks:
        - type: command
          command: "echo '[integrated-dev-workflow] File updated - check if task status needs updating in task_plan.md'"
  Stop:
    - hooks:
        - type: command
          command: "echo '[integrated-dev-workflow] Session ending - verify task_plan.md shows correct progress'"
metadata:
  version: "1.0.0"
  integrates:
    - spec-kit
    - superpowers
    - planning-with-files
---

> **English** | [中文](README.zh-CN.md)

---

# Integrated Development Workflow

> **One command to start everything.** When user says "build something", "implement feature", or "create X", immediately invoke this skill and start the workflow.

## 🎯 Usage

```bash
# Just say what you want to build:
"I want to build a user login feature"
# OR
"Implement a shopping cart"
# OR
"Create an API for..."
```

This skill will AUTOMATICALLY:
1. ✅ Check for previous session
2. ✅ Create tracking files
3. ✅ Guide through requirements (spec-kit)
4. ✅ Plan the implementation (superpowers)
5. ✅ Execute with best practices (planning-with-files)

---

WH|- `finishing-a-development-branch` - Completion
JQ|
QV|---
QV|
QV|## 🎯 Hybrid Mode: Zero-Dependency with Fallback
QV|
QV|This skill works in **two modes**:
QV|
QV|### Mode 1: Advanced (with sub-skills)
QV|If sub-skills are installed, this skill delegates to them for optimal experience:
QV|
QV|```bash
QV|# Try external skill first
QV|skill("brainstorming")
QV|skill("test-driven-development")
QV|skill("verification-before-completion")
QV|...
QV|```
QV|
QV|### Mode 2: Standalone (zero-dependency)
QV|If sub-skills are NOT installed, this skill uses **built-in fallback** content:
QV|
QV|- All core workflows are documented inline
QV|- Templates are embedded in this skill
QV|- You follow the same process, but with guidance in this file
QV|
QV|### How It Works
QV|
QV|1. **First**: Try to invoke sub-skill `skill("xxx")`
QV|2. **If not found**: Use the inline fallback guide for that step
QV|3. **Result**: Works identically either way - just different experience levels
QV|
QV|### Recommended: Install Sub-Skills
QV|For best experience, also install these sub-skills:
QV|
QV|- `planning-with-files` - Enhanced tracking hooks
QV|- `test-driven-development` - Full TDD workflow
QV|- `systematic-debugging` - Structured debugging
QV|- `verification-before-completion` - Comprehensive verification
QV|- `requesting-code-review` - Code review workflow
QV|- `finishing-a-development-branch` - PR/merge options
QV|
QV|But the skill **works completely without them** - that's the design.
QV|
QV|---
QV|
QV|## 📖 Workflow: Zero-Dependency Fallback Guides

---

## 🔄 Automatic Workflow

### Step 1: Session Recovery

Check for previous session:
- git status
- ls task_plan.md findings.md progress.md 2>/dev/null

If files exist → Read them → Resume from last position

### Step 2: Initialize Tracking

If NO previous session → Create tracking files:

```bash
# task_plan.md
cat > task_plan.md << 'EOF'
# Task Plan

## Goal
[USER'S GOAL]

## Phases
- [ ] Phase 1: Requirements & Design
- [ ] Phase 2: Technical Planning
- [ ] Phase 3: Implementation
- [ ] Phase 4: Testing & Review
- [ ] Phase 5: Completion

## Current Phase
Phase 1: Requirements & Design

## Tasks (Phase 1)
- [ ] Define requirements with user
- [ ] Create specification
- [ ] Review and approve spec

## Decisions Made
-

## Blockers
-
EOF

# findings.md
cat > findings.md << 'EOF'
# Findings

## Research
-

## Technical Decisions
-

## Notes
-
EOF

# progress.md
cat > progress.md << 'EOF'
# Progress

## Session Log
- Started: [timestamp]

## Test Results
| Test | Status |
|------|--------|
| | |

## Errors Encountered
| Error | Resolution |
|-------|------------|
| | |
EOF
```

### Step 3: Requirements Phase

Say to user: "Let's define the requirements. What should [feature] do?"

Then guide with spec-kit:
BY|3. Try: skill("brainstorming") - If not found, use guide below
BY|
BY|#### Fallback: Requirements Guide
BY|If brainstorming skill not available, follow this process:
BY|
BY|1. **Explore context** - Check project files, docs, recent commits
BY|2. **Ask questions** - One at a time, understand purpose/success criteria
BY|3. **Propose approaches** - 2-3 options with trade-offs
BY|4. **Present design** - Get user approval
BY|5. **Document** - Save to task_plan.md

After requirements are clear:
→ Update task_plan.md Phase 1 to complete
→ Mark Phase 2 as in_progress

### Step 4: Technical Planning

HV|If tasks need refinement:
HX|Try: skill("writing-plans") - If not found, use guide below
HX|
HX|#### Fallback: Task Refinement Guide
HV|If writing-plans skill not available:
HV|
HV|1. **Break down** each feature into smallest testable units
HV|2. **Identify dependencies** between tasks
HV|3. **Estimate effort** - mark complex tasks
HV|4. **Order tasks** - what can be done in parallel?
HV|5. **Update task_plan.md** with refined task list

After planning complete:
→ Update task_plan.md Phase 2 to complete
→ Mark Phase 3 as in_progress

### Step 5: Implementation

HM|      - Write failing test
NS|      - Write minimal code
RN|      - Refactor
PR|      - Commit
TK|
TB|#### Fallback: TDD Process (if skill unavailable)
TB|
TB|1. **RED**: Write failing test first
TB|2. **GREEN**: Write minimal code to pass
TB|3. **REFACTOR**: Clean up code
TB|
TB|**Iron Law**: NO CODE WITHOUT FAILING TEST FIRST
   
   b. After each code change:
      - Update progress.md with what was done
      - Update task status in task_plan.md

RY|   - skill("systematic-debugging")
JY|
JY|#### Fallback: Debugging (if skill unavailable)
JY|1. Read errors carefully
JY|2. Reproduce consistently
JY|3. Form hypothesis
JY|4. Test before fixing
JY|5. Fix root cause
JY|
JY|**Rule**: NO FIXES WITHOUT ROOT CAUSE FIRST

After all tasks complete:
→ Update task_plan.md Phase 3 to complete
→ Mark Phase 4 as in_progress

### Step 6: Testing & Review

JQ|   - Check all requirements met
RX|
RX|#### Fallback: Verification Guide (if skill unavailable)
RX|1. Run test suite - all tests pass?
RX|2. Build project - compiles without errors?
RX|3. Check spec - all requirements met?
RX|4. Manual test - key flows work?

2. skill("requesting-code-review")
   - Review code against plan
   - Report issues by severity

BR|   - Fix and re-review
WR|
WR|#### Fallback: Review Handling (if skill unavailable)
WR|1. Read review comments carefully
WR|2. Don't take personally - it's about code
WR|3. Ask clarifying questions if unclear
WR|4. Make changes, re-request review
WR|5. Thank reviewer for feedback

After review complete:
→ Update task_plan.md Phase 4 to complete
→ Mark Phase 5 as in_progress

### Step 7: Completion

1. Final verification:
   - All tests passing?
   - Build successful?
   - All tasks marked complete?

VH|   - Present options: merge / PR / keep / discard
YH|
YH|#### Fallback: Completion Options (if skill unavailable)
YH|1. **Merge** - Pull into main branch
YH|2. **PR** - Create pull request for review
YH|3. **Keep** - Leave branch for later
YH|4. **Discard** - Delete branch if not needed
YH|Choose based on team workflow and project state

3. Update final status in progress.md

---

## 📋 Command Reference

| This Skill Automatically Uses | For |
|------------------------------|-----|
| `planning-with-files` hooks | Persistence reminders |
| `spec-kit commands` | Requirements & planning |
| `brainstorming` | Requirement clarification |
| `writing-plans` | Task refinement |
| `using-git-worktrees` | Branch management |
| `subagent-driven-development` | Task execution |
| `test-driven-development` | TDD workflow |
| `systematic-debugging` | Issue resolution |
| `verification-before-completion` | Quality verification |
| `requesting-code-review` | Code review |
| `receiving-code-review` | Review handling |
| `finishing-a-development-branch` | Completion |

---

## 💡 Usage Examples

### Example 1: Simple Feature

**User says:**
> "Add user authentication to the app"

**Workflow:**
1. Create task_plan.md with goal "Add user authentication"
2. Ask: "What should authentication include? (login, register, password reset?)"
3. Run /speckit.specify to document requirements
4. Plan tasks: login form, register form, API endpoints, token handling
5. Execute each task with TDD
6. Verify and create PR

### Example 2: Complex Refactoring

**User says:**
> "Refactor the data layer to use repositories"

**Workflow:**
1. Create task_plan.md with goal "Refactor data layer"
2. Ask: "What's the current architecture? What's the target?"
3. Document current problems in findings.md
4. Plan: extract interface, create repository, migrate callers one by one
5. Execute with test coverage at each step
6. Full regression testing before completion

### Example 3: Bug Fix

**User says:**
> "Fix the login timeout issue"

**Workflow:**
1. Create task_plan.md with goal "Fix login timeout"
2. Ask: "When does it timeout? What's the error?"
3. Research: check logs, find root cause in findings.md
4. Plan: fix timeout value, add retry logic, update tests
5. Implement and verify the fix works

---

## 🔧 Troubleshooting

### Problem: Session recovery fails

**Symptom:** Tracking files exist but contain outdated information

**Solution:**
1. Read the files to understand current state
2. Ask user: "Should I resume from where we left off or start fresh?"
3. If resuming, update the "Current Phase" to reflect actual state

### Problem: User doesn't want to define requirements

**Symptom:** User says "just start coding"

**Solution:**
1. Create minimal task_plan.md with just the goal
2. Note in findings.md: "Requirements not formally defined"
3. Proceed with implementation but add validation step at the end
4. Document any assumptions in findings.md

### Problem: Too many tasks in task_plan.md

**Symptom:** task_plan.md becomes overwhelming

**Solution:**
1. Break into multiple phases
2. Create subtask files: `tasks-phase1.md`, `tasks-phase2.md`
3. Focus on current phase only
4. Move completed tasks to "Completed Tasks" section

### Problem: Progress tracking feels redundant

**Symptom:** "I'm already updating task_plan.md, why progress.md too?"

**Solution:**
- task_plan.md = WHAT needs doing (tasks, phases)
- progress.md = WHAT HAPPENED (session log, errors, test results)
- Keep progress.md brief: one line per session action

### Problem: Workflow interrupted mid-phase

**Symptom:** User has to leave, context will be lost

**Solution:**
1. Update task_plan.md with exact next step
2. Add note in progress.md: "Interrupted at [step], resume with [action]"
3. On resume: read files, continue from marked position

---

## ⚠️ Common Mistakes

### ❌ Skipping session recovery check
Always check for existing tracking files first. Users may have partially completed work.

### ❌ Not updating progress.md
Without progress.md, you lose context on what was tried and failed.

### ❌ Coding before specification
Even for small changes, document what you're building first.

### ❌ Skipping verification
Never claim "complete" without running tests and verifying the build.

### ❌ Ignoring errors in findings.md
Every error should be logged to prevent repeating failed approaches.

---

## 🔑 Key Rules

1. **ALWAYS create tracking files first** — task_plan.md, findings.md, progress.md
2. **Update progress.md after EVERY significant action**
3. **Log ALL errors** in findings.md — prevents repetition
4. **Verify BEFORE claiming complete** — run tests, check build
5. **NEVER code without spec** — use spec-kit first

---

## 📝 What Gets Created

| File | Purpose |
|------|---------|
| `task_plan.md` | Phase tracking, task checklist |
| `findings.md` | Research, decisions, notes |
| `progress.md` | Session log, test results, errors |

---

## 🚀 Quick Start Template

When user says "build X", IMMEDIATELY run:

```bash
# 1. Create task_plan.md
cat > task_plan.md << 'TEMPLATE'
# Task Plan

## Goal
[BUILD X]

## Phases
- [ ] Phase 1: Requirements
- [ ] Phase 2: Planning
- [ ] Phase 3: Implementation
- [ ] Phase 4: Testing & Review
- [ ] Phase 5: Completion

## Current Phase
Phase 1

## Tasks
TEMPLATE

# 2. Create findings.md
echo "# Findings" > findings.md

# 3. Create progress.md
echo "# Progress" > progress.md

# 4. Tell user what's next
say: "Let's start! I'll guide you through: Requirements → Planning → Implementation → Testing → Completion"
```

Then proceed through the phases automatically.
