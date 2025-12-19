Linters are a great way to keep your project clean and remove any code cruft.

In our projects we include several static analysis linters.

### PHP

#### PHP_CodeSniffer

[PHP_CodeSniffer](https://github.com/PHPCSStandards/PHP_CodeSniffer) is a script that will tokenize your PHP files and based on selected rulesets, detect violations of set coding standards.


It also includes the `phpcbf` script that will automatically correct coding standard violations when possible.

It can tokenize and check JS and CSS, but we don't use it for checking those file types.

PHPCS doesn't know about context. It just tokenizes your code, and checks that tokenized code against sniffs. Sniffs are just rules that will check if tokenized code is valid. For instance if you disallow Yoda conditions, tokenized code will look different for

```php
if ('yes' === $someCode) { //... }
```

vs

```php
if ($someCode  === 'yes') { //... }
```

#### PHPStan

[PHPStan](https://phpstan.org) is a static analysis tool that will parse your code (not tokenize like phpcs), and try to infer types that go in your code and out of your code, either from doc-blocks or arguments, and warn you if something is off. For instance, passing integer from one function to another function as argument when the expected argument type is a string.

It does what compilers do in compiled languages like Java, but faster, and you can configure it or extend it.

Because of that, it goes perfectly with PHPCS. Since it can understand context it's an essential tool for preventing bugs that can happen by type juggling for instance.

### JavaScript

For JS linting we are using [ESLint](https://eslint.org/). It's a static analyzer. It works similarly to PHPStan, parsing code to abstract syntax tree (AST) and finding patterns in code.

### CSS

For CSS files we are also using [ESlint](https://eslint.org/blog/2025/02/eslint-css-support/).
CSS linting is accomplished using the [@eslint/css](https://npmjs.com/package/@eslint/css) plugin, which is an officially supported language plugin.

### Tailwind

To keep class lists consistent (and avoid noisy diffs), enable [Automatic Class Sorting with Prettier](https://tailwindcss.com/blog/automatic-class-sorting-with-prettier).

### Running linters

You can run individual linter or all of them at once.

Once you've set up a project, you'll have `bun` and `composer` scripts at your disposal. Running

```bash
bun run lint
```
will run JS, CSS and coding standards check.

You have individual `bun` checks:

```bash
bun run lintJs
bun run lintStyle
```

For composer checks you can run:

```bash
composer test:types // PHPStan
composer test:standards // PHPCS
```

_Note: These scripts can be renamed in your project._
