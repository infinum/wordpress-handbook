# Deployment and build scripts

Deployment to servers is done using continuous integration (CI). You can use CI to add build scripts, execute unit/integration tests, check for code validity using `phpcs` and deploy to various environments.

In the root of the project folder, a `ci` folder should be made. In that folder will be all the scripts related to building the app and deploying the app. For example we can have `build.sh`, `deploy.sh`, `test.sh` and `qa.sh`.

Example of build script looks like, the real implementation may differ per project basis.

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

Of course, the details of the server are provided by your friendly devops.
