# TEST WORKFLOW DEMONSTRATION
[FILE LENGTH: 45/600 lines]

## Test Request: "Create a new React app called todo-list"

### STEP 1: READ MASTER.MD
✅ I'm reading master.md first (as required by the warning header)
- Identify task type: "Project Management" (creating new project)
- Navigate to: ./projects/project-guide.md

### STEP 2: READ PROJECT-GUIDE.MD
✅ Following the decision tree
- Check "FIRST STEP: Always check active-projects.md"
- Since this is a new project, proceed to project setup

### STEP 3: CHECK GLOBAL-PREFERENCES.MD
✅ Verify coding standards before starting
- React: Use Vite (NOT Create React App)
- Files: lowercase-kebab
- Variables: camelCase
- const by default

### STEP 4: CREATE PROJECT STRUCTURE
✅ Following template-web.md
```bash
# Create project folder
mkdir projects/todo-list
cd projects/todo-list

# Initialize with Vite
npm create vite@latest todo-list -- --template react
cd todo-list
npm install
```

### STEP 5: CREATE REQUIRED FILES
✅ Following project-guide.md template
- state-current.md
- prd-todo-list.md
- log-decisions.md

### STEP 6: UPDATE ACTIVE-PROJECTS.MD
✅ Add new project to tracker
| Project | Location | Type | Status | Last Worked | Current Task | Next Step |
|---------|----------|------|--------|-------------|--------------|-----------|
| todo-list | projects/todo-list/ | web | active | [timestamp] | initial setup | add components |

### STEP 7: CREATE STATE-CURRENT.MD
✅ Track current project state
```markdown
# CURRENT STATE - todo-list

## LAST ACTION
Created React app with Vite and project structure

## CURRENT POSITION
Basic React app initialized, ready for component development

## NEXT STEPS
1. Create TodoItem component
2. Create TodoList component
3. Add state management
4. Implement add/delete functionality

## FILES MODIFIED
- Created project structure
- Added state-current.md
- Added prd-todo-list.md
- Added log-decisions.md

## DEPENDENCIES INSTALLED
- React 18.x
- Vite
```

### STEP 8: INITIALIZE GIT
✅ Following project-guide.md git workflow
```bash
cd projects/todo-list
git init
git add .
git commit -m "feat: initial React todo app setup"
```

### STEP 9: UPDATE ALL FILES
✅ Following kilo-code-instructions.md checklist
☐ Updated state-current.md? ✅
☐ Updated active-projects.md? ✅
☐ Documented new learnings? ✅
☐ Checked file lengths? ✅
☐ Made git commit? ✅

## DEMONSTRATION COMPLETE
All system workflows tested successfully:
- Navigation from master.md works
- Decision tree routing works
- Project creation follows templates
- State tracking is functional
- File updates are automatic
- Git integration works