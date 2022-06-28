# Development - Configure Prettier

## Check Node.js Installation

```
node -v
npm -v
```

## Install Prettier

`npm install prettier -D`

## Configure

Next, I recommend you set up a configuration file. This has the added benefit that regardless of what editor your team members use, they will all reference the same configuration file.  
Prettier has defaults, and so you only need to specify the options you wish to override. However, I like to be explicit so that any developer on your team knows exactly what rules are being applied. 

Create a `.prettierrc` file:

```json
{
  "printWidth": 120,
  "overrides": [
    {
      "files": ["*.html"],
      "options": {
        "tabWidth": 4,
        "singleAttributePerLine": true
      }
    },
    {
      "files": ["*.ts"],
      "options": {
        "singleQuote": true
      }
    }
  ]
}
```

The defaults we are overriding here is the printWidth (default is 80), enforcing single quotes instead of double quotes (for TypeScript files) and allow only one attribute per line (for HTML files).

## Ignoring Code

By default, it is :

```
**/.git
**/.svn
**/.hg
**/node_modules
```

Create `.prettierignore` and add:
```
**/build
**/coverage
**/e2e
```

## IDE

### VS Code

Install [Prettier Formatter for Visual Studio Code](https://github.com/prettier/prettier-vscode).  
Then, in your user settings, make sure you enable formatting on save:

```
“editor.formatOnSave”: true
```

### WebStorm

See the official guide [JetBrains WebStorm - Prettier](https://www.jetbrains.com/help/webstorm/prettier.html) and [WebStorm Setup](https://prettier.io/docs/en/webstorm.html)

## Add scripts to package.json

```
"scripts": {
  "prettier:check": "prettier --list-different ./src/{app,environments,assets/scss}/**/*.{ts,html,scss}",
  "prettier:fix": "prettier --write ./src/{app,environments,assets/styles}/**/*.{ts,html,scss}"
}
```

## Sources

- [Prettier Docs](https://prettier.io/docs/en/index.html)
- [Setting up Prettier in an Angular CLI Project](https://medium.com/@victormejia/setting-up-prettier-in-an-angular-cli-project-2f50c3b9a537)
- [Set Up Prettier for Angular in 30 Minutes](https://levelup.gitconnected.com/setup-prettier-on-angular-in-30-minutes-12b2ed85e7b7)
