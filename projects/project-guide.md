# PROJECT GUIDE

## FIRST STEP: ALWAYS CHECK ACTIVE-PROJECTS.MD
When user says "continue" → read active-projects.md → find project → read its state-current.md

## PROJECT TYPES
- **Web**: React/Vue/Angular applications
- **Mobile**: React Native, Flutter
- **CLI**: Command-line tools, scripts
- **API**: REST/GraphQL services

## PROJECT STRUCTURE
```
D:\Kilo Code\projects\[project-name]\
├── state-current.md      # ALWAYS UPDATE THIS FILE
├── prd-[name].md         # Project requirements
├── log-decisions.md      # Why we made choices
├── log-learnings.md      # What we learned
└── [project files]
```

## STARTING NEW PROJECTS
1. Create project folder: `projects/[project-name]/`
2. Initialize required files (see templates below)
3. Set up Git: `git init`, `git add .`, `git commit -m "Initial commit"`
4. Update state-current.md immediately

## REQUIRED FILES

### state-current.md TEMPLATE
```markdown
# CURRENT STATE - [Project Name]

## LAST ACTION
[What you just did]

## CURRENT POSITION
[Where you are in the project]

## NEXT STEPS
1. [Next immediate action]
2. [Following action]

## FILES MODIFIED
- [filename]: [change made]

## DEPENDENCIES INSTALLED
- [package]: [version]

## GIT STATUS
[Current git status]

## QUICK RESUME
[Command to get back to work]
```

### prd-[name].md TEMPLATE
```markdown
# PROJECT REQUIREMENTS - [Project Name]

## GOAL
[What this project accomplishes]

## FEATURES
- [Feature 1]
- [Feature 2]

## TECHNICAL REQUIREMENTS
- [Technology stack]
- [Performance needs]

## SUCCESS CRITERIA
- [How to know it's done]
```

## TRACKING ACTIVE PROJECTS
- Keep a simple list in each project's state-current.md
- Mark status: ACTIVE, PAUSED, COMPLETE
- Update after every work session

## GIT WORKFLOW
1. Initialize: `git init`
2. Add files: `git add .`
3. Commit: `git commit -m "type: description"`
4. Types: feat, fix, docs, refactor, test

## PROJECT CLEANUP
- When complete, move to `projects/completed/`
- Update final state-current.md
- Archive in knowledge base if useful