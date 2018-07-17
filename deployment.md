# Deployment and build scripts

Deployment to servers is done using continuous integration (CI). You can use CI to add build scripts, execute unit/integration tests, check for code validity using `phpcs` and deploy to various environments.

In the root of the project folder, a `ci` folder should be made. In that folder will be all the scripts related to building the app and deploying the app. For example we can have `build.sh`, `deploy.sh`, `test.sh` and `qa.sh`.

Example of build script looks like, the real implementation may differ per project basis.

### Build (`build.sh`)

```sh
#!/usr/bin/env bash

set -o errexit
set -o pipefail
set -o nounset
set -o xtrace # debugging purposes

function composer_setup() {
  echo "--> composer setup"
  composer install --no-dev --no-scripts
  composer update --no-dev --no-scripts
  composer -o dump-autoload
}

function npm_setup() {
  echo "--> npm setup"
  npm install
  npm run build
}

function main() {
  composer_setup
  npm_setup
}

main "$@"
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

### Deploy (`deploy.sh`)

```sh
#!/usr/bin/env bash

set -o errexit
set -o pipefail
set -o nounset
# set -o xtrace # debugging purposes

## custom functions block ##

function main() {
  bash build.sh
  ssh-keyscan -p 123 server-instance.addres.test >> ~/.ssh/known_hosts
  rsync -azqe 'ssh -p 123' --exclude-from='./ci-exclude.txt' ./ test_user@server-instance.addres.test:/home/test_user/www/server-instance.addres.test/
}

main "$@"
```

Of course, the details of the server are provided by your friendly devops.
