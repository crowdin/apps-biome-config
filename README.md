# @crowdin/apps-biome-config

Shared [Biome](https://biomejs.dev) configuration for Crowdin apps.

## Usage

```sh
npm install --save-dev @crowdin/apps-biome-config
```

Biome is a pinned peer dependency and installs with this package. Do not add `@biomejs/biome` separately.

`biome.json`:

```json
{
  "$schema": "./node_modules/@crowdin/apps-biome-config/schema.json",
  "extends": ["@crowdin/apps-biome-config/biome"]
}
```

`package.json` scripts:

```json
{
  "lint": "biome check --error-on-warnings",
  "lint-fix": "biome check --write",
  "prepare": "husky || true"
}
```

In CI, run `biome ci --error-on-warnings`: the same check as `lint`.

### Pre-commit hook

Formats and fixes staged files on every commit, also when an AI agent makes the commit.

```sh
npm install --save-dev husky
```

`.husky/pre-commit`:

```sh
biome check --write --staged --no-errors-on-unmatched
git update-index --again
```

`|| true` in `prepare` keeps installs without dev dependencies, such as production Docker images, from failing.

## Settings

Only settings that differ from Biome's defaults. Each one needs a reason here.

| Setting                                | Value    | Biome default | Why                                                                                            |
| -------------------------------------- | -------- | ------------- | ---------------------------------------------------------------------------------------------- |
| `vcs.useIgnoreFile`                    | `true`   | `false`       | Skips what `.gitignore` skips.                                                                 |
| `files.ignoreUnknown`                  | `true`   | `false`       | No errors on unsupported file types.                                                           |
| `formatter.indentStyle`                | `space`  | `tab`         | Same look in every editor and review tool.                                                     |
| `formatter.lineWidth`                  | 120      | 80            | Room for TypeScript signatures, still fits side-by-side diffs.                                 |
| `javascript.formatter.quoteStyle`      | `single` | `double`      | No escaping in strings that contain HTML, JSON or quoted text.                                 |
| `linter.rules.style.useThrowOnlyError` | `error`  | off           | Throwing a string or object loses the stack trace.                                             |
| `yaml.formatter.enabled`               | `false`  | `true`        | Helm chart templates are not valid YAML until rendered; formatting breaks `{{ }}` expressions. |
| `css.parser.tailwindDirectives`        | `true`   | `false`       | Parses Tailwind at-rules such as `@theme` and `@apply`.                                        |
| `--error-on-warnings` (CI)             | on       | off           | Warnings that never fail the build get ignored.                                                |

## License

[MIT](LICENSE)
