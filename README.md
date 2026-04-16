<p align="center"><a href="https://valkyrja.io" target="_blank">
    <img src="https://raw.githubusercontent.com/valkyrjaio/art/refs/heads/master/full-logo/orange/php.png" width="400">
</a></p>

# Valkyrja PHP Code Sniffer

PHP Code Sniffer rules for the Valkyrja project.

## Requirements

- PHP >= 8.4
- [`squizlabs/php_codesniffer`](https://github.com/squizlabs/PHP_CodeSniffer)
  ^4.0.1
- [`slevomat/coding-standard`](https://github.com/slevomat/coding-standard)
  ^8.28.1

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
