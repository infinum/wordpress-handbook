Linters are a great way to keep your project clean and remove any code cruft.

In our projects we include several static analysis linters.

### PHP

#### PHPCS

[PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer) is a script that will tokenize your PHP files and based on selected rulesets, detect violations of set coding standards.

It can tokenize and check JS and CSS, but we don't use it for checking those file types.

PHPCS doesn't know about context. It just tokenizes your code, and checks that tokenized code against sniffs. Sniffs are just rules that will check if tokenized code is valid. For instance if you disallow Yoda conditions, tokenized code will look different for

```php
if ('yes' === $someCode) { //... }
```

vs

```php
if ($someCode  === 'yes') { //... }
```

We are using [Eightshift Coding Standards](https://github.com/infinum/eightshift-coding-standards) for our projects. Check your project `composer.json` file for the correct command, but you can usually run them with the following command:

```bash
composer test:standards
```

#### PHPStan

[PHPStan](https://phpstan.org) is a static analysis tool that will parse your code (not tokenize like phpcs), and try to infer types that go in your code and out of your code, either from doc-blocks or arguments, and warn you if something is off. For instance, passing integer from one function to another function as argument when the expected argument type is a string.

It does what compilers do in compiled languages like Java, but faster, and you can configure it or extend it.

Because of that, it goes perfectly with PHPCS. Since it can understand context it's an essential tool for preventing bugs that can happen by type juggling for instance.

We are using [PHPStan](https://phpstan.org) for our projects. Check your project `composer.json` file for the correct command, but you can usually run them with the following command:

```bash
composer test:types
```

### JavaScript

For JS linting we are using [ESLint](https://eslint.org/). It's a static analyzer. It works similarly to PHPStan, parsing code to abstract syntax tree (AST) and finding patterns in code.

We are using [Eightshift ESLint](https://github.com/infinum/eightshift-frontend-libs-tailwind/tree/main/linters) for our projects. Check your project `package.json` file for the correct command, but you can usually run them with the following command:

```bash
bun lintJs
```

### CSS/SCSS

For CSS/SCSS files we are also using [ESlint](https://eslint.org/blog/2025/02/eslint-css-support/).
CSS linting is accomplished using the [@eslint/css](https://npmjs.com/package/@eslint/css) plugin, which is an officially supported language plugin.

We are using [Eightshift CSS Linter](https://github.com/infinum/eightshift-frontend-libs-tailwind/tree/main/linters) for our projects. Check your project `package.json` file for the correct command, but you can usually them with the following command:

```bash
bun lintStyle
```

### Prettier

Prettier is a code formatter that can be used to format your code according to the rules of the project. We are using [Eightshift Prettier](https://github.com/infinum/eightshift-frontend-libs-tailwind/blob/main/.prettierrc) for our projects. Check your project `.prettierrc` file, as formatting is done automatically on save in your editor.

To keep class lists consistent (and avoid noisy diffs), enable [Automatic Class Sorting with Prettier](https://tailwindcss.com/blog/automatic-class-sorting-with-prettier) in your project.
