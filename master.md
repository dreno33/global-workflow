## ⚠️ KILO CODE: READ THIS FIRST
Before doing ANYTHING:
1. I am reading master.md (✓ doing it now)
2. I will follow the decision tree below
3. I will update ALL relevant files
4. See kilo-code-instructions.md for my rules

# KILO CODE - MASTER CONTROL FILE

## CORE PRINCIPLES
- SIMPLICITY FIRST
- BUILD STRONG FOUNDATIONS  
- DOCUMENT EVERYTHING

## ALWAYS START HERE
Every prompt begins by reading this file first. Use the decision tree below to route to the correct subsystem.

## DECISION TREE
```
TASK TYPE → DESTINATION
├── Code/Programming → ./coding/coding-guide.md
├── System Architecture → ./architecture/arch-guide.md  
├── Debugging/Troubleshooting → ./debug/debug-guide.md
├── Learning/Research → ./knowledge/knowledge-guide.md
├── Project Management → ./projects/project-guide.md
└── General/Other → ./general/general-guide.md
```

## FOLDER STRUCTURE
```
D:\Kilo Code\
├── master.md (THIS FILE)
├── coding/          # Code tasks, languages, frameworks
├── architecture/    # System design, patterns, planning
├── debug/          # Troubleshooting, error analysis
├── knowledge/      # Learning, research, documentation
├── projects/       # Project workflows, management
│   └── spotless-improved/  # Spotless App Project
│       ├── README.md       # → START HERE for Spotless project
│       └── [project files]
└── general/        # Miscellaneous tasks
```

## 🚨 SPOTLESS PROJECT RULE

**When working on Spotless App (`D:\Kilo Code\projects\spotless-improved`):**

1. **CRITICAL:** Follow documentation on LOCAL PC, not on server!
   - MD files are in git for version control
   - Server gets them via git pull (harmless text files)
   - But YOU follow checklists on YOUR LOCAL PC to make changes
   - Server only RUNS: code and build files

2. **ALWAYS START** with `projects/spotless-improved/README.md`

3. **USE CHECKLISTS** for all deployment tasks:
   - Backend changes → [BACKEND-CHECKLIST.md](./projects/spotless-improved/BACKEND-CHECKLIST.md)
   - Frontend changes → [FRONTEND-CHECKLIST.md](./projects/spotless-improved/FRONTEND-CHECKLIST.md)
   - Mobile changes → [MOBILE-CHECKLIST.md](./projects/spotless-improved/MOBILE-CHECKLIST.md)

4. **BEFORE mobile changes:** MUST read VERSION-INCREMENT-REMINDER.md (version must increment for EVERY build)

5. **NEVER BE CONFUSED:** If we've done it before, the process is documented in the checklist

6. **UPDATE** all relevant documentation after changes (locally only)

**Navigation:** `D:\Kilo Code\projects\spotless-improved\README.md` → Choose checklist by component

**Core Documentation Files:**
- **README.md** - Navigation hub with quick links by task
- **BACKEND-CHECKLIST.md** - Complete backend deployment workflow
- **FRONTEND-CHECKLIST.md** - Complete frontend deployment workflow
- **MOBILE-CHECKLIST.md** - Complete mobile build/deploy workflow
- **VERSION-INCREMENT-REMINDER.md** - CRITICAL for mobile (always increment version)
- **VERSION-TRACKING.md** - Mobile version history
- **MASTER-DOCUMENTATION.md** - Complete system architecture
- **state-current.md** - Current project state and active issues

## CONTINUOUS UPDATE PROTOCOL
1. After completing any task, update relevant guide files
2. Add new patterns/learnings to appropriate subsystems
3. Update this master file if new task categories emerge
4. **FILE LENGTH MANAGEMENT: Keep all files under 600 lines**
   - When approaching 600 lines, split content: main.md → main-part2.md
   - Add "→ main-part2.md" link at bottom of files
   - Update cross-references when splitting files
5. Maintain clear cross-references between files

## NAVIGATION LINKS
- [Coding Guide](./coding/coding-guide.md)
- [Architecture Guide](./architecture/arch-guide.md)
- [Debug Guide](./debug/debug-guide.md)
- [Knowledge Guide](./knowledge/knowledge-guide.md)
- [Project Guide](./projects/project-guide.md)
- [General Guide](./general/general-guide.md)

## QUICK START
1. Read this file
2. Follow decision tree to identify task type
3. Navigate to linked guide
4. Execute task using guide's framework
5. Update files based on learnings