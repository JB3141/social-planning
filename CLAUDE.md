# CLAUDE.md - Project Knowledge Base

**Purpose:** Coding patterns, conventions, and preferences that make development faster and more consistent.

**Last Updated:** 2025-11-16

---

## Project Overview

**Stack:** Node.js (Early Stage)

**Architecture:** New project - architecture will be documented as it evolves

**Key Principles:**
1. Prefer duplication over complexity
2. Document as you go
3. Quality compounds
4. Each feature makes the next easier

---

## Code Style

### General JavaScript/TypeScript Conventions

**Naming:**
- Use camelCase for variables and functions
- Use PascalCase for classes and components
- Use UPPER_SNAKE_CASE for constants
- Use descriptive names that reveal intent

**File Organization:**
- One primary export per file
- Group related functionality in directories
- Index files for clean imports

**Comments:**
- Explain "why", not "what"
- Document complex business logic
- Keep comments up to date with code changes

### Code Formatting

- Use consistent indentation (2 or 4 spaces)
- Use semicolons consistently
- Use single quotes for strings (or configure prettier)
- Add trailing commas in multi-line structures

---

## Common Patterns

### Pattern Template

Use this template when documenting new patterns:

**Context:** [When to use this pattern]

**Implementation:**
```javascript
// Code example here
```

**Why This Works:**
- [Benefit 1]
- [Benefit 2]

**Established:** [Feature name], [Date]
**Last Used:** [Feature name], [Date]

---

## Project-Specific Conventions

### To Be Established

As you build features, document your conventions here:

- Error handling approach
- Async/await vs promises
- Testing patterns
- API response formats
- Database query patterns
- Validation approach
- Authentication/authorization patterns

---

## Common Gotchas

### To Be Discovered

Document common mistakes and how to avoid them:

**Problem:** [What goes wrong]
**Solution:** [How to fix it]
**Prevention:** [How to avoid it in the first place]

---

## Reusable Components/Utilities

### To Be Built

As you create reusable code, document it here:

**Component/Utility Name:**
- **Purpose:** What it does
- **Location:** Where to find it
- **Usage Example:** How to use it
- **Last Updated:** When it was last modified

---

## Testing Strategy

### To Be Defined

Document your testing approach:

- Unit testing patterns
- Integration testing approach
- Test file naming and location
- Mocking strategies
- Coverage goals

---

## Recent Learnings

### Project Initialization - 2025-11-16

**What Worked:**
- Setting up compounding engineering workflow from the start
- Creating knowledge capture structure before writing code

**Pattern Established:**
- Document as you go
- Capture patterns immediately when they emerge

**Time:** 15 minutes for initialization

---

## Cross-References

- **llms.txt:** Architectural principles and high-level decisions
- **knowledge/learnings/:** Feature-specific detailed learnings
- **README.md:** Project setup and overview (to be created)

---

## Quick Reference

### Adding a New Pattern

1. Build the feature
2. Identify what worked well
3. Document the pattern in the appropriate section above
4. Update "Last Used" when you reuse it
5. Refine the pattern based on repeated use

### When to Update This File

- After completing each feature
- When establishing a new convention
- When you discover a better way to do something
- When you hit a gotcha that should be documented

---

**Remember:** This file grows with each feature. Keep it updated, and it will make every subsequent feature easier.

**The Compounding Effect:** Each update here makes the next feature 10-30% easier.
