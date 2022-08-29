# Node.js - Project Starter with TypeScript

## Git

If the project is not already part of a git repository then initialize it:

```shell
git init
```

## npm

```shell
npm init
```

## TypeScript

### Install TypeScript binary

```shell
npm install --save-dev typescript
```

### Configure 

Create the basic `tsconfig.json`

```shell
tsc --init
```

Edit `tsconfig.json` and make the following changes for Node.js v16

```json
{
  "compilerOptions": {
    "target": "es2021",
    "lib": ["es2021"],

    "rootDir": "./src",
    "moduleResolution": "node",

    "sourceMap": true,
    "outDir": "./dist"
  },
  "include": [
    "src/**/*.ts"
  ],
  "exclude": [
    "node_modules",
    "**/*.spec.ts"
  ]
}
```

## Useful configurations & scripts

### Code Style and Formatting

Check the Prettier guide ;)

### Cold reloading

```shell
npm install --save-dev ts-node nodemon
```

Add a `nodemon.json` config.

```json
{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "ts-node ./src/index.ts"
}
```

And then to run the project, all we have to do is run nodemon. Let's add a script for that.

```json
{
  "scripts": {
    "start:dev": "nodemon"
  }
}
```

### Create production builds

```shell
npm install --save-dev rimraf
```

And then, add this to your package.json.

```json
{
  "scripts": {
    "build": "rimraf ./build && tsc"
  }
}
```

### Production startup script

In order to start the app in production, all we need to do is run the build command first, and then execute the compiled JavaScript at build/index.js.

The startup script looks like this.

```json
{
  "scripts": {
    "start": "npm run build && node build/index.js"
  }
}
```

## Add Node.js types

```shell
npm install --save-dev @types/node
```

## [OPTIONAL] Install Express
 
```shell
npm install --save-prod express
npm install --save-dev @types/express
```

## [OPTIONAL] Install dotenv

```shell
npm install --save-prod dotenv
npm install --save-dev @types/dotenv
```

## Source

- [TSConfig Reference](https://www.typescriptlang.org/tsconfig)
- [How to Setup a TypeScript + Node.js Project](https://khalilstemmler.com/blogs/typescript/node-starter-project/)
- [Simple TypeScript Starter | 2022](https://github.com/stemmlerjs/simple-typescript-starter)
- [TypeScript Express tutorial](https://wanago.io/2018/12/03/typescript-express-tutorial-routing-controllers-middleware/)
- [How to Set Up a Node.js Project with TypeScript](https://blog.appsignal.com/2022/01/19/how-to-set-up-a-nodejs-project-with-typescript.html)
- [GitHub - tsconfig / bases](https://github.com/tsconfig/bases)
- [WebMound - Best Setup to Use TypeScript with Node.js and Express Project](https://www.webmound.com/best-typescript-setup-with-nodejs-express-project/)
- [Medium - How To Set Up a TypeScript Project](https://medium.com/codex/how-to-set-up-a-typescript-project-2022-2d06ddbe3f17)
- 
