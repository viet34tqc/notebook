# Setup ESlint + Prettier + Typescript React

Making a new project ready for development can be a little overwhelming, especially if you use React, Typescript, ESLint, and Prettier, but today I am going to help you set up your React project correctly in a simple way.

Usually, the problem is that every time you have to set up ESLint and Prettier you go to the web search for an article, and this article is probably not updated. Usually, they use a lot of plugins and non-default configurations.

We are going to use the basic configuration of ESLint and Prettier to have fewer headaches.

## What is ESLint?

ESLint is a JavaScript linting open source project and is used to find problems and syntax issues in your code, it will help us find broken logic that would be found only in run time.

To install ESLint:

➜ yarn add -D eslint
or 
➜ npm install eslint --save-dev
After installing the ESLint we have to initialize the config file:

➜ npx eslint --init
Here we are going to answer some questions about our project:

? How would you like to use ESLint? … 
  To check syntax only
▸ To check syntax and find problems
  To check syntax, find problems, and enforce code style
Choose ‘To check syntax and find problems’ because later we are going to use Prettier to enforce code style as well.

? What type of modules does your project use? … 
▸ JavaScript modules (import/export)
  CommonJS (require/exports)
  None of these
Choose JavaScript modules mainly because ReactJS uses them.

? Which framework does your project use? … 
▸ React
  Vue.js
  None of these
Choose ‘React’ as our framework.

? Does your project use TypeScript? ‣ No / Yes
Choose ‘Yes’ for TypeScript.

? Where does your code run? …  (Press <space> to select, <a> to toggle all, <i> to invert selection)
✔ Browser
✔ Node
Choose ‘Browser’ because ReactJS runs on the Browser.

? What format do you want your config file to be in? … 
  JavaScript
  YAML
▸ JSON
Here you are free to choose any of the options, but I am going to use JSON as my format.

The config that you've selected requires the following dependencies:
eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
? Would you like to install them now with npm? ‣ No / Yes
If you are using NPM then just select ‘Yes’, if not select ‘No’ then copy the packages and:

➜ yarn add -D eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest @typescript-eslint/parser@latest
Right now the ESLint package created a .eslintrc file with the format you choose:


Before continuing we have to install TypeScript plugins related to ESLint and to do so:

➜ yarn add -D eslint-plugin-import @typescript-eslint/parser eslint-import-resolver-typescript
If everything went well your eslintrc file should look something like this:

{
    "env": {
        "browser": true,
        "es2021": true
    },
    "extends": [
        "eslint:recommended",
        "plugin:react/recommended",
        "plugin:@typescript-eslint/recommended"
    ],
    "parser": "@typescript-eslint/parser",
    "parserOptions": {
        "ecmaFeatures": {
            "jsx": true
        },
        "ecmaVersion": "latest",
        "sourceType": "module"
    },
    "plugins": [
        "react",
        "@typescript-eslint"
    ],
    "rules": {
    }
}
Let’s install prettier and then edit the eslint and prettier configuration files.

## Prettier

Prettier is a code style formatter, different from ESLint, Prettier only format the code style, and does not look for syntax problems.

Rules like max-len, no-mixed-spaces-and-tabs, keyword-spacing, comma-style are some popular formatting rules in Prettier.

And rules like no-unused-vars, no-extra-bind, no-implicit-globals, prefer-promise-reject-errors are common rules in ESLint.

In this case, I prefer ESLint to search for problems and syntax errors and let the code style for prettier.

To install Prettier:

➜ yarn add -D prettier eslint-config-prettier eslint-plugin-prettier eslint-plugin-react-hooks
After installing you have to create the prettierc file:

➜ touch .prettierrc
At this point you have a blank .prettierrc file and a default eslintrc file, so the next step is to configure the eslintrc file.

Open your eslintrc file.

If you are going to use or you intend to use Jest in your project add the next lines to your ‘env’:

{
    "env": {
        "browser": true,
        "es2021": true,
	"jest": true // Add this line!
    },
	...
}
To use prettier alongside with eslint you need to change the extends object:

{
	...
	"extends": [
	  "eslint:recommended",
	  "plugin:react/recommended",
	  "plugin:@typescript-eslint/recommended",
	  "prettier" // Add this line!
	],
	...
}
The main part of ESLint is the rules objects. Here you can put any rule you think is good for your project and team.

My basic configuration is this:

{
	...
	"rules": {
        "react/react-in-jsx-scope": "off",
        "camelcase": "error",
        "spaced-comment": "error",
        "quotes": ["error", "single"],
        "no-duplicate-imports": "error"
  },
	...
}
If you want to learn more about ESLint rules you can check out rules page.

To use the plugins we have installed, update the plugins object in the eslintrc file:

"plugins": ["react", "react-hooks", "@typescript-eslint", "prettier"],
The last thing to set up in ESLint is the eslint-import-resolver-typescript. Just add the settings key in the eslint configuration file.

{
	...
	"settings": {
    "import/resolver": {
      "typescript": {}
    }
  }
}
Lastly, we are going to set up the .prettierrc file created at the begging of the article.

My basic configuration for prettierrc file is:

{
  "semi": false,
  "tabWidth": 2,
  "printWidth": 100,
  "singleQuote": true,
  "trailingComma": "all",
  "jsxSingleQuote": true,
  "bracketSpacing": true
}
But if you want to learn more check out the options page.

Finally, we have to add the scripts in the package.json:

```json
{
	"scripts": {
	  "lint": "eslint src/**/*.{js,jsx,ts,tsx,json}",
      "lint:fix": "eslint --fix 'src/**/*.{js,jsx,ts,tsx,json}'",
      "format": "prettier --write 'src/**/*.{js,jsx,ts,tsx,css,md,json}' --config ./.prettierrc"
    },
}
```
Basically:

lint: will search for problems, but will not fix
lint fix: will search and try to fix the problems.
format: will call prettier to fix the code style.
At this point, you probably have a fully working ReactJS project with linting. If you encounter some error or problem don’t hesitate in commenting below.