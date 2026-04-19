<p align="center"><a href="https://valkyrja.io" target="_blank">
    <img src="https://raw.githubusercontent.com/valkyrjaio/art/refs/heads/master/long-banner/orange/php.png" width="100%">
</a></p>

# Valkyrja PHP Code Sniffer

Shared PHP Code Sniffer configuration for Valkyrja PHP projects — ruleset
and reusable workflow that enforce the project's file-naming and namespace
conventions across consuming repositories.

<p>
    <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/require/php" alt="PHP Version Require"></a>
    <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/v" alt="Latest Stable Version"></a>
    <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/license" alt="License"></a>
    <a href="https://github.com/valkyrjaio/ci-phpcodesniffer-php/actions/workflows/ci.yml?query=branch%3A26.x"><img src="https://github.com/valkyrjaio/ci-phpcodesniffer-php/actions/workflows/ci.yml/badge.svg?branch=26.x" alt="CI Status"></a>
    <a href="https://scrutinizer-ci.com/g/valkyrjaio/ci-phpcodesniffer-php/?branch=26.x"><img src="https://scrutinizer-ci.com/g/valkyrjaio/ci-phpcodesniffer-php/badges/quality-score.png?b=26.x" alt="Scrutinizer"></a>
    <a href="https://coveralls.io/github/valkyrjaio/ci-phpcodesniffer-php?branch=26.x"><img src="https://coveralls.io/repos/github/valkyrjaio/ci-phpcodesniffer-php/badge.svg?branch=26.x" alt="Coverage Status" /></a>
    <a href="https://shepherd.dev/github/valkyrjaio/ci-phpcodesniffer-php"><img src="https://shepherd.dev/github/valkyrjaio/ci-phpcodesniffer-php/coverage.svg" alt="Psalm Shepherd" /></a>
    <a href="https://sonarcloud.io/summary/new_code?id=valkyrjaio_phpcodesniffer"><img src="https://sonarcloud.io/api/project_badges/measure?project=valkyrjaio_phpcodesniffer&metric=sqale_rating" alt="Maintainability Rating" /></a>
</p>

Usage
-----

The ruleset is defined in `.github/ci/phpcodesniffer/ruleset.xml` and run
via the `phpcodesniffer` Composer script:

```
composer phpcodesniffer
```

This runs `phpcs` against `src/` and `tests/` using the project ruleset.
The `src/` run suppresses warnings (`-n`); the `tests/` run additionally
shows sniff source codes (`-n -s`).

To see all warnings (not just errors), run:

```
cd .github/ci/phpcodesniffer && composer warnings
```

Ruleset
-------

### Excluded Paths

The following paths are excluded from all checks:

| Path                |
|---------------------|
| `.github/`          |
| `.phpunit.cache/`   |
| `build/`            |
| `coverage-html/`    |
| `docs/`             |
| `functions/`        |
| `pre-commit-hooks/` |
| `vendor/`           |

### Rules

#### `SlevomatCodingStandard.Files.TypeNameMatchesFileName`

Enforces that each type (class, interface, trait, enum) is declared in a
file whose name matches the type name, rooted at the configured namespaces:

| Root path      | Namespace        |
|----------------|------------------|
| `src/Valkyrja` | `Valkyrja`       |
| `tests/Tests`  | `Valkyrja\Tests` |

The following directories are skipped when resolving file paths:

`.github`, `docs`, `functions`, `pre-commit-hooks`, `vendor`

#### `SlevomatCodingStandard.Namespaces.ReferenceUsedNamesOnly`

All referenced names must appear in `use` statements — fully qualified names
are not permitted inline:

| Property                                     | Value   | Effect                                                                  |
|----------------------------------------------|---------|-------------------------------------------------------------------------|
| `allowPartialUses`                           | `false` | Partial uses (e.g. `use Foo\Bar` referencing `Bar\Baz`) are not allowed |
| `allowFullyQualifiedNameForCollidingClasses` | `false` | Colliding class names must be aliased, not referenced fully qualified   |
| `searchAnnotations`                          | `true`  | Annotations in docblocks are also checked for unimported names          |

Workflows
---------

The [`_workflow-call.yml`](.github/workflows/_workflow-call.yml) reusable
workflow runs PHP Code Sniffer against the calling repository's source. It
is designed to be called from other repositories via `workflow_call`.

### Inputs

| Input              | Type    | Default                       | Description                                                                                                                                           |
|--------------------|---------|-------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|
| `paths`            | string  | —                             | **Required.** YAML filter spec with two keys: `ci` (CI config files that trigger a base-branch fetch) and `files` (all files that trigger the check). |
| `post-pr-comment`  | boolean | `true`                        | Post a PR comment on failure and remove it on success. Disable when the calling workflow handles its own reporting.                                   |
| `composer-options` | string  | `''`                          | Extra flags passed to every `composer install` step (e.g. `--ignore-platform-req=ext-openswoole`).                                                    |
| `php-version`      | string  | `'8.4'`                       | PHP version to use.                                                                                                                                   |
| `ci-directory`     | string  | `'.github/ci/phpcodesniffer'` | Path to the CI directory containing `composer.json` and the tool config.                                                                              |
| `extensions`       | string  | `'mbstring, intl'`            | PHP extensions to install via `shivammathur/setup-php`.                                                                                               |

### Usage

```yaml
jobs:
  phpcodesniffer:
    uses: valkyrjaio/ci-phpcodesniffer-php/.github/workflows/_workflow-call.yml@26.x
    permissions:
      pull-requests: write
      contents: read
    with:
      php-version: '8.4'
      paths: |
        ci:
          - '.github/ci/phpcodesniffer/**'
          - '.github/workflows/phpcodesniffer.yml'
        files:
          - '.github/ci/phpcodesniffer/**'
          - '.github/workflows/phpcodesniffer.yml'
          - 'src/**/*.php'
          - 'composer.json'
    secrets: inherit
```

`secrets: inherit` is required to pass the `VALKYRJA_GHA_APP_ID` and
`VALKYRJA_GHA_PRIVATE_KEY` org secrets used for PR comments.

Contributing
------------

See [`CONTRIBUTING.md`][contributing url] for the submission process and
[`VOCABULARY.md`][vocabulary url] for the terminology used across Valkyrja.

Security Issues
---------------

If you discover a security vulnerability, please follow our
[disclosure procedure][security vulnerabilities url].

License
-------

Licensed under the [MIT license][MIT license url]. See
[`LICENSE.md`](./LICENSE.md).

[contributing url]: https://github.com/valkyrjaio/.github/blob/master/CONTRIBUTING.md

[vocabulary url]: https://github.com/valkyrjaio/.github/blob/master/VOCABULARY.md

[security vulnerabilities url]: https://github.com/valkyrjaio/.github/blob/master/SECURITY.md

[MIT license url]: https://opensource.org/licenses/MIT
