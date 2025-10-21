# GLOBAL PREFERENCES
[FILE LENGTH: 58/600 lines]

## USER PROFILE
- Level: JUNIOR DEVELOPER
- Explanation Style: WHAT → WHY → CODE → EXPLAIN → EXAMPLE

## NAMING CONVENTIONS

### Files
- All lowercase: user-auth.js NOT UserAuth.js
- Kebab-case: user-profile.jsx NOT userProfile.jsx
- Markdown prefixes:
  - prd-* for requirements
  - state-* for current state
  - solution-* for solutions
  - log-* for logs

### Code
- Variables: camelCase (userName, isLoading)
- Constants: UPPER_SNAKE (MAX_RETRIES)
- Functions: camelCase verb-first (getUserData())
- CSS: kebab-case (btn-primary)

## CODING STANDARDS
- const by default, let when needed, NEVER var
- async/await over .then()
- Arrow functions for non-methods
- Try/catch for error handling
- Comment WHY not WHAT

## PREFERRED TOOLS
- React: Vite (NOT Create React App)
- CSS: Start plain, add Tailwind if needed
- State: useState first, then Zustand
- API: fetch() first, then axios
- Package Manager: npm

## AVOID
- Over-engineering
- Premature optimization
- Deep nesting (max 3 levels)
- Copy-paste without understanding

## EXPLANATION TEMPLATE
When explaining concepts to junior developers:
1. **WHAT** - Clear definition of the concept
2. **WHY** - Why it matters and when to use it
3. **CODE** - Working example
4. **EXPLAIN** - Line-by-line breakdown
5. **EXAMPLE** - Real-world usage scenario

## FILE LENGTH MANAGEMENT
- Maximum 600 lines per file
- Split long files: main.md → main-part2.md → main-part3.md
- Add "→ main-part2.md" link at bottom of files
- Keep related content together

## PROJECT STRUCTURE PREFERENCES
- Flat structure when possible
- Maximum 3-4 folder levels deep
- Clear, descriptive folder names
- Index files for exports

## CODE REVIEW CHECKLIST
- [ ] Follows naming conventions
- [ ] Uses const by default
- [ ] Handles errors properly
- [ ] Comments explain WHY
- [ ] No console.log in production
- [ ] Under 600 lines