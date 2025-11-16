# Compounding Engineering Workflow

This project uses the compounding engineering workflow to make each feature easier than the last.

## Quick Start

### Plan a Feature
```bash
claude /compounding-engineering:plan "Add user authentication"
```

Creates: `tasks/PRD_user_authentication.md`

### Execute the Plan
```bash
claude /compounding-engineering:work tasks/PRD_user_authentication.md
```

Creates: Branch, worktree, executes systematically

### Review the Work
```bash
claude /compounding-engineering:review
```

Runs: 12+ specialized review agents in parallel

### Capture Learnings

After completing a feature:
1. Update `CLAUDE.md` with patterns used
2. Update `llms.txt` with architectural decisions
3. Create learning doc in `knowledge/learnings/` using the template

## File Structure

```
.
├── CLAUDE.md                  # Coding patterns and conventions
├── llms.txt                   # Architecture and design principles
├── tasks/                     # Feature plans and task breakdowns
├── status/                    # Live execution dashboards (git ignored)
└── knowledge/
    ├── README.md              # This file
    └── learnings/             # Feature retrospectives
        └── TEMPLATE.md        # Template for learning docs
```

## Philosophy

**Each unit of engineering work should make subsequent units of work easier—not harder.**

This is achieved through:

1. **Systematic Planning** - Detailed PRDs that think through the feature before coding
2. **Organized Execution** - Clear task breakdown and status tracking
3. **Quality Review** - Multi-agent review catching issues early
4. **Knowledge Capture** - Documenting patterns and learnings as you go

## The Compounding Effect

### First Feature
- Set up testing infrastructure
- Establish error handling pattern
- Create first reusable component
- Document patterns in CLAUDE.md

### Second Feature
- Reuse testing infrastructure (30% faster)
- Follow established error handling (fewer bugs)
- Build on existing components (less code)
- Reference documented patterns (fewer decisions)

### Third Feature and Beyond
- Even faster because of accumulated patterns
- Higher quality because of established conventions
- Less cognitive load because of documentation
- More consistency because of reusable components

**The result:** Each feature compounds the value of previous work, making development progressively faster and better.

## Workflow Commands

### /compounding-engineering:plan
Creates a detailed Product Requirements Document (PRD) for a feature.

**Usage:** `/compounding-engineering:plan "Feature description"`

**Output:** `tasks/PRD_[feature_name].md`

**Includes:**
- User stories and requirements
- Technical specification
- Implementation tasks
- Testing strategy
- Success criteria

### /compounding-engineering:work
Executes a PRD systematically with real-time status tracking.

**Usage:** `/compounding-engineering:work tasks/PRD_[feature_name].md`

**Process:**
1. Creates feature branch and worktree
2. Breaks down tasks
3. Executes systematically
4. Updates status in real-time
5. Commits work with clear messages

### /compounding-engineering:review
Runs comprehensive multi-agent code review.

**Usage:** `/compounding-engineering:review`

**Agents (12+):**
- Security vulnerabilities
- Performance issues
- Code quality
- Test coverage
- Documentation
- Error handling
- And more...

### /compounding-engineering:triage
Reviews and prioritizes findings from review.

**Usage:** `/compounding-engineering:triage`

**Process:**
- Categorizes issues by severity
- Suggests fixes
- Prioritizes action items

## Best Practices

### 1. Document While Building

Don't wait until after the feature is done. Document patterns as you discover them.

**When to update CLAUDE.md:**
- Found a clean solution to a problem
- Established a new convention
- Created a reusable component
- Discovered a gotcha to avoid

### 2. Keep Learnings Current

Use the template in `knowledge/learnings/TEMPLATE.md` to capture:
- What worked well
- What was challenging
- Patterns established
- Time estimates vs. actuals

### 3. Reference Documentation

Before starting a new feature:
1. Review CLAUDE.md for relevant patterns
2. Check llms.txt for architectural guidelines
3. Read learnings from similar features

### 4. Update as You Go

Don't let documentation drift from reality:
- Update patterns when they evolve
- Mark decisions as superseded when changed
- Keep examples current with actual code

## Getting Started

### Your First Feature

1. **Read the documentation:**
   - Review `CLAUDE.md`
   - Review `llms.txt`
   - Familiarize yourself with this README

2. **Plan your feature:**
   ```bash
   claude /compounding-engineering:plan "Your feature description"
   ```

3. **Review the plan:**
   - Check `tasks/PRD_[feature].md`
   - Make sure it's complete
   - Add any missing details

4. **Execute the plan:**
   ```bash
   claude /compounding-engineering:work tasks/PRD_[feature].md
   ```

5. **Review the code:**
   ```bash
   claude /compounding-engineering:review
   ```

6. **Capture learnings:**
   - Copy `knowledge/learnings/TEMPLATE.md`
   - Fill in what you learned
   - Update `CLAUDE.md` and `llms.txt`

### Your Second Feature (Already Easier!)

1. **Reference previous work:**
   - Check CLAUDE.md for patterns
   - Review learnings from first feature
   - Reuse established conventions

2. **Follow the same workflow:**
   - Plan, Work, Review, Learn
   - Each iteration compounds

## Tips for Success

1. **Start small** - Don't try to document everything upfront
2. **Be specific** - Document actual patterns you use, not theoretical ones
3. **Include examples** - Code examples make patterns clear
4. **Update regularly** - Little updates compound better than big rewrites
5. **Trust the process** - The benefits compound over time

## Links

- **Compounding Engineering Article:** [Every.to Source Code](https://every.to/source-code/my-ai-had-already-fixed-the-code-before-i-saw-it)
- **Project Knowledge:** `CLAUDE.md`
- **Architecture Decisions:** `llms.txt`
- **Learning Archive:** `knowledge/learnings/`

---

**Remember:** The goal isn't perfect documentation. The goal is to make each feature easier than the last. Documentation is the tool, not the destination.

Start building, start documenting, and watch the compound effect accelerate your development!
