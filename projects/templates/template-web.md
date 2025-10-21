# WEB PROJECT TEMPLATE

## PROJECT SETUP
```bash
# Create React app with Vite
npm create vite@latest project-name -- --template react
cd project-name
npm install
```

## FOLDER STRUCTURE
```
project-name/
├── src/
│   ├── components/     # Reusable UI components
│   ├── pages/         # Route-level components
│   ├── hooks/         # Custom React hooks
│   ├── services/      # API calls
│   ├── utils/         # Helper functions
│   ├── styles/        # CSS files
│   ├── App.jsx        # Main app component
│   └── main.jsx       # Entry point
├── public/            # Static assets
├── package.json
└── README.md
```

## INITIAL FILES TO CREATE
```bash
# Create additional folders
mkdir src/components src/pages src/hooks src/services src/utils src/styles

# Create basic components
touch src/components/Header.jsx
touch src/components/Footer.jsx
touch src/pages/Home.jsx
touch src/pages/About.jsx
touch src/services/api.js
touch src/utils/helpers.js
```

## PACKAGE.JSON ADDITIONS
```json
{
  "dependencies": {
    "react-router-dom": "^6.8.0"
  },
  "devDependencies": {
    "eslint": "^8.0.0",
    "eslint-plugin-react": "^7.32.0"
  }
}
```

## BASIC APP.JSX STRUCTURE
```jsx
import React from 'react';
import { BrowserRouter as Router, Routes, Route } from 'react-router-dom';
import Header from './components/Header';
import Footer from './components/Footer';
import Home from './pages/Home';
import About from './pages/About';
import './styles/main.css';

function App() {
  return (
    <Router>
      <div className="app">
        <Header />
        <main>
          <Routes>
            <Route path="/" element={<Home />} />
            <Route path="/about" element={<About />} />
          </Routes>
        </main>
        <Footer />
      </div>
    </Router>
  );
}

export default App;
```

## DEVELOPMENT COMMANDS
```bash
npm run dev          # Start development server
npm run build        # Build for production
npm run preview      # Preview production build
npm run lint         # Run ESLint
```

## GIT INITIALIZATION
```bash
git init
git add .
git commit -m "feat: initial React app setup"
```

## NEXT STEPS
1. Set up routing structure
2. Create basic components
3. Add styling system
4. Set up API integration
5. Add testing framework