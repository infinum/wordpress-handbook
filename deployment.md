# Deployment and build scripts

Deployment to servers is done using continuous integration (CI). You can use CI to add build scripts, execute unit/integration tests, check for code validity using `phpcs` and deploy to various environments.

Build and deploy scripts are located in `bin` folder inside root of your project and usually look like this:

### Build (`build.sh`)

```sh
#!/usr/bin/env bash

set -o errexit
set -o pipefail
set -o nounset
# set -o xtrace

function composer_setup() {
  echo "--> composer setup"
  composer self-update
  composer update
  composer require --dev jakub-onderka/php-parallel-lint
  composer require --dev jakub-onderka/php-console-highlighter
}

function composer_tests() {
  echo "--> composer tests"
 vendor/bin/parallel-lint --exclude wp-admin --exclude wp-content --exclude ./vendor/jakub-onderka .
}

function main() {
  composer_setup
  composer_tests
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

## custom functions block ###

function main() {
  bash ./_build.sh
  ssh-keyscan -p 123 server-instance.addres.test >> ~/.ssh/known_hosts
  rsync -azqe 'ssh -p 123' --exclude-from='./ci-exclude.txt' ./ test_user@server-instance.addres.test:/home/test_user/www/server-instance.addres.test/
}
main "$@"
```

Of course, the details of the server are provided by your friendly devops.
