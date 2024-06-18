## Setting Up an Express API with TypeScript and Pre-commit Hooks Using Husky

## TL;DR

Setting up an Express API with TypeScript can greatly enhance your development experience by providing strong typing and modern JavaScript features. This post covers the initial setup of an Express server with TypeScript, integration of linting with ESLint and Prettier, and ensuring code quality with Husky pre-commit hooks. For the complete code and setup details, please visit the [GitHub repository](https://github.com/etorralba/express-ts-api-boilerplate).

Today, I am configuring an Express server with TypeScript and setting up pre-commit hooks using Husky to ensure code quality. This setup will include linting and testing the code before it's committed to version control, which is crucial for maintaining code standards and reducing bugs.

## Create an Express Project

I'm establishing a mono repo for both the API and the client. Here’s the initial structure for the API:
```
├── LICENSE
├── README.md
├── api
│   ├── eslint.config.mjs
│   ├── .eslintrc.json
│   ├── .prettierrc
│   ├── node_modules
│   ├── package-lock.json
│   ├── package.json
│   ├── src
│   └── tsconfig.json
├── node_modules
│   └── husky
├── package-lock.json
└── package.json
```

### Configuring Project Dependencies

1. Navigate to the API directory and initialize the Node.js project:
```bash
cd api/
npm init -y
```
2. Install necessary libraries including Express and TypeScript:
```bash
npm install express
npm install -D @types/express @types/node ts-node typescript nodemon
```
3. Create the following configuration files:
- **tsconfig.json**: This file sets up TypeScript for our project.
```json
{
  "compilerOptions": {
    "target": "es2018",
    "module": "commonjs",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "moduleResolution": "node",
    "resolveJsonModule": true,
    "noImplicitReturns": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "removeComments": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "**/*.spec.ts"]
}
```

- **src/server.ts**: This is the main server file using Express.
```typescript
import express, { Request, Response } from 'express';

const app = express();
const PORT = process.env.PORT || 3000;

app.get('/', (req: Request, res: Response) => {
  res.send('Hello World from Express and TypeScript!');
});

app.listen(PORT, () => {
  console.log(`Server running on http://localhost:${PORT}`);
});
```

4. Update the `npm` scripts for easier development and build processes:
```json
"scripts": {
  "build": "tsc",
  "start": "node dist/server.js",
  "dev": "nodemon --watch 'src/**/*.ts' --exec 'ts-node' src/server.ts"
},
```

## Adding Linting to the Project

Linting helps maintain a standardized style across the project. Let's set up ESLint and Prettier:

1. Install ESLint:
```bash
npm install -D eslint@8.57.0
```
2. Initialize ESLint and configure it:
```bash
npx eslint --init
```
Choose the appropriate options for Node.js environment with ES modules.

3. Update `package.json` with a lint command:
```json
"scripts": {
  "lint": "eslint src --fix"
},
```

4. Install Prettier and its ESLint integration:
```bash
npm install -D prettier eslint-config-prettier eslint-plugin-prettier
```
5. Configure Prettier and update ESLint settings:
```json
// .prettierrc
{
  "semi": true,
  "singleQuote": true
}
```

```json
// .eslintrc.json
{
  "extends": ["plugin:prettier/recommended"],
  "plugins": ["prettier"],
  "rules": {
    "prettier/prettier": "error"
  }
}
```

6. Add a format script to `package.json`:
```json
"scripts": {
  "format": "prettier --write \"src/**/*.{js,jsx,ts,tsx,json,css,scss,md}\""
},
```

## Adding Pre-commit Hooks

Pre-commit hooks help enforce code standards by running lint and format checks before committing:

1. Install Husky in the root folder:
```bash
npm install -D husky
```
2. Initialize Husky to create the `.husky/` directory:
```bash
npx husky
```
3. Add a `pre-commit` file inside the `.husky` directory and provide the following command to lint and format code:
```
cd api && npm run lint && npm run format
```

## Further Improvements

- **Continuous Integration**: Integrate with a CI/CD pipeline to run tests and deploy automatically.
- **Testing**: Set up unit and integration tests using Jest or Mocha to ensure code quality and functionality.
- **Dockerization**: Containerize the application with Docker for easier deployment and scalability.

This setup provides a robust foundation for developing an Express API with TypeScript, emphasizing code quality and developer productivity.