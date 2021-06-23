# vroom
Wrapper for make to help setup/execute standard destroy/setup/run/watch commands

## Installation

``` bash
brew install flare576/scripts/vroom
```

Recommendation: Add `.vroom` to your `~/.gitignore` file unless you want to share your configuration

## Use

``` bash
$ vroom         # Run your project
$ vroom -d      # Tear down your project
$ vroom -s      # Setup your project
$ vroom -w      # Run your watch command
$ vroom -sr     # Setup, then run your project
$ vroom -f      # DO ALLL THE THINGS (destroy, setup, run, watch)
```

## Config

Running `vroom` inside of a project for the first time will scan your project for build/package
managers to determine default commands:
  - Docker Compose
    - Trigger files: docker-compose.yml
      - Note: if docker-compose-local.yml is found, `-f docker-compose-local.yml` is added to the
          calls
    - Setup: `docker compose build`
    - Run: `docker compose up -d`
  - Docker
    - Trigger files: Dockerfile
    - Setup: `docker build -t <project_name>:1 .`
    - Run: `docker run <project_name>:1`
  - NPM
    - Trigger files: package.json
    - Setup:
      - `npm ci` - if package-lock.json
      - `npm install` - if NO package-lock.json
    - Run: Will scan available scripts and use the first it finds of:
      - start:local
      - start:dev
      - start
  - Yarn
    - Trigger files: yarn.lock
    - Setup: `yarn install`
    - Run: Will scan available scripts and use the first it finds of:
      - start:local
      - start:dev
      - start
  - Python
    - Trigger files: Pipfile
    - Setup: `pipenv install`
    - Run: ¯\_(ツ)_/¯ (If I find more projects using __main__.py, I'll update to that)
  - PHP
    - Trigger files: composer.json
    - Setup: `composer install`
    - Run: Will scan available scripts and use the first it finds of:
      - start:local
      - start:dev
      - start

Destroy and Watch commands are always defaulted to no-op commands. You can also run `vroom -e` as
the initial command to open your $EDITOR with a .vroom template.

### .vroom Files

Early versions of vroom used a custom file format... then I discovered
[make](https://www.gnu.org/software/make/) and realized I was basically recreating it.

.vroom files are now standard [makefile](https://www.gnu.org/software/make/manual/make.html) name
that should prevent a normal `make` invocation from finding it. In normal makefile conventions, if
you want to use `vroom` as a full replacement for the make command, you can add an
[include](https://www.gnu.org/software/make/manual/html_node/Include.html#Include) to the .vroom for
your normal make file(s).
