Deployment to servers is carried out using continuous integration (CI). You can use CI to add build scripts, execute unit/integration tests, check for code validity using `phpcs` and deploy to various environments.

A `ci` (or `bin`) folder should be created in the root of the project folder. That folder will contain all scripts related to building and deploying the app. For example, we can have `build.sh`, `deploy.sh`, `test.sh`, and `qa.sh`.

Here's an example of what a build script looks like, the real implementation may differ for each project.

### Build (`build.sh`)

```sh
#!/usr/bin/env sh

function build() {
  npm install
  composer install --no-dev --no-scripts
  npm run build
}

build
```

### Tests (`test.sh`)

```sh
#!/usr/bin/env bash

set -o errexit
set -o pipefail
set -o nounset
set -o xtrace # debugging purposes

function composer_setup() {
  echo "--> composer setup"
  composer self-update
  composer update
  composer -o dump-autoload
  require infinum/coding-standards-wp
}

function npm_setup() {
  echo "--> npm setup"
  npm install
  npm i -g eslint
  npm install -g sass-unused
  npm install -g stylelint
  npm run build
}

function composer_tests() {
  echo "--> composer tests"
  vendor/bin/phpcs --standard=Infinum --extensions=php --patterns=vendor,tests --processes=4 .
}

function npm_tests() {
  echo "--> npm tests"
  sass-unused '**/*.scss'
  stylelint '**/*.scss'
  eslint --ext .js '**/*.js'
}

function main() {
  composer_setup
  composer_tests
  npm_setup
  npm_tests
}

main "$@"
```

Of course, your friendly devops provide the details of the server.
