# CODING GUIDE

## CODING STANDARDS FOR JUNIOR DEVELOPERS

## NAMING CONVENTIONS
- **Files**: lowercase-kebab (example: user-authentication.js)
- **Variables**: camelCase (example: userName, isAuthenticated)
- **Constants**: UPPER_SNAKE (example: API_BASE_URL)
- **Functions**: camelCase, descriptive verbs (example: getUserData(), validateInput())
- **Components**: PascalCase (example: UserProfile, LoginForm)

## CODE STRUCTURE
1. **Imports first** - Group by type (third-party, local)
2. **Constants next** - Configuration, magic numbers
3. **Functions/components** - Main logic
4. **Exports last** - What this file provides

## PREFERRED PACKAGES

### Web Development
- **React**: Use Vite for setup (`npm create vite@latest`)
- **CSS**: Start with plain CSS, add Tailwind when needed
- **State**: useState/useContext first, Redux only if complex
- **Routing**: React Router
- **API**: Fetch API first, Axios only if needed

### Node.js
- **Framework**: Express.js for APIs
- **Database**: Start with SQLite, upgrade to PostgreSQL later
- **Validation**: Joi or Zod
- **Testing**: Jest for unit tests

### Python
- **Web**: Flask (simple), Django (complex)
- **Data**: pandas, numpy
- **CLI**: Click or argparse

## CODING PRINCIPLES

### SIMPLE-FIRST APPROACH
1. **Make it work** - Basic functionality
2. **Make it right** - Clean code, error handling
3. **Make it fast** - Optimize only if needed

### ERROR HANDLING
```javascript
// Always handle errors
try {
  const result = await riskyOperation();
  return result;
} catch (error) {
  console.error('Operation failed:', error.message);
  // Handle gracefully - show user message, fallback, etc.
}
```

### ASYNC/AWAIT PATTERN
```javascript
// Use async/await, not .then()
async function fetchData() {
  try {
    const response = await fetch('/api/data');
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch error:', error);
    return null;
  }
}
```

## CODE QUALITY

### USE CONST BY DEFAULT
```javascript
// Good
const API_URL = 'https://api.example.com';
const users = await fetchUsers();

// Only use let if you need to reassign
let counter = 0;
counter++;
```

### AVOID THESE
- Premature optimization
- Over-engineering
- Complex abstractions
- Magic numbers without constants
- Console.log in production code

### WRITE READABLE CODE
```javascript
// Good - descriptive names
const isAuthenticatedUser = user.token && user.token.expiresAt > Date.now();

// Bad - unclear names
const check = u.t && u.t.e > Date.now();
```

## FILE ORGANIZATION
```
src/
├── components/     # Reusable UI parts
├── pages/         # Route components
├── utils/         # Helper functions
├── services/      # API calls
├── hooks/         # Custom React hooks
└── styles/        # CSS files
```

## WHEN TO REFACTOR
- Function longer than 20 lines
- Repeated code (DRY principle)
- Complex nested conditions
- Hard to understand variable names

## CODE REVIEW CHECKLIST
- [ ] Variables have clear names
- [ ] Functions do one thing
- [ ] Errors are handled
- [ ] Code is readable without comments
- [ ] No console.log statements
- [ ] Constants used for magic values