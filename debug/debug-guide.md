# DEBUG GUIDE

## DEBUGGING APPROACH

### SYSTEMATIC DEBUGGING PROCESS
1. **Reproduce the error** - Make it happen consistently
2. **Isolate the problem** - Find the smallest case that fails
3. **Examine the evidence** - Error messages, logs, stack traces
4. **Form a hypothesis** - What do you think is wrong?
5. **Test the hypothesis** - Prove or disprove it
6. **Fix the issue** - Apply the solution
7. **Verify the fix** - Ensure it works and doesn't break other things

## ERROR DOCUMENTATION

### ERROR TEMPLATE
```markdown
# ERROR: [Error Name]

## SYMPTOMS
[What you see happening]

## ERROR MESSAGE
```
[Exact error message]
```

## STACK TRACE
```
[Relevant stack trace if available]
```

## ENVIRONMENT
- OS: [Operating system]
- Node/Python/Other version: [Version]
- Browser: [If applicable]
- Other relevant context: [Details]

## CAUSES (Most likely first)
1. [Most probable cause]
2. [Second most probable]
3. [Less likely causes]

## SOLUTIONS
1. **Solution 1** - [Description]
   ```[language]
   [Code fix]
   ```
   Status: ✅ Working / 🧪 Tested / ❌ Failed

2. **Solution 2** - [Description]
   ```[language]
   [Alternative fix]
   ```
   Status: ✅ Working / 🧪 Tested / ❌ Failed

## PREVENTION
[How to avoid this error in the future]

## RELATED ERRORS
- [Link to similar error documentation]
- [Link to knowledge base solutions]

## PROJECTS WHERE THIS OCCURRED
- [Project name]: [Brief context]
```

## COMMON ERROR CATEGORIES

### SYNTAX ERRORS
- Missing brackets, semicolons, quotes
- Typos in variable/function names
- Incorrect language syntax

### RUNTIME ERRORS
- Null/undefined references
- Type mismatches
- Out of bounds access

### LOGIC ERRORS
- Wrong conditional logic
- Incorrect algorithm implementation
- Off-by-one errors

### ENVIRONMENT ERRORS
- Missing dependencies
- Version conflicts
- Configuration issues

### NETWORK ERRORS
- Connection failures
- API response issues
- Timeout problems

## DEBUGGING TOOLS

### BROWSER TOOLS
- **Console**: `console.log()`, `console.error()`, `console.table()`
- **Network tab**: Check API calls, responses
- **Debugger**: Set breakpoints, step through code
- **Elements**: Inspect DOM, CSS issues

### NODE.JS DEBUGGING
- **VS Code debugger**: Set breakpoints, inspect variables
- **Node inspector**: `node --inspect app.js`
- **Logging**: Winston, Bunyan for structured logs

### PYTHON DEBUGGING
- **pdb**: Python debugger (`import pdb; pdb.set_trace()`)
- **print()**: Simple but effective
- **logging module**: Structured logging

## DEBUGGING STRATEGIES

### RUBBER DUCK DEBUGGING
1. Explain the problem out loud
2. Describe what should happen
3. Explain what's actually happening
4. Often reveals the issue while explaining

### DIVIDE AND CONQUER
- Comment out half the code
- If error disappears, problem is in commented part
- Keep narrowing down

### BINARY SEARCH DEBUGGING
- Test middle of problematic code section
- Eliminate half the code each iteration
- Quickly isolate the problematic line

### REPRODUCTION IN ISOLATION
- Create minimal test case
- Remove all unnecessary code
- Test with simple data
- Build up complexity gradually

## LOGGING BEST PRACTICES

### WHAT TO LOG
- Function entry/exit with key parameters
- Error conditions with context
- Important state changes
- Performance metrics

### LOG LEVELS
- **ERROR**: System-breaking issues
- **WARN**: Potential problems
- **INFO**: Important events
- **DEBUG**: Detailed troubleshooting info

### LOG FORMAT
```javascript
// Good logging
console.log(`[UserService] getUser(${userId}) - Starting`);
console.error(`[UserService] getUser(${userId}) - Database error: ${error.message}`);
console.log(`[UserService] getUser(${userId}) - Success: ${user.name}`);
```

## ERROR PREVENTION

### CODE REVIEWS
- Check for null/undefined handling
- Verify error handling paths
- Look for edge cases
- Validate input data

### TESTING
- Unit tests for error conditions
- Integration tests for failure scenarios
- Manual testing of edge cases

### DEFENSIVE PROGRAMMING
```javascript
// Always validate inputs
function processUser(user) {
  if (!user || !user.id) {
    throw new Error('Invalid user: missing ID');
  }
  // Continue processing
}
```

## TROUBLESHOOTING CHECKLIST

### BEFORE DEBUGGING
- [ ] Can I reproduce the error?
- [ ] What changed recently?
- [ ] Are there error messages?
- [ ] Is it environment-specific?

### DURING DEBUGGING
- [ ] What's the smallest case that fails?
- [ ] What are the exact error conditions?
- [ ] Are my assumptions correct?
- [ ] Have I checked the logs?

### AFTER FIXING
- [ ] Does the fix work?
- [ ] Did I break anything else?
- [ ] Is this a permanent fix?
- [ ] Should I document this?