# INSTALLATION GUIDES
[FILE LENGTH: 45/600 lines]

## Node.js & npm
1. Download from https://nodejs.org (LTS version)
2. Run installer with default settings
3. Verify installation:
   ```bash
   node --version
   npm --version
   ```

## Git
1. Download from https://git-scm.com
2. Run installer with default settings
3. Configure:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"
   ```

## Visual Studio Code
1. Download from https://code.visualstudio.com
2. Install with default settings
3. Recommended extensions:
   - ES7+ React/Redux/React-Native snippets
   - Prettier - Code formatter
   - ESLint
   - GitLens

## React Development
```bash
# Create new React app with Vite
npm create vite@latest my-app -- --template react
cd my-app
npm install
npm run dev
```

## Python Development
1. Download from https://python.org (version 3.9+)
2. Add to PATH during installation
3. Verify:
   ```bash
   python --version
   pip --version
   ```

## MongoDB (Local)
1. Download from https://www.mongodb.com/try/download/community
2. Install with default settings
3. Start MongoDB service
4. Optional: MongoDB Compass for GUI

## VS Code Extensions Setup
Install these extensions for optimal development:
```bash
# In VS Code terminal
code --install-extension ms-vscode.vscode-json
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension dbaeumer.vscode-eslint
```

## Browser Developer Tools
- Chrome: Built-in DevTools (F12)
- Firefox: Built-in DevTools (F12)
- React Developer Tools extension
- Redux DevTools extension (if using Redux)

## Environment Setup Verification
Run this command to verify your setup:
```bash
node --version && npm --version && git --version
```

## Troubleshooting
- Node not found: Restart terminal or restart computer
- Permission denied: Use administrator mode
- npm slow: Consider using npm mirror: `npm config set registry https://registry.npmmirror.com`