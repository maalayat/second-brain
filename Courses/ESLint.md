## Install
```
npm init @eslint/confignpm init @eslint/config
```

### Adding codely configuration

- [eslint-config-codely](https://www.npmjs.com/package/eslint-config-codely)

```
npm install --dev eslint eslint-config-codely
```

### Adding `.eslintrc.js` to the root project
```
{
  extends: [ "eslint-config-codely/typescript" ],
  parserOptions: {
    project: ["./tsconfig.json"],
  },
}
```

### Adding scripts to our `package.json`
```
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint --fix ."
  },
}
```

### Autofix on indivual rules
- [eslint-nibble](https://github.com/IanVS/eslint-nibble)

## Configuration

### env
```
"env": {
	"es6": true,
  "browser": true,
  "node": true
}
```

## Typescript

### eslintrc
```
"parser": "@typescript-eslint/parser",
"parserOptions": {
  "sourceType": "module",
  "ecmaFeatures": {
    "jsx": true
  }
}
```

### package.json
```
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

## Rules
```
"extends": ["eslint:recommended", "plugin:@typescript-eslint/recommended"],
"plugins": ["@typescript-eslint"],
"rules": {
  "semi": ["error", "always"],
  "quotes": ["error", "double"],
  "@typescript-eslint/no-non-null-assertion": "off"
},
```

## Overrides
```
overrides: [
  {
    files: ["*.js"],
    rules: {
      "@typescript-eslint/no-unsafe-assignment": "off",
      "@typescript-eslint/no-unsafe-call": "off",
      "@typescript-eslint/no-unsafe-member-access": "off",
      "@typescript-eslint/no-var-requires": "off",
    },
  },
],
```

## Archivos de configuración ESLint

-   Debe tener instalada la dependencia eslint

```
npm install --dev eslint
```

-   Debe existir un archivo .eslintrc.js en la raíz de nuestro proyecto

-   Debe haber en nuestro package.json los scripts para pasar ESLint y arreglar los problemas automáticamente:

```
{
  "scripts": {
    "lint": "eslint .",
    "lint:fix": "eslint --fix ."
  },
}
```

### Configuración ESLint en el IDE

#### VSCode

Para Visual Studio Code, instalar la extensión [VS Code ESLint extension](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint) y ajustar las preferencias para validar usando ESLint, y aplicar el autofix al guardar.

#### JetBrains (WebStorm, IntelliJ IDEA)

El [plugin ESLint](https://plugins.jetbrains.com/plugin/7494-eslint) viene instalado y activado por defecto, pero si por aluguna razón no está activado se puede modificar este ajuste:

## Configurar [typescript-eslint](https://typescript-eslint.io/)

> typescript-eslint enables ESLint to run on TypeScript code. It brings in the best of both tools to help you write the best JavaScript or TypeScript code you possibly can.

### Instalar

```
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin
```

### Configurar

```
module.exports = {
  extends: ['eslint:recommended', 'plugin:@typescript-eslint/recommended'],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  root: true,
};
```

### Ejecutar

```
npx eslint .
```

## Prettier

### Prettier vs. Linters

[https://prettier.io/docs/en/comparison.html](https://prettier.io/docs/en/comparison.html)

> In other words, use **Prettier for formatting** and **linters for catching bugs!**

## Reglas interesantes

### Reglas de imports

-   [https://www.npmjs.com/package/eslint-plugin-import](https://www.npmjs.com/package/eslint-plugin-import)
-   [eslint-plugin-simple-import-sort](https://github.com/lydell/eslint-plugin-simple-import-sort/)
-   [eslint-plugin-unused-imports](https://github.com/sweepline/eslint-plugin-unused-imports)
-   [https://www.npmjs.com/package/eslint-plugin-folders](https://www.npmjs.com/package/eslint-plugin-folders)

```
"import/first": "error",
"import/newline-after-import": "error",
"import/no-duplicates": "error",
"import/no-unresolved": "error",
"simple-import-sort/exports": "error",
"simple-import-sort/imports": "error",
```

### Regla para imports perzonalizado

```
"simple-import-sort/imports": [
  "error",
  {
    groups: [
      // Imports que producen side effects: `import "./setup";`
      ["^\\u0000"],
      // Paquetes: `import fs from "fs";`
      ["^@?\\w"],
      // Paquetes internos.
      ["^(@|@codely)(/.*|$)"],
      // Imports de niveles superiores.
      ["^\\.\\.(?!/?$)", "^\\.\\./?$"],
      // Otros imports relativos. De la misma carpeta y `.` al final.
      ["^\\./(?=.*/)(?!/?$)", "^\\.(?!/?$)", "^\\./?$"],
      // Imports de estilo.
      ["^.+\\.s?css$"],
    ],
  },
],
```

### Reglas de imports y variables sin usar

```
"unused-imports/no-unused-imports": "error",
"unused-imports/no-unused-vars": [
  "warn",
  {
    vars: "all",
    varsIgnorePattern: "^_",
    args: "after-used",
    argsIgnorePattern: "^_",
  },
],
```

### Regla de nombrados de carpetas

```
"folders/match-regex": ["error", "^[a-z-]+$", `${process.cwd()}/`]
```

### Reglas para posibles bugs

[https://eslint.org/docs/latest/rules/#possible-problems](https://eslint.org/docs/latest/rules/#possible-problems)

```
"array-callback-return": ["error", { checkForEach: true }],
"no-await-in-loop": "error",
"no-constant-binary-expression": "error",
"no-constructor-return": "error",
"no-promise-executor-return": "error",
"no-self-compare": "error",
"no-template-curly-in-string": "error",
"no-unmodified-loop-condition": "error",
"no-unreachable-loop": "error",
"no-unused-private-class-members": "error",
"no-use-before-define": "error",
"require-atomic-updates": "error",
```

### Reglas para buenas prácticas

[https://eslint.org/docs/latest/rules/#suggestions](https://eslint.org/docs/latest/rules/#suggestions)

```
camelcase: "error",
eqeqeq: "error",
"new-cap": "error",
"no-array-constructor": "error",
"no-console": ["error", { allow: ["error"] }],
"no-else-return": ["error", { allowElseIf: false }],
"no-extend-native": "error",
"no-lonely-if": "error",
"no-param-reassign": "error",
"no-return-assign": "error",
"no-throw-literal": "error",
"no-var": "error",
"object-shorthand": "error",
"prefer-const": "error",
"prefer-rest-params": "error",
"prefer-spread": "error",
"prefer-template": "error",
radix: "error",
yoda: "error",
```

### Otras reglas

- [https://github.com/jfmengels/eslint-plugin-fp](https://github.com/jfmengels/eslint-plugin-fp)
- [https://github.com/amwmedia/eslint-plugin-woke](https://github.com/amwmedia/eslint-plugin-woke)
- [https://github.com/kantord/eslint-plugin-write-good-comments](https://github.com/kantord/eslint-plugin-write-good-comments)
- [https://github.com/dustinspecker/awesome-eslint](https://github.com/dustinspecker/awesome-eslint)

## Configuración recomendada

### Dependencias:

-   eslint  
-   @typescript-eslint/parser
-   @typescript-eslint/eslint-plugin
-   eslint-config-prettier    
-   eslint-plugin-prettier
-   Husky

```
npm install --dev eslint
npm install --save-dev @typescript-eslint/parser @typescript-eslint/eslint-plugin
npm install --save-dev eslint-config-prettier
npm install --save-dev eslint-plugin-prettier
npm install --save-dev husky
```

### Archivo .eslintrc.js

```
module.exports = {
  root: true,
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  extends: [
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
  ],
  rules: {
    '@typescript-eslint/no-explicit-any': 'warn',
  },
};
```

### Archivo .eslintignore

```
/dist
node_modules
.git
/coverage
/kafka/data
```

### Archivo .prettierrc

```
{
  "singleQuote": true,
  "trailingComma": "all"
}
```

### Scripts

```
"scripts": {
  "lint": "eslint .",
  "lint:fix": "eslint --fix ."
  
  "lint": "eslint --ignore-path .eslintignore --ext .js,.ts ."
  
  "lint": "eslint --ext .js,.ts .",
  "lint": "eslint . --ext .ts",
  "format": "prettier --ignore-path .gitignore --write \"**/*.+(js|ts|json)\""
  "format": "prettier --config .prettierrc 'src/**/*.ts' --write",
}
```

### Hooks

#### Husky

```
npm install husky --save-dev

npm pkg set scripts.prepare="husky install"
npm run prepare

npx husky add .husky/pre-commit "npm run lint"
```

##### lint-staged

```
npm install --save-dev lint-staged
```

```
"lint-staged": {
  "*.ts": "eslint --fix"
}
```

## Blogs

-   [https://blog.logrocket.com/linting-typescript-eslint-prettier/](https://blog.logrocket.com/linting-typescript-eslint-prettier/)
-   [https://khalilstemmler.com/blogs/tooling/enforcing-husky-precommit-hooks/](https://khalilstemmler.com/blogs/tooling/enforcing-husky-precommit-hooks/)

## Documentación

-   [ESLint](https://eslint.org/)
-   [eslint-nibble](https://github.com/IanVS/eslint-nibble)
-   [https://typescript-eslint.io/](https://typescript-eslint.io/)
-   [https://prettier.io/docs/en/comparison.html](https://prettier.io/docs/en/comparison.html)
-   [ESLint configuration](https://eslint.org/docs/user-guide/configuring)  
-   [ESLint rules](https://eslint.org/docs/rules/)
-   [Typescript-eslint rules](https://typescript-eslint.io/rules)
-   [husky](https://typicode.github.io/husky/#/)
-   [https://www.npmjs.com/package/lint-staged](https://www.npmjs.com/package/lint-staged)
-  [Law of Triviality](https://en.wikipedia.org/wiki/Law_of_triviality)
-   [Bikeshedding](https://en.wiktionary.org/wiki/bikeshedding)
-   [StandardJS](https://standardjs.com/)
-   [Airbnb config](https://www.npmjs.com/package/eslint-config-airbnb)
-   [Airbnb JS guide](https://github.com/airbnb/javascript)
-   [Codely config](https://github.com/CodelyTV/eslint-config-codely/)