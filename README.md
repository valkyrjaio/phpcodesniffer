<p align="center"><a href="https://valkyrja.io" target="_blank">
    <img src="https://raw.githubusercontent.com/valkyrjaio/art/refs/heads/master/full-logo/orange/php.png" width="400">
</a></p>

# Valkyrja PHP Code Sniffer

PHP Code Sniffer rules for the Valkyrja project.

<p>
    <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/require/php" alt="PHP Version Require"></a>
    <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/v" alt="Latest Stable Version"></a>
    <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/license" alt="License"></a>
    <!-- <a href="https://packagist.org/packages/valkyrja/phpcodesniffer"><img src="https://poser.pugx.org/valkyrja/phpcodesniffer/downloads" alt="Total Downloads"></a>-->
    <a href="https://scrutinizer-ci.com/g/valkyrjaio/phpcodesniffer/?branch=master"><img src="https://scrutinizer-ci.com/g/valkyrjaio/phpcodesniffer/badges/quality-score.png?b=master" alt="Scrutinizer"></a>
    <a href="https://coveralls.io/github/valkyrjaio/phpcodesniffer?branch=master"><img src="https://coveralls.io/repos/github/valkyrjaio/phpcodesniffer/badge.svg?branch=master" alt="Coverage Status" /></a>
    <a href="https://shepherd.dev/github/valkyrjaio/phpcodesniffer"><img src="https://shepherd.dev/github/valkyrjaio/phpcodesniffer/coverage.svg" alt="Psalm Shepherd" /></a>
    <a href="https://sonarcloud.io/summary/new_code?id=valkyrjaio_phpcodesniffer"><img src="https://sonarcloud.io/api/project_badges/measure?project=valkyrjaio_phpcodesniffer&metric=sqale_rating" alt="Maintainability Rating" /></a>
</p>

Build Status
------------

<table>
    <tbody>
        <tr>
            <td>Linting</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpcodesniffer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpcodesniffer.yml/badge.svg?branch=master" alt="PHP Code Sniffer Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpcsfixer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpcsfixer.yml/badge.svg?branch=master" alt="PHP CS Fixer Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Coding Rules</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phparkitect.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phparkitect.yml/badge.svg?branch=master" alt="PHPArkitect Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/rector.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/rector.yml/badge.svg?branch=master" alt="Rector Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Static Analysis</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpstan.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpstan.yml/badge.svg?branch=master" alt="PHPStan Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/psalm.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/psalm.yml/badge.svg?branch=master" alt="Psalm Build Status"></a>
            </td>
        </tr>
        <tr>
            <td>Testing</td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpunit.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/phpunit.yml/badge.svg?branch=master" alt="PHPUnit Build Status"></a>
            </td>
            <td>
                <a href="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/validate-composer.yml?query=branch%3Amaster"><img src="https://github.com/valkyrjaio/phpcodesniffer/actions/workflows/validate-composer.yml/badge.svg?branch=master" alt="Validate Composer Build Status"></a>
            </td>
        </tr>
    </tbody>
</table>

## Usage

The ruleset is defined in `.github/ci/phpcodesniffer/ruleset.xml` and run via
the `phpcodesniffer` Composer script:

```bash
composer phpcodesniffer
```

This runs `phpcs` against `src/` and `tests/` using the project ruleset. The
`src/` run suppresses warnings (`-n`); the `tests/` run additionally shows
sniff source codes (`-n -s`).

To see all warnings (not just errors) run:

```bash
cd .github/ci/phpcodesniffer && composer warnings
```

## Ruleset

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

Enforces that each type (class, interface, trait, enum) is declared in a file
whose name matches the type name, rooted at the configured namespaces:

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
