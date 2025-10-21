# CLI PROJECT TEMPLATE

## PROJECT SETUP
```bash
# Create new project directory
mkdir project-name
cd project-name
npm init -y
```

## INSTALL DEPENDENCIES
```bash
# Core dependencies
npm install commander inquirer chalk

# Development dependencies
npm install -D jest eslint
```

## FOLDER STRUCTURE
```
project-name/
├── bin/
│   └── cli.js           # Executable entry point
├── src/
│   ├── commands/        # Command implementations
│   ├── utils/           # Helper functions
│   ├── config/          # Configuration handling
│   └── index.js         # Main CLI logic
├── tests/               # Test files
├── .env                 # Environment variables
├── .gitignore
├── package.json
└── README.md
```

## PACKAGE.JSON CONFIGURATION
```json
{
  "name": "project-name",
  "version": "1.0.0",
  "description": "CLI tool description",
  "main": "src/index.js",
  "bin": {
    "project-name": "./bin/cli.js"
  },
  "scripts": {
    "start": "node bin/cli.js",
    "test": "jest",
    "lint": "eslint src/ bin/",
    "link": "npm link"
  },
  "keywords": ["cli", "tool"],
  "author": "Your Name",
  "license": "MIT"
}
```

## BIN/CLI.JS (Executable Entry Point)
```javascript
#!/usr/bin/env node

const { program } = require('commander');
const cli = require('../src/index');

program
  .name('project-name')
  .description('CLI tool description')
  .version('1.0.0');

// Add commands
cli.addCommands(program);

program.parse();
```

## SRC/INDEX.JS (Main CLI Logic)
```javascript
const chalk = require('chalk');
const { initCommand } = require('./commands/init');
const { buildCommand } = require('./commands/build');

const addCommands = (program) => {
  // Init command
  program
    .command('init')
    .description('Initialize a new project')
    .option('-n, --name <name>', 'Project name')
    .option('-t, --type <type>', 'Project type (web, api, cli)')
    .action(initCommand);

  // Build command
  program
    .command('build')
    .description('Build the project')
    .option('-w, --watch', 'Watch for changes')
    .action(buildCommand);

  // Error handling
  program.on('command:*', () => {
    console.error(chalk.red('Invalid command: %s'), program.args.join(' '));
    console.log('See --help for a list of available commands.');
    process.exit(1);
  });
};

module.exports = { addCommands };
```

## SAMPLE COMMAND (src/commands/init.js)
```javascript
const inquirer = require('inquirer');
const chalk = require('chalk');
const fs = require('fs');
const path = require('path');

const initCommand = async (options) => {
  console.log(chalk.blue('🚀 Initializing new project...'));

  // Get user input if not provided via options
  const answers = await inquirer.prompt([
    {
      type: 'input',
      name: 'name',
      message: 'Project name:',
      default: options.name || 'my-project',
      when: !options.name
    },
    {
      type: 'list',
      name: 'type',
      message: 'Project type:',
      choices: ['web', 'api', 'cli'],
      default: options.type || 'web',
      when: !options.type
    }
  ]);

  const projectName = options.name || answers.name;
  const projectType = options.type || answers.type;

  try {
    // Create project directory
    const projectPath = path.join(process.cwd(), projectName);
    fs.mkdirSync(projectPath, { recursive: true });

    // Create basic files
    const packageJson = {
      name: projectName,
      version: '1.0.0',
      description: `${projectType} project`,
      main: 'index.js',
      scripts: {
        start: 'node index.js',
        test: 'jest'
      }
    };

    fs.writeFileSync(
      path.join(projectPath, 'package.json'),
      JSON.stringify(packageJson, null, 2)
    );

    console.log(chalk.green(`✅ Project ${projectName} created successfully!`));
    console.log(chalk.cyan(`📁 Location: ${projectPath}`));
    console.log(chalk.yellow(`📋 Next steps:`));
    console.log(chalk.white(`   cd ${projectName}`));
    console.log(chalk.white(`   npm install`));
  } catch (error) {
    console.error(chalk.red('❌ Error creating project:'), error.message);
    process.exit(1);
  }
};

module.exports = { initCommand };
```

## SAMPLE UTILITY (src/utils/logger.js)
```javascript
const chalk = require('chalk');

const logger = {
  info: (message) => console.log(chalk.blue(message)),
  success: (message) => console.log(chalk.green(message)),
  warning: (message) => console.log(chalk.yellow(message)),
  error: (message) => console.log(chalk.red(message)),
  dim: (message) => console.log(chalk.dim(message))
};

module.exports = logger;
```

## DEVELOPMENT COMMANDS
```bash
npm run link          # Link CLI for local testing
project-name --help   # Test CLI
npm run test          # Run tests
npm run lint          # Run linter
```

## GIT INITIALIZATION
```bash
git init
git add .
git commit -m "feat: initial CLI tool setup"
```

## NEXT STEPS
1. Add more commands
2. Implement configuration system
3. Add validation and error handling
4. Create comprehensive tests
5. Add help documentation