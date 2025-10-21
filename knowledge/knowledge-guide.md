# KNOWLEDGE GUIDE

## AUTOMATIC CAPTURE RULE
EVERY time you solve a new problem:
1. Check if solution exists in index.md
2. If not, IMMEDIATELY create solution-[problem].md
3. Add to index.md
4. Tag appropriately

## SOLUTION TEMPLATE
# SOLUTION: [Problem Name]
## Problem
[What needed solving]
## Solution
```[language]
[Working code]
```
## Context
[When/why this occurs]
## Tags
#tag1 #tag2

## WHEN TO CAPTURE KNOWLEDGE
1. **Solved a problem** - Document the solution
2. **Found a better way** - Record the improvement
3. **Hit an error** - Note the fix
4. **Learned a new pattern** - Save for future use
5. **Discovered a useful tool** - Add to your toolkit

## KNOWLEDGE TYPES & STORAGE

### SOLUTIONS
**Location**: `knowledge/solutions/`
**Naming**: `solution-[problem].md`
**When**: Fixed a specific problem

### PATTERNS
**Location**: `knowledge/patterns/`
**Naming**: `pattern-[name].md`
**When**: Found reusable code approach

### LANGUAGES
**Location**: `knowledge/languages/`
**Naming**: `lang-[language]-[topic].md`
**When**: Language-specific learning

### FRAMEWORKS
**Location**: `knowledge/frameworks/`
**Naming**: `fw-[framework]-[topic].md`
**When**: Framework-specific knowledge

## DOCUMENTATION TEMPLATES

### SOLUTION TEMPLATE
```markdown
# SOLUTION: [Problem Name]

## PROBLEM
[Brief description of the issue]

## SYMPTOMS
[What you saw that indicated the problem]

## ROOT CAUSE
[Why the problem occurred]

## SOLUTION
[Step-by-step fix]

## CODE EXAMPLE
```[language]
[Working code that solves it]
```

## PREVENTION
[How to avoid this in the future]

## RELATED
- [Link to related knowledge]
- [Link to project where this occurred]

## CONFIDENCE
✅ Tested / 🧪 Experimental
```

### PATTERN TEMPLATE
```markdown
# PATTERN: [Pattern Name]

## PURPOSE
[What this pattern accomplishes]

## WHEN TO USE
[Specific situations where this applies]

## IMPLEMENTATION
```[language]
[Code showing the pattern]
```

## BENEFITS
- [Benefit 1]
- [Benefit 2]

## DRAWBACKS
- [Potential issues]
- [When NOT to use]

## EXAMPLES
- [Real usage examples]

## RELATED PATTERNS
- [Link to similar patterns]
```

## TAGGING SYSTEM

### PRIMARY TAGS
- `#frontend` - UI/React/CSS
- `#backend` - API/Database/Server
- `#debugging` - Troubleshooting
- `#performance` - Optimization
- `#security` - Security practices
- `#testing` - Test approaches

### SECONDARY TAGS
- `#react` `#nodejs` `#python` `#javascript`
- `#css` `#html` `#api` `#database`
- `#git` `#deployment` `#tools`

### USAGE
Add tags to the top of each file:
```markdown
# SOLUTION: Authentication Error

#tags: #backend #debugging #nodejs #api
```

## SEARCHING KNOWLEDGE

### EFFECTIVE SEARCH STRATEGY
1. **Start broad** - Search by category first
2. **Use tags** - Filter by relevant tags
3. **Check patterns** - Look for reusable solutions
4. **Review solutions** - Check for similar problems
5. **Cross-reference** - Follow related links

### SEARCH COMMANDS
```bash
# Find all React patterns
find knowledge/patterns/ -name "*react*"

# Search for debugging solutions
grep -r "#debugging" knowledge/solutions/

# Find all authentication-related knowledge
grep -r "auth" knowledge/ --include="*.md"
```

## KNOWLEDGE MAINTENANCE

### UPDATING EXISTING KNOWLEDGE
1. **Review monthly** - Check for outdated information
2. **Improve clarity** - Add better examples
3. **Add cross-references** - Link related concepts
4. **Update confidence** - Change 🧪 to ✅ when tested

### KNOWLEDGE RETIREMENT
- Mark outdated files with `#deprecated`
- Move to `knowledge/archived/` if no longer relevant
- Add note explaining why it's deprecated

## CAPTURE WORKFLOW

1. **Encounter problem/solution**
2. **Create appropriate file** using template
3. **Add tags and cross-references**
4. **Test the solution** (if applicable)
5. **Update confidence level**
6. **Link from project logs** if project-specific

## KNOWLEDGE SHARING

### WHEN TO SHARE
- Common problems that others might face
- Useful patterns that save time
- Best practices that improve quality

### HOW TO SHARE
- Keep explanations clear for junior developers
- Include working code examples
- Explain the "why" not just the "how"
- Add context about when to use/not use